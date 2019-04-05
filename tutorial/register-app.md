<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o centro de administração do Azure Active Directory.

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.

1. Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](./images/aad-portal-app-registrations.png)

1. Selecione **novo registro**. Na página **registrar um aplicativo** , defina os valores da seguinte maneira.

    - Defina **** o nome `PHP Graph Tutorial`como.
    - Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:8000/callback`valor como.

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. Escolha **registrar**. Na página **tutorial do gráfico do PHP** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. Selecione **certificados & segredos** sob **gerenciar**. Selecione o botão **novo cliente secreto** . Insira um valor em **Descrição** e selecione uma das opções para **expirar** e escolha **Adicionar**.

    ![Uma captura de tela da caixa de diálogo Adicionar um segredo do cliente](./images/aad-new-client-secret.png)

1. Copie o valor de segredo do cliente antes de sair desta página. Você precisará dela na próxima etapa.

    > [!IMPORTANT]
    > Esse segredo do cliente nunca é mostrado novamente, portanto, certifique-se de copiá-lo agora.

    ![Uma captura de tela do novo segredo do cliente recentemente adicionado](./images/aad-copy-client-secret.png)