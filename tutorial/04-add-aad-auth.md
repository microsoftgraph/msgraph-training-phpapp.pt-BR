<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0ea1e-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="0ea1e-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="0ea1e-103">Nesta etapa, você integrará a [biblioteca oauth2-client](https://github.com/thephpleague/oauth2-client) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-103">In this step you will integrate the [oauth2-client](https://github.com/thephpleague/oauth2-client) library into the application.</span></span>

1. <span data-ttu-id="0ea1e-104">Abra o **arquivo .env** na raiz do seu aplicativo PHP e adicione o seguinte código ao final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-104">Open the **.env** file in the root of your PHP application, and add the following code to the end of the file.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env" range="51-57":::

1. <span data-ttu-id="0ea1e-105">Substitua pela ID do aplicativo do Portal de Registro de Aplicativos e substitua `YOUR_APP_ID_HERE` `YOUR_APP_SECRET_HERE` pela senha gerada.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the password you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0ea1e-106">Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo do controle de código-fonte para evitar o vazamento inadvertida de sua ID de aplicativo `.env` e senha.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="0ea1e-107">Crie um novo arquivo na pasta **./config** nomeado `azure.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-107">Create a new file in the **./config** folder named `azure.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/config/azure.php":::

## <a name="implement-sign-in"></a><span data-ttu-id="0ea1e-108">Implementar a login</span><span class="sxs-lookup"><span data-stu-id="0ea1e-108">Implement sign-in</span></span>

1. <span data-ttu-id="0ea1e-109">Crie um novo arquivo no **diretório ./app/Http/Controllers** nomeado `AuthController.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-109">Create a new file in the **./app/Http/Controllers** directory named `AuthController.php` and add the following code.</span></span>

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

    <span data-ttu-id="0ea1e-110">Isso define um controlador com duas ações: `signin` e `callback` .</span><span class="sxs-lookup"><span data-stu-id="0ea1e-110">This defines a controller with two actions: `signin` and `callback`.</span></span>

    <span data-ttu-id="0ea1e-111">A ação gera a URL de entrada do Azure AD, salva o valor gerado pelo cliente OAuth e redireciona o navegador para a página de entrada do `signin` `state` Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-111">The `signin` action generates the Azure AD signin URL, saves the `state` value generated by the OAuth client, then redirects the browser to the Azure AD signin page.</span></span>

    <span data-ttu-id="0ea1e-112">A `callback` ação é onde o Azure redireciona após a conclusão da assinatura.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-112">The `callback` action is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="0ea1e-113">Essa ação garante que o valor corresponde ao valor salvo e, em seguida, os usuários o código de autorização enviado pelo `state` Azure para solicitar um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-113">That action makes sure the `state` value matches the saved value, then users the authorization code sent by Azure to request an access token.</span></span> <span data-ttu-id="0ea1e-114">Em seguida, redireciona de volta para a home page com o token de acesso no valor de erro temporário.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-114">It then redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="0ea1e-115">Você usará isso para verificar se a login está funcionando antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-115">You'll use this to verify that sign-in is working before moving on.</span></span>

1. <span data-ttu-id="0ea1e-116">Adicione as rotas a **./routes/web.php**.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-116">Add the routes to **./routes/web.php**.</span></span>

    ```php
    Route::get('/signin', 'AuthController@signin');
    Route::get('/callback', 'AuthController@callback');
    ```

1. <span data-ttu-id="0ea1e-117">Inicie o servidor e navegue até `https://localhost:8000` .</span><span class="sxs-lookup"><span data-stu-id="0ea1e-117">Start the server and browse to `https://localhost:8000`.</span></span> <span data-ttu-id="0ea1e-118">Clique no botão de login e você deve ser redirecionado `https://login.microsoftonline.com` para.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-118">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="0ea1e-119">Faça logon com sua conta da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-119">Login with your Microsoft account.</span></span>

1. <span data-ttu-id="0ea1e-120">Examine a solicitação de consentimento.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-120">Examine the consent prompt.</span></span> <span data-ttu-id="0ea1e-121">A lista de permissões corresponde à lista de escopos de permissões configurados em **.env**.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-121">The list of permissions correspond to list of permissions scopes configured in **.env**.</span></span>

    - <span data-ttu-id="0ea1e-122">**Mantenha acesso aos dados aos** que você deu a eles acesso: ( ) Essa permissão é solicitada pela MSAL para recuperar `offline_access` tokens de atualização.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-122">**Maintain access to data you have given it access to:** (`offline_access`) This permission is requested by MSAL in order to retrieve refresh tokens.</span></span>
    - <span data-ttu-id="0ea1e-123">**Entre e leia seu perfil:** ( ) Essa permissão permite que o aplicativo receba o perfil do usuário conectado e a foto `User.Read` do perfil.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-123">**Sign you in and read your profile:** (`User.Read`) This permission allows the app to get the logged-in user's profile and profile photo.</span></span>
    - <span data-ttu-id="0ea1e-124">**Leia as configurações da** caixa de correio: ( ) Essa permissão permite que o aplicativo leia as configurações da caixa de correio do usuário, incluindo o fuso horário e o `MailboxSettings.Read` formato de hora.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-124">**Read your mailbox settings:** (`MailboxSettings.Read`) This permission allows the app to read the user's mailbox settings, including time zone and time format.</span></span>
    - <span data-ttu-id="0ea1e-125">Tenha acesso total aos seus **calendários:** ( ) Essa permissão permite que o aplicativo leia eventos no calendário do usuário, adicione novos eventos e modifique `Calendars.ReadWrite` os existentes.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-125">**Have full access to your calendars:** (`Calendars.ReadWrite`) This permission allows the app to read events on the user's calendar, add new events, and modify existing ones.</span></span>

1. <span data-ttu-id="0ea1e-126">Consentir com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-126">Consent to the requested permissions.</span></span> <span data-ttu-id="0ea1e-127">O navegador redireciona para o aplicativo, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-127">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="0ea1e-128">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="0ea1e-128">Get user details</span></span>

<span data-ttu-id="0ea1e-129">Nesta seção, você atualizará o `callback` método para obter o perfil do usuário do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-129">In this section you'll update the `callback` method to get the user's profile from Microsoft Graph.</span></span>

1. <span data-ttu-id="0ea1e-130">Adicione as instruções `use` a seguir na parte superior de **/app/Http/Controllers/AuthController.php**, abaixo da `namespace App\Http\Controllers;` linha.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-130">Add the following `use` statements to the top of **/app/Http/Controllers/AuthController.php**, beneath the `namespace App\Http\Controllers;` line.</span></span>

    ```php
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    ```

1. <span data-ttu-id="0ea1e-131">Substitua o `try` bloco no método pelo código a `callback` seguir.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-131">Replace the `try` block in the `callback` method with the following code.</span></span>

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

<span data-ttu-id="0ea1e-132">O novo código cria um objeto, atribui o token de acesso e o usa `Graph` para solicitar o perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-132">The new code creates a `Graph` object, assigns the access token, then uses it to request the user's profile.</span></span> <span data-ttu-id="0ea1e-133">Ele adiciona o nome de exibição do usuário à saída temporária para teste.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-133">It adds the user's display name to the temporary output for testing.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="0ea1e-134">Armazenar os tokens</span><span class="sxs-lookup"><span data-stu-id="0ea1e-134">Storing the tokens</span></span>

<span data-ttu-id="0ea1e-135">Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-135">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="0ea1e-136">Como este é um aplicativo de exemplo, por questão de simplicidade, você os armazenará na sessão.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-136">Since this is a sample app, for simplicity's sake, you'll store them in the session.</span></span> <span data-ttu-id="0ea1e-137">Um aplicativo real usaria uma solução de armazenamento seguro mais confiável, como um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-137">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

1. <span data-ttu-id="0ea1e-138">Crie um novo diretório no diretório **./app** chamado , em seguida, crie um novo arquivo nesse diretório nomeado `TokenStore` e adicione o código a `TokenCache.php` seguir.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-138">Create a new directory in the **./app** directory named `TokenStore`, then create a new file in that directory named `TokenCache.php`, and add the following code.</span></span>

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

1. <span data-ttu-id="0ea1e-139">Adicione a instrução a seguir na parte superior `use` **de ./app/Http/Controllers/AuthController.php**, abaixo da `namespace App\Http\Controllers;` linha.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-139">Add the following `use` statement to the top of **./app/Http/Controllers/AuthController.php**, beneath the `namespace App\Http\Controllers;` line.</span></span>

    ```php
    use App\TokenStore\TokenCache;
    ```

1. <span data-ttu-id="0ea1e-140">Substitua o `try` bloco na função existente pelo `callback` seguinte.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-140">Replace the `try` block in the existing `callback` function with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="StoreTokensSnippet":::

## <a name="implement-sign-out"></a><span data-ttu-id="0ea1e-141">Implementar a saída</span><span class="sxs-lookup"><span data-stu-id="0ea1e-141">Implement sign-out</span></span>

<span data-ttu-id="0ea1e-142">Antes de testar esse novo recurso, adicione uma maneira de sair.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-142">Before you test this new feature, add a way to sign out.</span></span>

1. <span data-ttu-id="0ea1e-143">Adicione a ação a seguir à `AuthController` classe.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-143">Add the following action to the `AuthController` class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="SignOutSnippet":::

1. <span data-ttu-id="0ea1e-144">Adicione esta ação a **./routes/web.php**.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-144">Add this action to **./routes/web.php**.</span></span>

    ```php
    Route::get('/signout', 'AuthController@signout');
    ```

1. <span data-ttu-id="0ea1e-145">Reinicie o servidor e vá pelo processo de login.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-145">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="0ea1e-146">Você deve voltar à home page, mas a interface do usuário deve mudar para indicar que você está entrar.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-146">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Uma captura de tela da home page após entrar](./images/add-aad-auth-01.png)

