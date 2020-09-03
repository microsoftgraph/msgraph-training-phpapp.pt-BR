<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca do [Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Crie um novo diretório no diretório **./app** chamado e, `TimeZones` em seguida, crie um novo arquivo no diretório chamado `TimeZones.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    Essa classe implementa um mapeamento simplista de nomes de fuso horário do Windows para os identificadores de fuso horário da IANA.

1. Crie um novo arquivo no diretório **./app/http/Controllers** chamado `CalendarController.php` e adicione o código a seguir.

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

    Considere o que esse código está fazendo.

    - A URL que será chamada é `/v1.0/me/calendarView` .
    - Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do modo de exibição.
    - O `$select` parâmetro limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
    - O `$orderby` parâmetro classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.
    - O `$top` parâmetro limita os resultados a 25 eventos.
    - O `Prefer: outlook.timezone=""` cabeçalho faz com que as horas de início e de término na resposta sejam ajustadas para o fuso horário preferencial do usuário.

1. Atualize as rotas no **./routes/Web.php** para adicionar uma rota a este novo controlador.

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.

1. Crie um novo arquivo no diretório **./Resources/views** chamado `calendar.blade.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.

1. Remova a `return response()->json($events);` linha da `calendar` ação em **./app/http/Controllers/CalendarController.php**e substitua-a pelo código a seguir.

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. Atualize a página e o aplicativo agora deve renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
