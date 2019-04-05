<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e0d96-101">Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o centro de administração do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e0d96-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="e0d96-102">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e0d96-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="e0d96-103">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="e0d96-104">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="e0d96-105">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="e0d96-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="e0d96-106">Selecione **novo registro**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-106">Select **New registration**.</span></span> <span data-ttu-id="e0d96-107">Na página **registrar um aplicativo** , defina os valores da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e0d96-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="e0d96-108">Defina \*\*\*\* o nome `PHP Graph Tutorial`como.</span><span class="sxs-lookup"><span data-stu-id="e0d96-108">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="e0d96-109">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="e0d96-110">Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:8000/callback`valor como.</span><span class="sxs-lookup"><span data-stu-id="e0d96-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. <span data-ttu-id="e0d96-112">Escolha **registrar**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-112">Choose **Register**.</span></span> <span data-ttu-id="e0d96-113">Na página **tutorial do gráfico do PHP** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="e0d96-113">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. <span data-ttu-id="e0d96-115">Selecione **certificados & segredos** sob **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-115">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="e0d96-116">Selecione o botão **novo cliente secreto** .</span><span class="sxs-lookup"><span data-stu-id="e0d96-116">Select the **New client secret** button.</span></span> <span data-ttu-id="e0d96-117">Insira um valor em **Descrição** e selecione uma das opções para **expirar** e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e0d96-117">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Uma captura de tela da caixa de diálogo Adicionar um segredo do cliente](./images/aad-new-client-secret.png)

1. <span data-ttu-id="e0d96-119">Copie o valor de segredo do cliente antes de sair desta página.</span><span class="sxs-lookup"><span data-stu-id="e0d96-119">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="e0d96-120">Você precisará dela na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="e0d96-120">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e0d96-121">Esse segredo do cliente nunca é mostrado novamente, portanto, certifique-se de copiá-lo agora.</span><span class="sxs-lookup"><span data-stu-id="e0d96-121">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Uma captura de tela do novo segredo do cliente recentemente adicionado](./images/aad-copy-client-secret.png)