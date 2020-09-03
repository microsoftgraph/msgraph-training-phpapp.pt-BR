<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="55c04-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c04-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="55c04-102">Para este aplicativo, você usará a biblioteca do [Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="55c04-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="55c04-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="55c04-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="55c04-104">Crie um novo diretório no diretório **./app** chamado e, `TimeZones` em seguida, crie um novo arquivo no diretório chamado `TimeZones.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="55c04-104">Create a new directory in the **./app** directory named `TimeZones`, then create a new file in that directory named `TimeZones.php`, and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    <span data-ttu-id="55c04-105">Essa classe implementa um mapeamento simplista de nomes de fuso horário do Windows para os identificadores de fuso horário da IANA.</span><span class="sxs-lookup"><span data-stu-id="55c04-105">This class implements a simplistic mapping of Windows time zone names to IANA time zone identifiers.</span></span>

1. <span data-ttu-id="55c04-106">Crie um novo arquivo no diretório **./app/http/Controllers** chamado `CalendarController.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="55c04-106">Create a new file in the **./app/Http/Controllers** directory named `CalendarController.php`, and add the following code.</span></span>

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    use App\TokenStore\TokenCache;
    use App\TimeZones\TimeZones;

    class CalendarController extends Controller
    {
      public function calendar()
      {
        $viewData = $this->loadViewData();

        $graph = $this->getGraph();

        // Get user's timezone
        $timezone = TimeZones::getTzFromWindows($viewData['userTimeZone']);

        // Get start and end of week
        $startOfWeek = new \DateTimeImmutable('sunday -1 week', $timezone);
        $endOfWeek = new \DateTimeImmutable('sunday', $timezone);

        $queryParams = array(
          'startDateTime' => $startOfWeek->format(\DateTimeInterface::ISO8601),
          'endDateTime' => $endOfWeek->format(\DateTimeInterface::ISO8601),
          // Only request the properties used by the app
          '$select' => 'subject,organizer,start,end',
          // Sort them by start time
          '$orderby' => 'start/dateTime',
          // Limit results to 25
          '$top' => 25
        );

        // Append query parameters to the '/me/calendarView' url
        $getEventsUrl = '/me/calendarView?'.http_build_query($queryParams);

        $events = $graph->createRequest('GET', $getEventsUrl)
          // Add the user's timezone to the Prefer header
          ->addHeaders(array(
            'Prefer' => 'outlook.timezone="'.$viewData['userTimeZone'].'"'
          ))
          ->setReturnType(Model\Event::class)
          ->execute();

        return response()->json($events);
      }

      private function getGraph(): Graph
      {
        // Get the access token from the cache
        $tokenCache = new TokenCache();
        $accessToken = $tokenCache->getAccessToken();

        // Create a Graph client
        $graph = new Graph();
        $graph->setAccessToken($accessToken);
        return $graph;
      }
    }
    ```

    <span data-ttu-id="55c04-107">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="55c04-107">Consider what this code is doing.</span></span>

    - <span data-ttu-id="55c04-108">A URL que será chamada é `/v1.0/me/calendarView` .</span><span class="sxs-lookup"><span data-stu-id="55c04-108">The URL that will be called is `/v1.0/me/calendarView`.</span></span>
    - <span data-ttu-id="55c04-109">Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="55c04-109">The `startDateTime` and `endDateTime` parameters define the start and end of the view.</span></span>
    - <span data-ttu-id="55c04-110">O `$select` parâmetro limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="55c04-110">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="55c04-111">O `$orderby` parâmetro classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="55c04-111">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="55c04-112">O `$top` parâmetro limita os resultados a 25 eventos.</span><span class="sxs-lookup"><span data-stu-id="55c04-112">The `$top` parameter limits the results to 25 events.</span></span>
    - <span data-ttu-id="55c04-113">O `Prefer: outlook.timezone=""` cabeçalho faz com que as horas de início e de término na resposta sejam ajustadas para o fuso horário preferencial do usuário.</span><span class="sxs-lookup"><span data-stu-id="55c04-113">The `Prefer: outlook.timezone=""` header causes the start and end times in the response to be adjusted to the user's preferred time zone.</span></span>

1. <span data-ttu-id="55c04-114">Atualize as rotas no **./routes/Web.php** para adicionar uma rota a este novo controlador.</span><span class="sxs-lookup"><span data-stu-id="55c04-114">Update the routes in **./routes/web.php** to add a route to this new controller.</span></span>

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. <span data-ttu-id="55c04-115">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="55c04-115">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="55c04-116">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="55c04-116">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="55c04-117">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="55c04-117">Display the results</span></span>

<span data-ttu-id="55c04-118">Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="55c04-118">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="55c04-119">Crie um novo arquivo no diretório **./Resources/views** chamado `calendar.blade.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="55c04-119">Create a new file in the **./resources/views** directory named `calendar.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    <span data-ttu-id="55c04-120">Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="55c04-120">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="55c04-121">Remova a `return response()->json($events);` linha da `calendar` ação em **./app/http/Controllers/CalendarController.php**e substitua-a pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="55c04-121">Remove the `return response()->json($events);` line from the `calendar` action in **./app/Http/Controllers/CalendarController.php**, and replace it with the following code.</span></span>

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. <span data-ttu-id="55c04-122">Atualize a página e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="55c04-122">Refresh the page and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
