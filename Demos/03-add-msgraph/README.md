# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="af3f8-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="af3f8-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af3f8-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="af3f8-102">Prerequisites</span></span>

<span data-ttu-id="af3f8-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="af3f8-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="af3f8-104">[Php](http://php.net/downloads.php) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="af3f8-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="af3f8-105">Se você não tiver o PHP, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="af3f8-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="af3f8-106">(**Observação:** este tutorial foi escrito com o PHP versão 7,2.</span><span class="sxs-lookup"><span data-stu-id="af3f8-106">(**Note:** This tutorial was written with PHP version 7.2.</span></span> <span data-ttu-id="af3f8-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="af3f8-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="af3f8-108">[Composer](https://getcomposer.org/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="af3f8-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="af3f8-109">[Laravel](https://laravel.com/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="af3f8-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="af3f8-110">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="af3f8-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="af3f8-111">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="af3f8-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="af3f8-112">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="af3f8-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="af3f8-113">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="af3f8-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="af3f8-114">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="af3f8-114">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="af3f8-115">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af3f8-115">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="af3f8-116">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="af3f8-117">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-117">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="af3f8-118">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="af3f8-118">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="af3f8-119">Selecione **novo registro**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-119">Select **New registration**.</span></span> <span data-ttu-id="af3f8-120">Na página **registrar um aplicativo** , defina os valores da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="af3f8-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="af3f8-121">Defina \*\*\*\* o nome `PHP Graph Tutorial`como.</span><span class="sxs-lookup"><span data-stu-id="af3f8-121">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="af3f8-122">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="af3f8-123">Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:8000/callback`valor como.</span><span class="sxs-lookup"><span data-stu-id="af3f8-123">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="af3f8-125">Escolha **registrar**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-125">Choose **Register**.</span></span> <span data-ttu-id="af3f8-126">Na página **tutorial do gráfico do PHP** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="af3f8-126">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="af3f8-128">Selecione **certificados & segredos** sob **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-128">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="af3f8-129">Selecione o botão **novo cliente secreto** .</span><span class="sxs-lookup"><span data-stu-id="af3f8-129">Select the **New client secret** button.</span></span> <span data-ttu-id="af3f8-130">Insira um valor em **Descrição** e selecione uma das opções para **expirar** e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="af3f8-130">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Uma captura de tela da caixa de diálogo Adicionar um segredo do cliente](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="af3f8-132">Copie o valor de segredo do cliente antes de sair desta página.</span><span class="sxs-lookup"><span data-stu-id="af3f8-132">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="af3f8-133">Você precisará dela na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="af3f8-133">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="af3f8-134">Esse segredo do cliente nunca é mostrado novamente, portanto, certifique-se de copiá-lo agora.</span><span class="sxs-lookup"><span data-stu-id="af3f8-134">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Uma captura de tela do novo segredo do cliente recentemente adicionado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="af3f8-136">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="af3f8-136">Configure the sample</span></span>

1. <span data-ttu-id="af3f8-137">ReNomear `.env.example` o `.env`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="af3f8-137">Rename the `.env.example` file to `.env`.</span></span>
1. <span data-ttu-id="af3f8-138">Edite `.env` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="af3f8-138">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="af3f8-139">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="af3f8-139">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="af3f8-140">Substitua `YOUR_APP_PASSWORD_HERE` pela senha obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="af3f8-140">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="af3f8-141">Na sua interface de linha de comando (CLI), navegue até este diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="af3f8-141">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```

1. <span data-ttu-id="af3f8-142">Na sua interface de linha de comando (CLI), execute o seguinte comando para gerar uma chave de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="af3f8-142">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="af3f8-143">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="af3f8-143">Run the sample</span></span>

1. <span data-ttu-id="af3f8-144">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="af3f8-144">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="af3f8-145">Abra um navegador e navegue até `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="af3f8-145">Open a browser and browse to `http://localhost:8000`.</span></span>