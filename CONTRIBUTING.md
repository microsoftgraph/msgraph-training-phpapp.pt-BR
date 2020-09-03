# <a name="contributing-to-microsoft-graph-training-repositories"></a><span data-ttu-id="d435f-101">Contribuindo para os repositórios de treinamento do Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="d435f-101">Contributing to Microsoft Graph training repositories</span></span>

<span data-ttu-id="d435f-102">Obrigado por contribuir com este projeto!</span><span class="sxs-lookup"><span data-stu-id="d435f-102">Thank you for contributing to this project!</span></span> <span data-ttu-id="d435f-103">Antes de enviar sua solicitação pull, considere o seguinte.</span><span class="sxs-lookup"><span data-stu-id="d435f-103">Before submitting your pull request, be sure to consider the following.</span></span>

## <a name="overview"></a><span data-ttu-id="d435f-104">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="d435f-104">Overview</span></span>

<span data-ttu-id="d435f-105">O código neste repositório serve três finalidades:</span><span class="sxs-lookup"><span data-stu-id="d435f-105">The code in this repository serves three purposes:</span></span>

- <span data-ttu-id="d435f-106">Os arquivos de redução na pasta [tutorial](/tutorial) são publicados como um tutorial na página [tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) .</span><span class="sxs-lookup"><span data-stu-id="d435f-106">The Markdown files in the [tutorial](/tutorial) folder are published as a tutorial on the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) page.</span></span>
- <span data-ttu-id="d435f-107">O projeto de exemplo na pasta de [demonstração](/demo) é a fonte de um [início rápido do Microsoft Graph](https://developer.microsoft.com/graph/quick-start).**\***</span><span class="sxs-lookup"><span data-stu-id="d435f-107">The sample project in the [demo](/demo) folder is the source for a [Microsoft Graph quick start](https://developer.microsoft.com/graph/quick-start).**\***</span></span>
- <span data-ttu-id="d435f-108">O projeto de exemplo na pasta de demonstração também é baixável diretamente do GitHub e deve ser executado como ocorre após algumas configurações simples.</span><span class="sxs-lookup"><span data-stu-id="d435f-108">The sample project in the demo folder is also downloadable directly from GitHub and should run as-is after some simple configuration.</span></span>

> <span data-ttu-id="d435f-109">**\*** Nem todos os repositórios de treinamento estão disponíveis como início rápido (ainda).</span><span class="sxs-lookup"><span data-stu-id="d435f-109">**\*** Not all training repositories are available as quick starts (yet).</span></span>

<span data-ttu-id="d435f-110">É importante ter em mente que as alterações em um local *podem* exigir alterações em outro, para manter as coisas sincronizadas. Whereever possível, os arquivos de redução se referem aos arquivos de código-fonte diretamente (usando uma `:::code` sintaxe personalizada), de modo que a atualização do código na fonte atualizará automaticamente o código em redução.</span><span class="sxs-lookup"><span data-stu-id="d435f-110">This is important to keep in mind, because changes in one place *may* require changes in another, to keep things in sync. Whereever possible, the Markdown files refer to the source code files directly (using a custom `:::code` syntax), so that updating code in source will automatically update the code in Markdown.</span></span>

## <a name="updating-code"></a><span data-ttu-id="d435f-111">Atualizando código</span><span class="sxs-lookup"><span data-stu-id="d435f-111">Updating code</span></span>

<span data-ttu-id="d435f-112">A `:::code` sintaxe usada na redução depende de comentários específicos no arquivo de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="d435f-112">The `:::code` syntax used in Markdown depends on specific comments in the source code file.</span></span> <span data-ttu-id="d435f-113">Estes comentários têm a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="d435f-113">These comments look like the following:</span></span>

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

<span data-ttu-id="d435f-114">Se você atualizar o código entre esses comentários de "marcador", os arquivos de redução receberão automaticamente essas alterações quando forem publicados no site de documentação do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d435f-114">If you update code between these "marker" comments, the Markdown files will automatically get those changes when published to the Microsoft Graph documentation site.</span></span> <span data-ttu-id="d435f-115">Se você atualizar o código fora desses comentários, é muito provável que você precise atualizar a redução correspondente.</span><span class="sxs-lookup"><span data-stu-id="d435f-115">If you update code outside of those comments, it's very likely that you'll need to update the corresponding Markdown.</span></span>

## <a name="adding-features"></a><span data-ttu-id="d435f-116">Adicionando recursos</span><span class="sxs-lookup"><span data-stu-id="d435f-116">Adding features</span></span>

<span data-ttu-id="d435f-117">Embora o entusiasmo seja apreciado, não envie solicitações pull para adicionar novos recursos ao exemplo.</span><span class="sxs-lookup"><span data-stu-id="d435f-117">While the enthusiasm is appreciated, please don't send pull requests to add new features to the sample.</span></span> <span data-ttu-id="d435f-118">Como esse repositório é basicamente um tutorial "criar seu primeiro aplicativo", o conjunto de recursos é limitado por design.</span><span class="sxs-lookup"><span data-stu-id="d435f-118">Because this repository is primarily a "build your first app" tutorial, the feature set is limited, by design.</span></span>

## <a name="submitting-pull-requests"></a><span data-ttu-id="d435f-119">Enviando solicitações pull</span><span class="sxs-lookup"><span data-stu-id="d435f-119">Submitting pull requests</span></span>

<span data-ttu-id="d435f-120">Envie todas as solicitações pull para a `master` ramificação.</span><span class="sxs-lookup"><span data-stu-id="d435f-120">Please submit all pull requests to the `master` branch.</span></span>

## <a name="when-do-changes-get-published"></a><span data-ttu-id="d435f-121">Quando as alterações são publicadas?</span><span class="sxs-lookup"><span data-stu-id="d435f-121">When do changes get published?</span></span>

<span data-ttu-id="d435f-122">A publicação de atualizações no site de [tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) não é automática.</span><span class="sxs-lookup"><span data-stu-id="d435f-122">Publishing of updates to the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) site is not automatic.</span></span> <span data-ttu-id="d435f-123">As alterações devem ser promovidas primeiro para a `live` ramificação, então uma compilação deve ser disparada pelos administradores do site.</span><span class="sxs-lookup"><span data-stu-id="d435f-123">Changes must first be promoted to the `live` branch, then a build must be triggered by the site admins.</span></span> <span data-ttu-id="d435f-124">Isso geralmente é feito em uma base "conforme necessário".</span><span class="sxs-lookup"><span data-stu-id="d435f-124">This is typically done on an "as-needed" basis.</span></span>

## <a name="code-of-conduct"></a><span data-ttu-id="d435f-125">Código de conduta</span><span class="sxs-lookup"><span data-stu-id="d435f-125">Code of conduct</span></span>

<span data-ttu-id="d435f-p106">Este projeto adotou o [Código de Conduta do Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/). Para saber mais, confira as [Perguntas frequentes do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou contate [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.</span><span class="sxs-lookup"><span data-stu-id="d435f-p106">This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.</span></span>
