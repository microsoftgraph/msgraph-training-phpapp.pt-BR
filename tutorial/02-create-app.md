<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto do Laravel.

1. Abra sua CLI (interface de linha de comando), navegue até um diretório onde você tenha direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo PHP.

    ```Shell
    laravel new graph-tutorial
    ```

1. Navegue até **o diretório de tutorial** do gráfico e insira o seguinte comando para iniciar um servidor Web local.

    ```Shell
    php artisan serve
    ```

1. Abra o navegador e vá até `http://localhost:8000`. Se tudo estiver funcionando, você verá uma página padrão do Laravel. Se você não vir essa página, verifique os documentos [do Laravel.](https://laravel.com/docs/8.x)

## <a name="install-packages"></a>Instalar pacotes

Antes de continuar, instale alguns pacotes adicionais que você usará mais tarde:

- [oauth2-client](https://github.com/thephpleague/oauth2-client) para manipular fluxos de entrada e token OAuth.
- [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    composer require league/oauth2-client microsoft/microsoft-graph
    ```

## <a name="design-the-app"></a>Projetar o aplicativo

1. Crie um novo arquivo no diretório **./resources/views** nomeado `layout.blade.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/resources/views/layout.blade.php" id="LayoutSnippet":::

    Este código adiciona [Bootstrap](http://getbootstrap.com/) para estilo simples e [Font Awesome](https://fontawesome.com/) para alguns ícones simples. Ele também define um layout global com uma barra de inv.

1. Crie um novo diretório no `./public` diretório chamado , em `css` seguida, crie um novo arquivo no diretório `./public/css` chamado `app.css` . Adicione o código a seguir.

    :::code language="css" source="../demo/graph-tutorial/public/css/app.css":::

1. Abra o `./resources/views/welcome.blade.php` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="php" source="../demo/graph-tutorial/resources/views/welcome.blade.php" id="WelcomeSnippet":::

1. Atualize `Controller` a classe base **em ./app/Http/Controllers/Controller.php** adicionando a função a seguir à classe.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/Controller.php" id="LoadViewDataSnippet":::

1. Crie um novo arquivo no `./app/Http/Controllers` diretório nomeado e adicione o código a `HomeController.php` seguir.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/HomeController.php":::

1. Atualize a rota `./routes/web.php` para usar o novo controlador. Substitua todo o conteúdo deste arquivo pelo seguinte.

    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    Route::get('/', 'HomeController@welcome');
    ```

1. Abra **./app/Providers/RouteServiceProvider.php** e descompromente a `$namespace` declaração.

    ```php
    /**
     * This namespace is applied to your controller routes.
     *
     * In addition, it is set as the URL generator's root namespace.
     *
     * @var string
     */
    protected $namespace = 'App\Http\Controllers';
    ```

1. Salve todas as suas alterações e reinicie o servidor. Agora, o aplicativo deve parecer muito diferente.

    ![Uma captura de tela da home page reprojetada](./images/create-app-01.png)
