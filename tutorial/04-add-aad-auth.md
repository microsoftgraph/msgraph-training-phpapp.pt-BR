<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca oauth2-client](https://github.com/thephpleague/oauth2-client) ao aplicativo.

1. Abra o **arquivo .env** na raiz do seu aplicativo PHP e adicione o seguinte código ao final do arquivo.

    :::code language="ini" source="../demo/graph-tutorial/example.env" range="51-57":::

1. Substitua pela ID do aplicativo do Portal de Registro de Aplicativos e substitua `YOUR_APP_ID_HERE` `YOUR_APP_SECRET_HERE` pela senha gerada.

    > [!IMPORTANT]
    > Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo do controle de código-fonte para evitar o vazamento inadvertida de sua ID de aplicativo `.env` e senha.

1. Crie um novo arquivo na pasta **./config** nomeado `azure.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/config/azure.php":::

## <a name="implement-sign-in"></a>Implementar a login

1. Crie um novo arquivo no **diretório ./app/Http/Controllers** nomeado `AuthController.php` e adicione o código a seguir.

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;

    class AuthController extends Controller
    {
      public function signin()
      {
        // Initialize the OAuth client
        $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
          'clientId'                => config('azure.appId'),
          'clientSecret'            => config('azure.appSecret'),
          'redirectUri'             => config('azure.redirectUri'),
          'urlAuthorize'            => config('azure.authority').config('azure.authorizeEndpoint'),
          'urlAccessToken'          => config('azure.authority').config('azure.tokenEndpoint'),
          'urlResourceOwnerDetails' => '',
          'scopes'                  => config('azure.scopes')
        ]);

        $authUrl = $oauthClient->getAuthorizationUrl();

        // Save client state so we can validate in callback
        session(['oauthState' => $oauthClient->getState()]);

        // Redirect to AAD signin page
        return redirect()->away($authUrl);
      }

      public function callback(Request $request)
      {
        // Validate state
        $expectedState = session('oauthState');
        $request->session()->forget('oauthState');
        $providedState = $request->query('state');

        if (!isset($expectedState)) {
          // If there is no expected state in the session,
          // do nothing and redirect to the home page.
          return redirect('/');
        }

        if (!isset($providedState) || $expectedState != $providedState) {
          return redirect('/')
            ->with('error', 'Invalid auth state')
            ->with('errorDetail', 'The provided auth state did not match the expected value');
        }

        // Authorization code should be in the "code" query param
        $authCode = $request->query('code');
        if (isset($authCode)) {
          // Initialize the OAuth client
          $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
            'clientId'                => config('azure.appId'),
            'clientSecret'            => config('azure.appSecret'),
            'redirectUri'             => config('azure.redirectUri'),
            'urlAuthorize'            => config('azure.authority').config('azure.authorizeEndpoint'),
            'urlAccessToken'          => config('azure.authority').config('azure.tokenEndpoint'),
            'urlResourceOwnerDetails' => '',
            'scopes'                  => config('azure.scopes')
          ]);

          try {
            // Make the token request
            $accessToken = $oauthClient->getAccessToken('authorization_code', [
              'code' => $authCode
            ]);

            // TEMPORARY FOR TESTING!
            return redirect('/')
              ->with('error', 'Access token received')
              ->with('errorDetail', $accessToken->getToken());
          }
          catch (League\OAuth2\Client\Provider\Exception\IdentityProviderException $e) {
            return redirect('/')
              ->with('error', 'Error requesting access token')
              ->with('errorDetail', $e->getMessage());
          }
        }

        return redirect('/')
          ->with('error', $request->query('error'))
          ->with('errorDetail', $request->query('error_description'));
      }
    }
    ```

    Isso define um controlador com duas ações: `signin` e `callback` .

    A ação gera a URL de entrada do Azure AD, salva o valor gerado pelo cliente OAuth e redireciona o navegador para a página de entrada do `signin` `state` Azure AD.

    A `callback` ação é onde o Azure redireciona após a conclusão da assinatura. Essa ação garante que o valor corresponde ao valor salvo e, em seguida, os usuários o código de autorização enviado pelo `state` Azure para solicitar um token de acesso. Em seguida, redireciona de volta para a home page com o token de acesso no valor de erro temporário. Você usará isso para verificar se a login está funcionando antes de continuar.

1. Adicione as rotas a **./routes/web.php**.

    ```php
    Route::get('/signin', 'AuthController@signin');
    Route::get('/callback', 'AuthController@callback');
    ```

1. Inicie o servidor e navegue até `https://localhost:8000` . Clique no botão de login e você deve ser redirecionado `https://login.microsoftonline.com` para. Faça logon com sua conta da Microsoft.

1. Examine a solicitação de consentimento. A lista de permissões corresponde à lista de escopos de permissões configurados em **.env**.

    - **Mantenha acesso aos dados aos** que você deu a eles acesso: ( ) Essa permissão é solicitada pela MSAL para recuperar `offline_access` tokens de atualização.
    - **Entre e leia seu perfil:** ( ) Essa permissão permite que o aplicativo receba o perfil do usuário conectado e a foto `User.Read` do perfil.
    - **Leia as configurações da** caixa de correio: ( ) Essa permissão permite que o aplicativo leia as configurações da caixa de correio do usuário, incluindo o fuso horário e o `MailboxSettings.Read` formato de hora.
    - Tenha acesso total aos seus **calendários:** ( ) Essa permissão permite que o aplicativo leia eventos no calendário do usuário, adicione novos eventos e modifique `Calendars.ReadWrite` os existentes.

1. Consentir com as permissões solicitadas. O navegador redireciona para o aplicativo, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

Nesta seção, você atualizará o `callback` método para obter o perfil do usuário do Microsoft Graph.

1. Adicione as instruções `use` a seguir na parte superior de **/app/Http/Controllers/AuthController.php**, abaixo da `namespace App\Http\Controllers;` linha.

    ```php
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    ```

1. Substitua o `try` bloco no método pelo código a `callback` seguir.

    ```php
    try {
      // Make the token request
      $accessToken = $oauthClient->getAccessToken('authorization_code', [
        'code' => $authCode
      ]);

      $graph = new Graph();
      $graph->setAccessToken($accessToken->getToken());

      $user = $graph->createRequest('GET', '/me?$select=displayName,mail,mailboxSettings,userPrincipalName')
        ->setReturnType(Model\User::class)
        ->execute();

      // TEMPORARY FOR TESTING!
      return redirect('/')
        ->with('error', 'Access token received')
        ->with('errorDetail', 'User:'.$user->getDisplayName().', Token:'.$accessToken->getToken());
    }
    ```

O novo código cria um objeto, atribui o token de acesso e o usa `Graph` para solicitar o perfil do usuário. Ele adiciona o nome de exibição do usuário à saída temporária para teste.

## <a name="storing-the-tokens"></a>Armazenar os tokens

Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo. Como este é um aplicativo de exemplo, por questão de simplicidade, você os armazenará na sessão. Um aplicativo real usaria uma solução de armazenamento seguro mais confiável, como um banco de dados.

1. Crie um novo diretório no diretório **./app** chamado , em seguida, crie um novo arquivo nesse diretório nomeado `TokenStore` e adicione o código a `TokenCache.php` seguir.

    ```php
    <?php

    namespace App\TokenStore;

    class TokenCache {
      public function storeTokens($accessToken, $user) {
        session([
          'accessToken' => $accessToken->getToken(),
          'refreshToken' => $accessToken->getRefreshToken(),
          'tokenExpires' => $accessToken->getExpires(),
          'userName' => $user->getDisplayName(),
          'userEmail' => null !== $user->getMail() ? $user->getMail() : $user->getUserPrincipalName(),
          'userTimeZone' => $user->getMailboxSettings()->getTimeZone()
        ]);
      }

      public function clearTokens() {
        session()->forget('accessToken');
        session()->forget('refreshToken');
        session()->forget('tokenExpires');
        session()->forget('userName');
        session()->forget('userEmail');
        session()->forget('userTimeZone');
      }

      public function getAccessToken() {
        // Check if tokens exist
        if (empty(session('accessToken')) ||
            empty(session('refreshToken')) ||
            empty(session('tokenExpires'))) {
          return '';
        }

        return session('accessToken');
      }
    }
    ```

1. Adicione a instrução a seguir na parte superior `use` **de ./app/Http/Controllers/AuthController.php**, abaixo da `namespace App\Http\Controllers;` linha.

    ```php
    use App\TokenStore\TokenCache;
    ```

1. Substitua o `try` bloco na função existente pelo `callback` seguinte.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="StoreTokensSnippet":::

## <a name="implement-sign-out"></a>Implementar a saída

Antes de testar esse novo recurso, adicione uma maneira de sair.

1. Adicione a ação a seguir à `AuthController` classe.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="SignOutSnippet":::

1. Adicione esta ação a **./routes/web.php**.

    ```php
    Route::get('/signout', 'AuthController@signout');
    ```

1. Reinicie o servidor e vá pelo processo de login. Você deve voltar à home page, mas a interface do usuário deve mudar para indicar que você está entrar.

    ![Uma captura de tela da home page após entrar](./images/add-aad-auth-01.png)

1. Clique no avatar do usuário no canto superior direito para acessar o link **Sair.** Clicar em **Sair** redefine a sessão e o retorna para a home page.

    ![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>Atualização de tokens

Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas de API. Esse é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token tem curta duração. O token expira uma hora após sua emissão. É aqui que o token de atualização se torna útil. O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente. Atualize o código de gerenciamento de token para implementar a atualização de token.

1. Abra **./app/TokenStore/TokenCache.php** e adicione a função a seguir à `TokenCache` classe.

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="UpdateTokensSnippet":::

1. Substitua a função `getAccessToken` existente pelo seguinte.

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="GetAccessTokenSnippet":::

Este método primeiro verifica se o token de acesso expirou ou está próximo de expirar. Se estiver, usa o token de atualização para obter novos tokens, atualiza o cache e retorna o novo token de acesso.
