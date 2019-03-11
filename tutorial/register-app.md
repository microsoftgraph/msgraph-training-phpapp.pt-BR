<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="df471-101">Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o ARP (portal de registro de aplicativo).</span><span class="sxs-lookup"><span data-stu-id="df471-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="df471-102">Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="df471-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="df471-103">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="df471-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="df471-104">Selecione **Adicionar um aplicativo** na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="df471-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df471-105">Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .</span><span class="sxs-lookup"><span data-stu-id="df471-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="df471-106">Na página **registrar seu aplicativo** , defina o tutorial de **nome do aplicativo** para o gráfico do **php** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="df471-106">On the **Register your application** page, set the **Application Name** to **PHP Graph Tutorial** and select **Create**.</span></span>

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](./images/arp-create-app-01.png)

1. <span data-ttu-id="df471-108">Na página **registro do tutorial do gráfico php** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="df471-108">On the **PHP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de tela da ID do aplicativo recém-criado](./images/arp-create-app-02.png)

1. <span data-ttu-id="df471-110">Role para baixo até a seção **segredos do aplicativo** .</span><span class="sxs-lookup"><span data-stu-id="df471-110">Scroll down to the **Application Secrets** section.</span></span>

    1. <span data-ttu-id="df471-111">Selecione **gerar nova senha**.</span><span class="sxs-lookup"><span data-stu-id="df471-111">Select **Generate New Password**.</span></span>
    1. <span data-ttu-id="df471-112">Na caixa de diálogo **nova senha gerada** , copie o conteúdo da caixa, pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="df471-112">In the **New password generated** dialog, copy the contents of the box as you will need it later.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="df471-113">Essa senha nunca é mostrada novamente, portanto, certifique-se de copiá-la agora.</span><span class="sxs-lookup"><span data-stu-id="df471-113">This password is never shown again, so make sure you copy it now.</span></span>

    ![Captura de tela da senha do aplicativo recém-criado](./images/arp-create-app-03.png)

1. <span data-ttu-id="df471-115">Role para baixo até a seção **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="df471-115">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="df471-116">Selecione **Adicionar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="df471-116">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="df471-117">Na caixa de diálogo **Adicionar plataforma** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="df471-117">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de tela criando uma plataforma para o aplicativo](./images/arp-create-app-04.png)

    1. <span data-ttu-id="df471-119">Na caixa plataforma **da Web** , digite a URL `http://localhost:8000/callback` das **URLs**de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="df471-119">In the **Web** platform box, enter the URL `http://localhost:8000/callback` for the **Redirect URLs**.</span></span>

        ![Captura de tela da nova plataforma Web adicionada para o aplicativo](./images/arp-create-app-05.png)

1. <span data-ttu-id="df471-121">Role até a parte inferior da página e selecione **salvar**.</span><span class="sxs-lookup"><span data-stu-id="df471-121">Scroll to the bottom of the page and select **Save**.</span></span>