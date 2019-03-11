<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4f8ca-101">Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo PHP.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following command to create a new PHP app.</span></span>

```Shell
laravel new graph-tutorial
```

<span data-ttu-id="4f8ca-102">Laravel cria um novo diretório chamado `graph-tutorial` e estruturará um aplicativo PHP.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-102">Laravel creates a new directory called `graph-tutorial` and scaffolds a PHP app.</span></span> <span data-ttu-id="4f8ca-103">Navegue até este novo diretório e digite o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-103">Navigate to this new directory and enter the following command to start a local web server.</span></span>

```Shell
php artisan serve
```

<span data-ttu-id="4f8ca-104">Abra o navegador e vá até `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-104">Open your browser and navigate to `http://localhost:8000`.</span></span> <span data-ttu-id="4f8ca-105">Se tudo estiver funcionando, você verá uma página padrão do Laravel.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-105">If everything is working, you will see a default Laravel page.</span></span> <span data-ttu-id="4f8ca-106">Se você não vir essa página, verifique os [documentos do Laravel](https://laravel.com/docs/5.6).</span><span class="sxs-lookup"><span data-stu-id="4f8ca-106">If you don't see that page, check the [Laravel docs](https://laravel.com/docs/5.6).</span></span>

<span data-ttu-id="4f8ca-107">Antes de prosseguir, instale algumas bibliotecas adicionais que serão usadas posteriormente:</span><span class="sxs-lookup"><span data-stu-id="4f8ca-107">Before moving on, install some additional libraries that you will use later:</span></span>