1. <span data-ttu-id="0ea1e-148">Clique no avatar do usuário no canto superior direito para acessar o link **Sair.**</span><span class="sxs-lookup"><span data-stu-id="0ea1e-148">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="0ea1e-149">Clicar em **Sair** redefine a sessão e o retorna para a home page.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-149">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="0ea1e-151">Atualização de tokens</span><span class="sxs-lookup"><span data-stu-id="0ea1e-151">Refreshing tokens</span></span>

<span data-ttu-id="0ea1e-152">Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-152">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="0ea1e-153">Esse é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-153">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="0ea1e-154">No entanto, esse token tem curta duração.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-154">However, this token is short-lived.</span></span> <span data-ttu-id="0ea1e-155">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-155">The token expires an hour after it is issued.</span></span> <span data-ttu-id="0ea1e-156">É aqui que o token de atualização se torna útil.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-156">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="0ea1e-157">O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-157">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span> <span data-ttu-id="0ea1e-158">Atualize o código de gerenciamento de token para implementar a atualização de token.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-158">Update the token management code to implement token refresh.</span></span>

1. <span data-ttu-id="0ea1e-159">Abra **./app/TokenStore/TokenCache.php** e adicione a função a seguir à `TokenCache` classe.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-159">Open **./app/TokenStore/TokenCache.php** and add the following function to the `TokenCache` class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="UpdateTokensSnippet":::

1. <span data-ttu-id="0ea1e-160">Substitua a função `getAccessToken` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-160">Replace the existing `getAccessToken` function with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="GetAccessTokenSnippet":::

<span data-ttu-id="0ea1e-161">Este método primeiro verifica se o token de acesso expirou ou está próximo de expirar.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-161">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="0ea1e-162">Se estiver, usa o token de atualização para obter novos tokens, atualiza o cache e retorna o novo token de acesso.</span><span class="sxs-lookup"><span data-stu-id="0ea1e-162">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span>
