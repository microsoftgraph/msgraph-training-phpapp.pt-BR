<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d7ab1-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-new-event-form"></a><span data-ttu-id="d7ab1-102">Criar novo formulário de evento</span><span class="sxs-lookup"><span data-stu-id="d7ab1-102">Create new event form</span></span>

1. <span data-ttu-id="d7ab1-103">Crie um novo arquivo no diretório **./resources/views** nomeado `newevent.blade.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-103">Create a new file in the **./resources/views** directory named `newevent.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/newevent.blade.php" id="NewEventFormSnippet":::

## <a name="add-controller-actions"></a><span data-ttu-id="d7ab1-104">Adicionar ações do controlador</span><span class="sxs-lookup"><span data-stu-id="d7ab1-104">Add controller actions</span></span>

1. <span data-ttu-id="d7ab1-105">Abra **./app/Http/Controllers/CalendarController.php** e adicione a função a seguir para renderizar o formulário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-105">Open **./app/Http/Controllers/CalendarController.php** and add the following function to render the form.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="getNewEventFormSnippet":::

1. <span data-ttu-id="d7ab1-106">Adicione a função a seguir para receber os dados do formulário quando o usuário enviar e criar um novo evento no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-106">Add the following function to receive the form data when the user's submits, and create a new event on the user's calendar.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="createNewEventSnippet":::

    <span data-ttu-id="d7ab1-107">Considere o que esse código faz.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-107">Consider what this code does.</span></span>

    - <span data-ttu-id="d7ab1-108">Ele converte a entrada de campo dos participantes em uma matriz de objetos [participantes do](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) Graph.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-108">It converts the attendees field input to an array of Graph [attendee](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) objects.</span></span>
    - <span data-ttu-id="d7ab1-109">Ele cria um [evento a partir](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) da entrada do formulário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-109">It builds an [event](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) from the form input.</span></span>
    - <span data-ttu-id="d7ab1-110">Ele envia um POST para o ponto de extremidade e redireciona de volta para a `/me/events` exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-110">It sends a POST to the `/me/events` endpoint, then redirects back to the calendar view.</span></span>

1. <span data-ttu-id="d7ab1-111">Salve todas as suas alterações e reinicie o servidor.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-111">Save all of your changes and restart the server.</span></span> <span data-ttu-id="d7ab1-112">Use o **botão Novo** evento para navegar até o novo formulário de eventos.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-112">Use the **New event** button to navigate to the new event form.</span></span>

1. <span data-ttu-id="d7ab1-113">Preencha os valores no formulário.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-113">Fill in the values on the form.</span></span> <span data-ttu-id="d7ab1-114">Use uma data de início da semana atual.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-114">Use a start date from the current week.</span></span> <span data-ttu-id="d7ab1-115">Selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-115">Select **Create**.</span></span>

    ![Uma captura de tela do novo formulário de eventos](images/create-event-01.png)

1. <span data-ttu-id="d7ab1-117">Quando o aplicativo redireciona para a exibição de calendário, verifique se o novo evento está presente nos resultados.</span><span class="sxs-lookup"><span data-stu-id="d7ab1-117">When the app redirects to the calendar view, verify that your new event is present in the results.</span></span>
