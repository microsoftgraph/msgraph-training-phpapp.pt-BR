# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="01cbe-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="01cbe-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01cbe-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="01cbe-102">Prerequisites</span></span>

<span data-ttu-id="01cbe-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="01cbe-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="01cbe-104">[Php](http://php.net/downloads.php) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="01cbe-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="01cbe-105">Se você não tiver o PHP, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="01cbe-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="01cbe-106">(**Observação:** este tutorial foi escrito com o PHP versão 7,2.</span><span class="sxs-lookup"><span data-stu-id="01cbe-106">(**Note:** This tutorial was written with PHP version 7.2.</span></span> <span data-ttu-id="01cbe-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="01cbe-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="01cbe-108">[Composer](https://getcomposer.org/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="01cbe-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="01cbe-109">[Laravel](https://laravel.com/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="01cbe-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="01cbe-110">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="01cbe-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="01cbe-111">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="01cbe-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="01cbe-112">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="01cbe-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="01cbe-113">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="01cbe-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="01cbe-114">Registrar um aplicativo Web com o portal de registro do aplicativo</span><span class="sxs-lookup"><span data-stu-id="01cbe-114">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="01cbe-115">Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="01cbe-115">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="01cbe-116">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="01cbe-117">Selecione **Adicionar um aplicativo** na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="01cbe-117">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="01cbe-118">**Observação:** Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .</span><span class="sxs-lookup"><span data-stu-id="01cbe-118">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="01cbe-119">Na página **registrar seu aplicativo** , defina o tutorial de **nome do aplicativo** para o gráfico do **php** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-119">On the **Register your application** page, set the **Application Name** to **PHP Graph Tutorial** and select **Create**.</span></span>

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="01cbe-121">Na página **registro do tutorial do gráfico php** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="01cbe-121">On the **PHP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de tela da ID do aplicativo recém-criado](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="01cbe-123">Role para baixo até a seção **segredos do aplicativo** .</span><span class="sxs-lookup"><span data-stu-id="01cbe-123">Scroll down to the **Application Secrets** section.</span></span>

    1. <span data-ttu-id="01cbe-124">Selecione **gerar nova senha**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-124">Select **Generate New Password**.</span></span>
    1. <span data-ttu-id="01cbe-125">Na caixa de diálogo **nova senha gerada** , copie o conteúdo da caixa, pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="01cbe-125">In the **New password generated** dialog, copy the contents of the box as you will need it later.</span></span>

        > <span data-ttu-id="01cbe-126">**Importante:** Essa senha nunca é mostrada novamente, portanto, certifique-se de copiá-la agora.</span><span class="sxs-lookup"><span data-stu-id="01cbe-126">**Important:** This password is never shown again, so make sure you copy it now.</span></span>

    ![Captura de tela da senha do aplicativo recém-criado](/tutorial/images/arp-create-app-03.png)

1. <span data-ttu-id="01cbe-128">Role para baixo até a seção **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="01cbe-128">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="01cbe-129">Selecione **Adicionar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-129">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="01cbe-130">Na caixa de diálogo **Adicionar plataforma** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-130">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de tela criando uma plataforma para o aplicativo](/tutorial/images/arp-create-app-04.png)

    1. <span data-ttu-id="01cbe-132">Na caixa plataforma **da Web** , digite a URL `http://localhost:8000/callback` das **URLs**de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="01cbe-132">In the **Web** platform box, enter the URL `http://localhost:8000/callback` for the **Redirect URLs**.</span></span>

        ![Captura de tela da nova plataforma Web adicionada para o aplicativo](/tutorial/images/arp-create-app-05.png)

1. <span data-ttu-id="01cbe-134">Role até a parte inferior da página e selecione **salvar**.</span><span class="sxs-lookup"><span data-stu-id="01cbe-134">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="01cbe-135">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="01cbe-135">Configure the sample</span></span>

1. <span data-ttu-id="01cbe-136">ReNomear `.env.example` o `.env`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="01cbe-136">Rename the `.env.example` file to `.env`.</span></span>
1. <span data-ttu-id="01cbe-137">Edite `.env` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="01cbe-137">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="01cbe-138">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01cbe-138">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="01cbe-139">Substitua `YOUR_APP_PASSWORD_HERE` pela senha obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01cbe-139">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="01cbe-140">Na sua interface de linha de comando (CLI), navegue até este diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="01cbe-140">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```
1. <span data-ttu-id="01cbe-141">Na sua interface de linha de comando (CLI), execute o seguinte comando para gerar uma chave de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01cbe-141">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="01cbe-142">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="01cbe-142">Run the sample</span></span>

1. <span data-ttu-id="01cbe-143">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01cbe-143">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="01cbe-144">Abra um navegador e navegue até `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="01cbe-144">Open a browser and browse to `http://localhost:8000`.</span></span>