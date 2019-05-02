<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0bd6e-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="0bd6e-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="0bd6e-103">Nesta etapa, você integrará a biblioteca [oauth2-Client](https://github.com/thephpleague/oauth2-client) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-103">In this step you will integrate the [oauth2-client](https://github.com/thephpleague/oauth2-client) library into the application.</span></span>

<span data-ttu-id="0bd6e-104">Abra o `.env` arquivo na raiz do seu aplicativo PHP e adicione o código a seguir ao final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-104">Open the `.env` file in the root of your PHP application, and add the following code to the end of the file.</span></span>

```text
OAUTH_APP_ID=YOUR_APP_ID_HERE
OAUTH_APP_PASSWORD=YOUR_APP_PASSWORD_HERE
OAUTH_REDIRECT_URI=http://localhost:8000/callback
OAUTH_SCOPES='openid profile offline_access user.read calendars.read'
OAUTH_AUTHORITY=https://login.microsoftonline.com/common
OAUTH_AUTHORIZE_ENDPOINT=/oauth2/v2.0/authorize
OAUTH_TOKEN_ENDPOINT=/oauth2/v2.0/token
```

<span data-ttu-id="0bd6e-105">Substitua `YOUR APP ID HERE` pela ID do aplicativo do portal de registro do aplicativo e substitua `YOUR APP SECRET HERE` pela senha gerada.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0bd6e-106">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `.env` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo e sua senha.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="0bd6e-107">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="0bd6e-107">Implement sign-in</span></span>

<span data-ttu-id="0bd6e-108">Crie um novo arquivo no `./app/Http/Controllers` diretório chamado `AuthController.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-108">Create a new file in the `./app/Http/Controllers` directory named `AuthController.php` and add the following code.</span></span>

```PHP
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
      'clientId'                => env('OAUTH_APP_ID'),
      'clientSecret'            => env('OAUTH_APP_PASSWORD'),
      'redirectUri'             => env('OAUTH_REDIRECT_URI'),
      'urlAuthorize'            => env('OAUTH_AUTHORITY').env('OAUTH_AUTHORIZE_ENDPOINT'),
      'urlAccessToken'          => env('OAUTH_AUTHORITY').env('OAUTH_TOKEN_ENDPOINT'),
      'urlResourceOwnerDetails' => '',
      'scopes'                  => env('OAUTH_SCOPES')
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

    if (!isset($expectedState) || !isset($providedState) || $expectedState != $providedState) {
      return redirect('/')
        ->with('error', 'Invalid auth state')
        ->with('errorDetail', 'The provided auth state did not match the expected value');
    }

    // Authorization code should be in the "code" query param
    $authCode = $request->query('code');
    if (isset($authCode)) {
      // Initialize the OAuth client
      $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
        'clientId'                => env('OAUTH_APP_ID'),
        'clientSecret'            => env('OAUTH_APP_PASSWORD'),
        'redirectUri'             => env('OAUTH_REDIRECT_URI'),
        'urlAuthorize'            => env('OAUTH_AUTHORITY').env('OAUTH_AUTHORIZE_ENDPOINT'),
        'urlAccessToken'          => env('OAUTH_AUTHORITY').env('OAUTH_TOKEN_ENDPOINT'),
        'urlResourceOwnerDetails' => '',
        'scopes'                  => env('OAUTH_SCOPES')
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

<span data-ttu-id="0bd6e-109">Isso define um controlador com duas ações: `signin` e `callback`.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-109">This defines a controller with two actions: `signin` and `callback`.</span></span>

<span data-ttu-id="0bd6e-110">A `signin` ação gera a URL de entrada do Azure AD, salva `state` o valor gerado pelo cliente OAuth e redireciona o navegador para a página de entrada do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-110">The `signin` action generates the Azure AD signin URL, saves the `state` value generated by the OAuth client, then redirects the browser to the Azure AD signin page.</span></span>

<span data-ttu-id="0bd6e-111">A `callback` ação é onde o Azure redireciona após a conclusão da entrada.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-111">The `callback` action is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="0bd6e-112">Essa ação garante que o `state` valor corresponde ao valor salvo e, em seguida, os usuários o código de autorização enviado pelo Azure para solicitar um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-112">That action makes sure the `state` value matches the saved value, then users the authorization code sent by Azure to request an access token.</span></span> <span data-ttu-id="0bd6e-113">Em seguida, ele redireciona de volta para a Home Page com o token de acesso no valor de erro temporário.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-113">It then redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="0bd6e-114">Usaremos isso para verificar se a entrada está funcionando antes de prosseguir.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-114">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="0bd6e-115">Antes de testarmos, precisamos adicionar as rotas ao `./routes/web.php`.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-115">Before we test, we need to add the routes to `./routes/web.php`.</span></span>

```PHP
Route::get('/signin', 'AuthController@signin');
Route::get('/callback', 'AuthController@callback');
```

<span data-ttu-id="0bd6e-116">Inicie o servidor e navegue até `https://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-116">Start the server and browse to `https://localhost:8000`.</span></span> <span data-ttu-id="0bd6e-117">Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-117">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="0bd6e-118">Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="0bd6e-119">O navegador redireciona para o aplicativo, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-119">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="0bd6e-120">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="0bd6e-120">Get user details</span></span>

<span data-ttu-id="0bd6e-121">Atualize o `callback` método no `/app/Http/Controllers/AuthController.php` para obter o perfil do usuário do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-121">Update the `callback` method in `/app/Http/Controllers/AuthController.php` to get the user's profile from Microsoft Graph.</span></span>

<span data-ttu-id="0bd6e-122">Primeiro, adicione as seguintes `use` instruções à parte superior do arquivo, abaixo da `namespace App\Http\Controllers;` linha.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-122">First, add the following `use` statements to the top of the file, beneath the `namespace App\Http\Controllers;` line.</span></span>

```php
use Microsoft\Graph\Graph;
use Microsoft\Graph\Model;
```

<span data-ttu-id="0bd6e-123">Substitua o `try` bloco no `callback` método pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-123">Replace the `try` block in the `callback` method with the following code.</span></span>

```php
try {
  // Make the token request
  $accessToken = $oauthClient->getAccessToken('authorization_code', [
    'code' => $authCode
  ]);

  $graph = new Graph();
  $graph->setAccessToken($accessToken->getToken());

  $user = $graph->createRequest('GET', '/me')
    ->setReturnType(Model\User::class)
    ->execute();

  // TEMPORARY FOR TESTING!
  return redirect('/')
    ->with('error', 'Access token received')
    ->with('errorDetail', 'User:'.$user->getDisplayName().', Token:'.$accessToken->getToken());
}
```

<span data-ttu-id="0bd6e-124">O novo código cria um `Graph` objeto, atribui o token de acesso e, em seguida, usa-o para solicitar o perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-124">The new code creates a `Graph` object, assigns the access token, then uses it to request the user's profile.</span></span> <span data-ttu-id="0bd6e-125">Ele adiciona o nome de exibição do usuário à saída temporária para teste.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-125">It adds the user's display name to the temporary output for testing.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="0bd6e-126">Armazenar tokens</span><span class="sxs-lookup"><span data-stu-id="0bd6e-126">Storing the tokens</span></span>

<span data-ttu-id="0bd6e-127">Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-127">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="0bd6e-128">Como este é um aplicativo de exemplo, por questões de simplicidade, você os armazenará na sessão.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-128">Since this is a sample app, for simplicity's sake, you'll store them in the session.</span></span> <span data-ttu-id="0bd6e-129">Um aplicativo real usaria uma solução de armazenamento segura mais confiável, como um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-129">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="0bd6e-130">Crie um novo diretório no `./app` diretório chamado `TokenStore`e, em seguida, crie um novo arquivo no diretório `TokenCache.php`chamado e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-130">Create a new directory in the `./app` directory named `TokenStore`, then create a new file in that directory named `TokenCache.php`, and add the following code.</span></span>

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
      'userEmail' => null !== $user->getMail() ? $user->getMail() : $user->getUserPrincipalName()
    ]);
  }

  public function clearTokens() {
    session()->forget('accessToken');
    session()->forget('refreshToken');
    session()->forget('tokenExpires');
    session()->forget('userName');
    session()->forget('userEmail');
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

<span data-ttu-id="0bd6e-131">Em seguida, atualize `callback` a função na `AuthController` classe para armazenar os tokens na sessão e redirecionar de volta para a página principal.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-131">Then, update the `callback` function in the `AuthController` class to store the tokens in the session and redirect back to the main page.</span></span>

<span data-ttu-id="0bd6e-132">Primeiro, adicione a seguinte `use` instrução à parte superior de `./app/Http/Controllers/AuthController.php`, abaixo da `namespace App\Http\Controllers;` linha.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-132">First, add the following `use` statement to the top of `./app/Http/Controllers/AuthController.php`, beneath the `namespace App\Http\Controllers;` line.</span></span>

```php
use App\TokenStore\TokenCache;
```

<span data-ttu-id="0bd6e-133">Em seguida, `try` substitua o bloco na `callback` função existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-133">Then replace the `try` block in the existing `callback` function with the following.</span></span>

```php
try {
  // Make the token request
  $accessToken = $oauthClient->getAccessToken('authorization_code', [
    'code' => $authCode
  ]);

  $graph = new Graph();
  $graph->setAccessToken($accessToken->getToken());

  $user = $graph->createRequest('GET', '/me')
    ->setReturnType(Model\User::class)
    ->execute();

  $tokenCache = new TokenCache();
  $tokenCache->storeTokens($accessToken, $user);

  return redirect('/');
}
```

## <a name="implement-sign-out"></a><span data-ttu-id="0bd6e-134">Implementar a saída</span><span class="sxs-lookup"><span data-stu-id="0bd6e-134">Implement sign-out</span></span>

<span data-ttu-id="0bd6e-135">Antes de testar esse novo recurso, adicione uma maneira de sair. Adicione a ação a seguir à `AuthController` classe.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-135">Before you test this new feature, add a way to sign out. Add the following action to the `AuthController` class.</span></span>

```PHP
public function signout()
{
  $tokenCache = new TokenCache();
  $tokenCache->clearTokens();
  return redirect('/');
}
```

<span data-ttu-id="0bd6e-136">Adicione esta ação a `./routes/web.php`.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-136">Add this action to `./routes/web.php`.</span></span>

```PHP
Route::get('/signout', 'AuthController@signout');
```

<span data-ttu-id="0bd6e-137">Reinicie o servidor e vá pelo processo de entrada.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-137">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="0bd6e-138">Você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-138">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

<span data-ttu-id="0bd6e-140">Clique no avatar do usuário no canto superior direito para acessar o \*\*\*\* link sair.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-140">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="0bd6e-141">Clicar \*\*\*\* em sair redefine a sessão e retorna à Home Page.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-141">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="0bd6e-143">Atualizando tokens</span><span class="sxs-lookup"><span data-stu-id="0bd6e-143">Refreshing tokens</span></span>

<span data-ttu-id="0bd6e-144">Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-144">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="0bd6e-145">Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-145">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="0bd6e-146">No entanto, esse token é de vida curta.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-146">However, this token is short-lived.</span></span> <span data-ttu-id="0bd6e-147">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-147">The token expires an hour after it is issued.</span></span> <span data-ttu-id="0bd6e-148">É onde o token de atualização se torna útil.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-148">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="0bd6e-149">O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-149">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span> <span data-ttu-id="0bd6e-150">Atualize o código de gerenciamento de token para implementar a atualização de token.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-150">Update the token management code to implement token refresh.</span></span>

<span data-ttu-id="0bd6e-151">Abra `./app/TokenStore/TokenCache.php` e adicione a função a seguir à `TokenCache` classe.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-151">Open `./app/TokenStore/TokenCache.php` and add the following function to the `TokenCache` class.</span></span>

```php
public function updateTokens($accessToken) {
  session([
    'accessToken' => $accessToken->getToken(),
    'refreshToken' => $accessToken->getRefreshToken(),
    'tokenExpires' => $accessToken->getExpires()
  ]);
}
```

<span data-ttu-id="0bd6e-152">Em seguida, substitua `getAccessToken` a função existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-152">Then replace the existing `getAccessToken` function with the following.</span></span>

```php
public function getAccessToken() {
  // Check if tokens exist
  if (empty(session('accessToken')) ||
      empty(session('refreshToken')) ||
      empty(session('tokenExpires'))) {
    return '';
  }

  // Check if token is expired
  //Get current time + 5 minutes (to allow for time differences)
  $now = time() + 300;
  if (session('tokenExpires') <= $now) {
    // Token is expired (or very close to it)
    // so let's refresh

    // Initialize the OAuth client
    $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
      'clientId'                => env('OAUTH_APP_ID'),
      'clientSecret'            => env('OAUTH_APP_PASSWORD'),
      'redirectUri'             => env('OAUTH_REDIRECT_URI'),
      'urlAuthorize'            => env('OAUTH_AUTHORITY').env('OAUTH_AUTHORIZE_ENDPOINT'),
      'urlAccessToken'          => env('OAUTH_AUTHORITY').env('OAUTH_TOKEN_ENDPOINT'),
      'urlResourceOwnerDetails' => '',
      'scopes'                  => env('OAUTH_SCOPES')
    ]);

    try {
      $newToken = $oauthClient->getAccessToken('refresh_token', [
        'refresh_token' => session('refreshToken')
      ]);

      // Store the new values
      $this->updateTokens($newToken);

      return $newToken->getToken();
    }
    catch (League\OAuth2\Client\Provider\Exception\IdentityProviderException $e) {
      return '';
    }
  }

  // Token is still valid, just return it
  return session('accessToken');
}
```

<span data-ttu-id="0bd6e-153">Este método primeiro verifica se o token de acesso expirou ou está prestes a expirar.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-153">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="0bd6e-154">Se for, ele usará o token de atualização para obter novos tokens, atualizará o cache e retornará o novo token de acesso.</span><span class="sxs-lookup"><span data-stu-id="0bd6e-154">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span>