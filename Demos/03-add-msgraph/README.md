# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nessa pasta, você precisará do seguinte:

- [Php](http://php.net/downloads.php) instalado em sua máquina de desenvolvimento. Se você não tiver o PHP, visite o link anterior para opções de download. (**Observação:** este tutorial foi escrito com o PHP versão 7,2. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.
- [Composer](https://getcomposer.org/) instalado em sua máquina de desenvolvimento.
- [Laravel](https://laravel.com/) instalado em sua máquina de desenvolvimento.
- Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.

Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo Web com o centro de administração do Azure Active Directory

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.

1. Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **novo registro**. Na página **registrar um aplicativo** , defina os valores da seguinte maneira.

    - Defina **** o nome `PHP Graph Tutorial`como.
    - Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:8000/callback`valor como.

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. Escolha **registrar**. Na página **tutorial do gráfico do PHP** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. Selecione **certificados & segredos** sob **gerenciar**. Selecione o botão **novo cliente secreto** . Insira um valor em **Descrição** e selecione uma das opções para **expirar** e escolha **Adicionar**.

    ![Uma captura de tela da caixa de diálogo Adicionar um segredo do cliente](/tutorial/images/aad-new-client-secret.png)

1. Copie o valor de segredo do cliente antes de sair desta página. Você precisará dela na próxima etapa.

    > [!IMPORTANT]
    > Esse segredo do cliente nunca é mostrado novamente, portanto, certifique-se de copiá-lo agora.

    ![Uma captura de tela do novo segredo do cliente recentemente adicionado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. ReNomear `.env.example` o `.env`arquivo para.
1. Edite `.env` o arquivo e faça as seguintes alterações.
    1. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.
    1. Substitua `YOUR_APP_PASSWORD_HERE` pela senha obtida do portal de registro do aplicativo.
1. Na sua interface de linha de comando (CLI), navegue até este diretório e execute o seguinte comando para instalar os requisitos.

    ```Shell
    composer install
    ```

1. Na sua interface de linha de comando (CLI), execute o seguinte comando para gerar uma chave de aplicativo.

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a>Executar o exemplo

1. Execute o comando a seguir em sua CLI para iniciar o aplicativo.

    ```Shell
    php artisan serve
    ```

1. Abra um navegador e navegue até `http://localhost:8000`.