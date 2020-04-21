<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto do Laravel.

1. Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo PHP.

    ```Shell
    laravel new graph-tutorial
    ```

1. Navegue até o diretório do **tutorial do gráfico** e digite o seguinte comando para iniciar um servidor Web local.

    ```Shell
    php artisan serve
    ```

1. Abra o navegador e vá até `http://localhost:8000`. Se tudo estiver funcionando, você verá uma página padrão do Laravel. Se você não vir essa página, verifique os [documentos do Laravel](https://laravel.com/docs/7.x).

## <a name="install-packages"></a>Instalar pacotes

Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:

- [oauth2-Client](https://github.com/thephpleague/oauth2-client) para manipulação de entrada e fluxos de token OAuth.
- [Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    composer require league/oauth2-client microsoft/microsoft-graph
    ```

## <a name="design-the-app"></a>Projetar o aplicativo

1. Crie um novo arquivo no diretório **./Resources/views** chamado `layout.blade.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/resources/views/layout.blade.php" id="LayoutSnippet":::

    Este código adiciona a [inicialização](http://getbootstrap.com/) para estilos simples e a [fonte incrível](https://fontawesome.com/) para alguns ícones simples. Também define um layout global com uma barra de navegação.

1. Crie um `./public` novo diretório no diretório chamado `css`e, em seguida, crie um novo arquivo `./public/css` no diretório `app.css`chamado. Adicione o código a seguir.

    :::code language="css" source="../demo/graph-tutorial/public/css/app.css":::

1. Abra o `./resources/views/welcome.blade.php` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="php" source="../demo/graph-tutorial/resources/views/welcome.blade.php" id="WelcomeSnippet":::

1. Atualize a classe `Controller` base no **./app/http/Controllers/Controller.php** adicionando a função a seguir à classe.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/Controller.php" id="LoadViewDataSnippet":::

1. Crie um novo arquivo no `./app/Http/Controllers` diretório chamado `HomeController.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/HomeController.php":::

1. Atualize a rota `./routes/web.php` para usar o novo controlador. Substitua todo o conteúdo deste arquivo com o seguinte.

    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    Route::get('/', 'HomeController@welcome');
    ```

1. Salve todas as suas alterações e reinicie o servidor. Agora, o aplicativo deve ser muito diferente.

    ![Uma captura de tela da página inicial reprojetada](./images/create-app-01.png)
