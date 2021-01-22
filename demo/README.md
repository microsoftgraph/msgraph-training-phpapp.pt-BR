# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="63aa1-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="63aa1-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63aa1-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="63aa1-102">Prerequisites</span></span>

<span data-ttu-id="63aa1-103">Para executar o projeto concluído nessa pasta, você precisa do seguinte:</span><span class="sxs-lookup"><span data-stu-id="63aa1-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="63aa1-104">[PHP](http://php.net/downloads.php) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="63aa1-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="63aa1-105">Se você não tiver PHP, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="63aa1-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="63aa1-106">(**Observação:** Este tutorial foi escrito com PHP versão 7.4.4.</span><span class="sxs-lookup"><span data-stu-id="63aa1-106">(**Note:** This tutorial was written with PHP version 7.4.4.</span></span> <span data-ttu-id="63aa1-107">As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)</span><span class="sxs-lookup"><span data-stu-id="63aa1-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="63aa1-108">[O compositor](https://getcomposer.org/) está instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="63aa1-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="63aa1-109">[Laravel](https://laravel.com/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="63aa1-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="63aa1-110">Uma conta pessoal da Microsoft com uma caixa de correio Outlook.com uma conta de trabalho ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63aa1-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="63aa1-111">Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="63aa1-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="63aa1-112">Você pode [se inscrever para uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="63aa1-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="63aa1-113">Você pode se inscrever no Programa para Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="63aa1-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="63aa1-114">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63aa1-114">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="63aa1-115">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="63aa1-115">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="63aa1-116">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="63aa1-117">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-117">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="63aa1-118">Uma captura de tela dos registros do aplicativo</span><span class="sxs-lookup"><span data-stu-id="63aa1-118">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="63aa1-119">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-119">Select **New registration**.</span></span> <span data-ttu-id="63aa1-120">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="63aa1-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="63aa1-121">Defina **Nome** para `PHP Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="63aa1-121">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="63aa1-122">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="63aa1-123">Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Web` e defina o valor como `http://localhost:8000/callback`.</span><span class="sxs-lookup"><span data-stu-id="63aa1-123">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![Uma captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="63aa1-125">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-125">Choose **Register**.</span></span> <span data-ttu-id="63aa1-126">Na página **do Tutorial do PHP Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="63aa1-126">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="63aa1-128">Selecione **Certificados e segredos** sob **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-128">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="63aa1-129">Selecione o botão **Novo segredo do cliente**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-129">Select the **New client secret** button.</span></span> <span data-ttu-id="63aa1-130">Insira um valor em **Descrição**, selecione uma das opções para **Expira** e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="63aa1-130">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Captura de tela da caixa de diálogo Adicionar um segredo do cliente](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="63aa1-132">Copie o valor secreto do cliente antes de sair desta página.</span><span class="sxs-lookup"><span data-stu-id="63aa1-132">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="63aa1-133">Você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="63aa1-133">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="63aa1-134">Este segredo do cliente nunca é mostrado novamente, portanto, copie-o agora.</span><span class="sxs-lookup"><span data-stu-id="63aa1-134">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Uma captura de tela do segredo do cliente recém-adicionado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="63aa1-136">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="63aa1-136">Configure the sample</span></span>

1. <span data-ttu-id="63aa1-137">Renomeie o `example.env` arquivo para `.env` .</span><span class="sxs-lookup"><span data-stu-id="63aa1-137">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="63aa1-138">Edite `.env` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="63aa1-138">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="63aa1-139">Substitua `YOUR_APP_ID_HERE` pela **ID do Aplicativo que** você recebeu do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="63aa1-139">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="63aa1-140">Substitua `YOUR_APP_PASSWORD_HERE` pela senha que você recebeu do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="63aa1-140">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="63aa1-141">Em sua CLI (interface de linha de comando), navegue até esse diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="63aa1-141">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```

1. <span data-ttu-id="63aa1-142">Em sua CLI (interface de linha de comando), execute o seguinte comando para gerar uma chave de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="63aa1-142">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="63aa1-143">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="63aa1-143">Run the sample</span></span>

1. <span data-ttu-id="63aa1-144">Execute o seguinte comando em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="63aa1-144">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="63aa1-145">Abra um navegador e navegue até `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="63aa1-145">Open a browser and browse to `http://localhost:8000`.</span></span>