- <span data-ttu-id="4f8ca-108">[oauth2-Client](https://github.com/thephpleague/oauth2-client) para manipulação de entrada e fluxos de token OAuth.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-108">[oauth2-client](https://github.com/thephpleague/oauth2-client) for handling sign-in and OAuth token flows.</span></span>
- <span data-ttu-id="4f8ca-109">[Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-109">[microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="4f8ca-110">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-110">Run the following command in your CLI.</span></span>

```Shell
composer require league/oauth2-client:dev-master microsoft/microsoft-graph
```

## <a name="design-the-app"></a><span data-ttu-id="4f8ca-111">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4f8ca-111">Design the app</span></span>

<span data-ttu-id="4f8ca-112">Comece criando o layout global para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-112">Start by creating the global layout for the app.</span></span> <span data-ttu-id="4f8ca-113">Crie um novo arquivo no `./resources/views` diretório chamado `layout.blade.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-113">Create a new file in the  `./resources/views` directory named `layout.blade.php` and add the following code.</span></span>

```php
<!DOCTYPE html>
<html>
  <head>
    <title>PHP Graph Tutorial</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css"
        integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
        integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link rel="stylesheet" href="{{ asset('/css/app.css') }}">
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
        integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"
        integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
  </head>

  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container">
        <a href="/" class="navbar-brand">PHP Graph Tutorial</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse"
            aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a href="/" class="nav-link {{$_SERVER['REQUEST_URI'] == '/' ? ' active' : ''}}">Home</a>
            </li>
            @if(isset($userName))
              <li class="nav-item" data-turbolinks="false">
                <a href="/calendar" class="nav-link{{$_SERVER['REQUEST_URI'] == '/calendar' ? ' active' : ''}}">Calendar</a>
              </li>
            @endif
          </ul>
          <ul class="navbar-nav justify-content-end">
            <li class="nav-item">
              <a class="nav-link" href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                <i class="fas fa-external-link-alt mr-1"></i>Docs
              </a>
            </li>
            @if(isset($userName))
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button"
                  aria-haspopup="true" aria-expanded="false">
                  @if(isset($user_avatar))
                    <img src="{{ $user_avatar }}" class="rounded-circle align-self-center mr-2" style="width: 32px;">
                  @else
                    <i class="far fa-user-circle fa-lg rounded-circle align-self-center mr-2" style="width: 32px;"></i>
                  @endif
                </a>
                <div class="dropdown-menu dropdown-menu-right">
                  <h5 class="dropdown-item-text mb-0">{{ $userName }}</h5>
                  <p class="dropdown-item-text text-muted mb-0">{{ $userEmail }}</p>
                  <div class="dropdown-divider"></div>
                  <a href="/signout" class="dropdown-item">Sign Out</a>
                </div>
              </li>
            @else
              <li class="nav-item">
                <a href="/signin" class="nav-link">Sign In</a>
              </li>
            @endif
          </ul>
        </div>
      </div>
    </nav>
    <main role="main" class="container">
      @if(session('error'))
        <div class="alert alert-danger" role="alert">
          <p class="mb-3">{{ session('error') }}</p>
          @if(session('errorDetail'))
            <pre class="alert-pre border bg-light p-2"><code>{{ session('errorDetail') }}</code></pre>
          @endif
        </div>
      @endif

      @yield('content')
    </main>
  </body>
</html>
```

<span data-ttu-id="4f8ca-114">Este código adiciona a [inicialização](http://getbootstrap.com/) para estilos simples e a [fonte incrível](https://fontawesome.com/) para alguns ícones simples.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-114">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="4f8ca-115">Também define um layout global com uma barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-115">It also defines a global layout with a nav bar.</span></span>

<span data-ttu-id="4f8ca-116">Agora, `./public/css/app.css` abra e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-116">Now open `./public/css/app.css` and replace its entire contents with the following.</span></span>

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="4f8ca-117">Agora, atualize a página padrão.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-117">Now update the default page.</span></span> <span data-ttu-id="4f8ca-118">Abra o `./resources/views/welcome.blade.php` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-118">Open the `./resources/views/welcome.blade.php` file and replace its contents with the following.</span></span>

```php
@extends('layout')

@section('content')
<div class="jumbotron">
  <h1>PHP Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from PHP</p>
  @if(isset($userName))
    <h4>Welcome {{ $userName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  @else
    <a href="/signin" class="btn btn-primary btn-large">Click here to sign in</a>
  @endif
</div>
@endsection
```

<span data-ttu-id="4f8ca-119">Atualize a classe `Controller` base no `./app/Http/Controllers/Controller.php` adicionando a função a seguir à classe.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-119">Update the base `Controller` class in `./app/Http/Controllers/Controller.php` by adding the following function to the class.</span></span>

```php
public function loadViewData()
{
  $viewData = [];

  // Check for flash errors
  if (session('error')) {
    $viewData['error'] = session('error');
    $viewData['errorDetail'] = session('errorDetail');
  }

  // Check for logged on user
  if (session('userName'))
  {
    $viewData['userName'] = session('userName');
    $viewData['userEmail'] = session('userEmail');
  }

  return $viewData;
}
```

<span data-ttu-id="4f8ca-120">Em seguida, adicione um controlador para a Home Page.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-120">Next, add a controller for the home page.</span></span> <span data-ttu-id="4f8ca-121">Crie um novo arquivo no `./app/Http/Controllers` diretório chamado `HomeController.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-121">Create a new file in the `./app/Http/Controllers` directory named `HomeController.php` and add the following code.</span></span>

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class HomeController extends Controller
{
  public function welcome()
  {
    $viewData = $this->loadViewData();

    return view('welcome', $viewData);
  }
}
```

<span data-ttu-id="4f8ca-122">Por fim, atualize a rota `./routes/web.php` para usar o novo controlador.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-122">Finally, update the route in `./routes/web.php` to use the new controller.</span></span> <span data-ttu-id="4f8ca-123">Substitua todo o conteúdo deste arquivo com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-123">Replace the entire contents of this file with the following.</span></span>

```php
<?php

Route::get('/', 'HomeController@welcome');
```

<span data-ttu-id="4f8ca-124">Salve todas as suas alterações e reinicie o servidor.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-124">Save all of your changes and restart the server.</span></span> <span data-ttu-id="4f8ca-125">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="4f8ca-125">Now, the app should look very different.</span></span>

![Uma captura de tela da página inicial reprojetada](./images/create-app-01.png)