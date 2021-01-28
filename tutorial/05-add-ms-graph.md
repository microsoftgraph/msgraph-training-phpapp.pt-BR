<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph ao aplicativo. Para este aplicativo, você usará a biblioteca [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Crie um novo diretório no diretório **./app** chamado , em seguida, crie um novo arquivo nesse diretório nomeado `TimeZones` e adicione o código a `TimeZones.php` seguir.

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    Essa classe implementa um mapeamento simplístico de nomes de fuso horário do Windows para identificadores de fuso horário IANA.

1. Crie um novo arquivo no **diretório ./app/Http/Controllers** nomeado `CalendarController.php` e adicione o código a seguir.

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

        $viewData['dateRange'] = $startOfWeek->format('M j, Y').' - '.$endOfWeek->format('M j, Y');

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

    Considere o que este código está fazendo.

    - A URL que será chamada é `/v1.0/me/calendarView` .
    - The `startDateTime` and `endDateTime` parameters define the start and end of the view.
    - O parâmetro limita os campos retornados para cada evento a apenas aqueles que o ponto de exibição `$select` realmente usará.
    - O parâmetro classifica os resultados pela data e hora em que foram criados, com `$orderby` o item mais recente sendo o primeiro.
    - O `$top` parâmetro limita os resultados a 25 eventos.
    - O header faz com que os horários de início e término na resposta sejam ajustados para `Prefer: outlook.timezone=""` o fuso horário preferido do usuário.

1. Atualize as rotas **em ./routes/web.php** para adicionar uma rota a esse novo controlador.

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. Entre e clique no link **Calendário** na barra de inv. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode adicionar um modo de exibição para exibir os resultados de maneira mais amigável.

1. Crie um novo arquivo no diretório **./resources/views** nomeado `calendar.blade.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    Isso fará um loop por uma coleção de eventos e adicionará uma linha de tabela para cada um deles.

1. Atualize as rotas **em ./routes/web.php** para adicionar rotas para `/calendar/new` . Você implementará essas funções na próxima seção, mas a rota precisa ser definida agora porque **calendar.blade.php** faz referência a ela.

    ```php
    Route::get('/calendar/new', 'CalendarController@getNewEventForm');
    Route::post('/calendar/new', 'CalendarController@createNewEvent');
    ```

1. Remova a `return response()->json($events);` linha da ação em `calendar` **./app/Http/Controllers/CalendarController.php** e substitua-a pelo código a seguir.

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. Atualize a página e o aplicativo agora deverá renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
