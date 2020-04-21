<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e67e3-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e67e3-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="e67e3-102">Para este aplicativo, você usará a biblioteca do [Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="e67e3-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="e67e3-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="e67e3-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="e67e3-104">Crie um novo arquivo no diretório **./app/http/Controllers** chamado `CalendarController.php`e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e67e3-104">Create a new file in the **./app/Http/Controllers** directory named `CalendarController.php`, and add the following code.</span></span>

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    use App\TokenStore\TokenCache;

    class CalendarController extends Controller
    {
      public function calendar()
      {
        $viewData = $this->loadViewData();

        // Get the access token from the cache
        $tokenCache = new TokenCache();
        $accessToken = $tokenCache->getAccessToken();

        // Create a Graph client
        $graph = new Graph();
        $graph->setAccessToken($accessToken);

        $queryParams = array(
          '$select' => 'subject,organizer,start,end',
          '$orderby' => 'createdDateTime DESC'
        );

        // Append query parameters to the '/me/events' url
        $getEventsUrl = '/me/events?'.http_build_query($queryParams);

        $events = $graph->createRequest('GET', $getEventsUrl)
          ->setReturnType(Model\Event::class)
          ->execute();

        return response()->json($events);
      }
    }
    ```

    <span data-ttu-id="e67e3-105">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="e67e3-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="e67e3-106">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="e67e3-106">The URL that will be called is `/v1.0/me/events`.</span></span>
    - <span data-ttu-id="e67e3-107">O `$select` parâmetro limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="e67e3-107">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="e67e3-108">O `$orderby` parâmetro classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="e67e3-108">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="e67e3-109">Atualize as rotas no **./routes/Web.php** para adicionar uma rota a este novo controlador.</span><span class="sxs-lookup"><span data-stu-id="e67e3-109">Update the routes in **./routes/web.php** to add a route to this new controller.</span></span>

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. <span data-ttu-id="e67e3-110">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="e67e3-110">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="e67e3-111">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="e67e3-111">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="e67e3-112">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="e67e3-112">Display the results</span></span>

<span data-ttu-id="e67e3-113">Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="e67e3-113">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="e67e3-114">Crie um novo arquivo no diretório **./Resources/views** chamado `calendar.blade.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e67e3-114">Create a new file in the **./resources/views** directory named `calendar.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    <span data-ttu-id="e67e3-115">Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="e67e3-115">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="e67e3-116">Remova a `return response()->json($events);` linha da `calendar` ação em **./app/http/Controllers/CalendarController.php**e substitua-a pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e67e3-116">Remove the `return response()->json($events);` line from the `calendar` action in **./app/Http/Controllers/CalendarController.php**, and replace it with the following code.</span></span>

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. <span data-ttu-id="e67e3-117">Atualize a página e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="e67e3-117">Refresh the page and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
