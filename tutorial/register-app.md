<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o ARP (portal de registro de aplicativo).

1. Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com). Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.

1. Selecione **Adicionar um aplicativo** na parte superior da página.

    > [!NOTE]
    > Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .

1. Na página **registrar seu aplicativo** , defina o tutorial de **nome do aplicativo** para o gráfico do **php** e selecione **criar**.

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](./images/arp-create-app-01.png)

1. Na página **registro do tutorial do gráfico php** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.

    ![Captura de tela da ID do aplicativo recém-criado](./images/arp-create-app-02.png)

1. Role para baixo até a seção **segredos do aplicativo** .

    1. Selecione **gerar nova senha**.
    1. Na caixa de diálogo **nova senha gerada** , copie o conteúdo da caixa, pois você precisará dela mais tarde.

        > [!IMPORTANT]
        > Essa senha nunca é mostrada novamente, portanto, certifique-se de copiá-la agora.

    ![Captura de tela da senha do aplicativo recém-criado](./images/arp-create-app-03.png)

1. Role para baixo até a seção **plataformas** .

    1. Selecione **Adicionar plataforma**.
    1. Na caixa de diálogo **Adicionar plataforma** , selecione **Web**.

        ![Captura de tela criando uma plataforma para o aplicativo](./images/arp-create-app-04.png)

    1. Na caixa plataforma **da Web** , digite a URL `http://localhost:8000/callback` das **URLs**de redirecionamento.

        ![Captura de tela da nova plataforma Web adicionada para o aplicativo](./images/arp-create-app-05.png)

1. Role até a parte inferior da página e selecione **salvar**.