# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nessa pasta, você precisa do seguinte:

- [PHP](http://php.net/downloads.php) instalado em sua máquina de desenvolvimento. Se você não tiver PHP, visite o link anterior para opções de download. (**Observação:** Este tutorial foi escrito com PHP versão 7.4.4. As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)
- [O compositor](https://getcomposer.org/) está instalado em sua máquina de desenvolvimento.
- [Laravel](https://laravel.com/) instalado em sua máquina de desenvolvimento.
- Uma conta pessoal da Microsoft com uma caixa de correio Outlook.com uma conta de trabalho ou de estudante da Microsoft.

Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Você pode se inscrever no Programa para Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo Web com o centro de administração do Azure Active Directory

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros do aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `PHP Graph Tutorial`.
    - Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Web` e defina o valor como `http://localhost:8000/callback`.

    ![Uma captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. Escolha **Registrar**. Na página **do Tutorial do PHP Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. Selecione **Certificados e segredos** sob **Gerenciar**. Selecione o botão **Novo segredo do cliente**. Insira um valor em **Descrição**, selecione uma das opções para **Expira** e escolha **Adicionar**.

    ![Captura de tela da caixa de diálogo Adicionar um segredo do cliente](/tutorial/images/aad-new-client-secret.png)

1. Copie o valor secreto do cliente antes de sair desta página. Você precisará dele na próxima etapa.

    > [!IMPORTANT]
    > Este segredo do cliente nunca é mostrado novamente, portanto, copie-o agora.

    ![Uma captura de tela do segredo do cliente recém-adicionado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. Renomeie o `example.env` arquivo para `.env` .
1. Edite `.env` o arquivo e faça as seguintes alterações.
    1. Substitua `YOUR_APP_ID_HERE` pela **ID do Aplicativo que** você recebeu do Portal de Registro de Aplicativos.
    1. Substitua `YOUR_APP_PASSWORD_HERE` pela senha que você recebeu do Portal de Registro de Aplicativos.
1. Em sua CLI (interface de linha de comando), navegue até esse diretório e execute o seguinte comando para instalar os requisitos.

    ```Shell
    composer install
    ```

1. Em sua CLI (interface de linha de comando), execute o seguinte comando para gerar uma chave de aplicativo.

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a>Executar o exemplo

1. Execute o seguinte comando em sua CLI para iniciar o aplicativo.

    ```Shell
    php artisan serve
    ```

1. Abra um navegador e navegue até `http://localhost:8000`.
