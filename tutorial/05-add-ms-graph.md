<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1c7a0-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="1c7a0-102">Para este aplicativo, você usará a biblioteca do [Microsoft-Graph](https://github.com/microsoftgraph/msgraph-sdk-php) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="1c7a0-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="1c7a0-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="1c7a0-104">Vamos começar adicionando um controlador para o modo de exibição calendário.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-104">Let's start by adding a controller for the calendar view.</span></span> <span data-ttu-id="1c7a0-105">Crie um novo arquivo na `./app/Http/Controllers` pasta nomeada `CalendarController.php`e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-105">Create a new file in the `./app/Http/Controllers` folder named `CalendarController.php`, and add the following code.</span></span>

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

<span data-ttu-id="1c7a0-106">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="1c7a0-107">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-107">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="1c7a0-108">O `$select` parâmetro limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-108">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="1c7a0-109">O `$orderby` parâmetro classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-109">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="1c7a0-110">Atualizar as rotas em `./routes/web.php` para adicionar uma rota a este novo controlador</span><span class="sxs-lookup"><span data-stu-id="1c7a0-110">Update the routes in `./routes/web.php` to add a route to this new controller</span></span>

```php
Route::get('/calendar', 'CalendarController@calendar');
```

<span data-ttu-id="1c7a0-111">Agora você pode testar isso.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-111">Now you can test this.</span></span> <span data-ttu-id="1c7a0-112">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-112">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="1c7a0-113">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-113">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="1c7a0-114">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="1c7a0-114">Display the results</span></span>

<span data-ttu-id="1c7a0-115">Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-115">Now you can add a view to display the results in a more user-friendly manner.</span></span> <span data-ttu-id="1c7a0-116">Crie um novo arquivo no `./resources/views` diretório chamado `calendar.blade.php` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-116">Create a new file in the `./resources/views` directory named `calendar.blade.php` and add the following code.</span></span>

```php
@extends('layout')

@section('content')
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    @isset($events)
      @foreach($events as $event)
        <tr>
          <td>{{ $event->getOrganizer()->getEmailAddress()->getName() }}</td>
          <td>{{ $event->getSubject() }}</td>
          <td>{{ \Carbon\Carbon::parse($event->getStart()->getDateTime())->format('n/j/y g:i A') }}</td>
          <td>{{ \Carbon\Carbon::parse($event->getEnd()->getDateTime())->format('n/j/y g:i A') }}</td>
        </tr>
      @endforeach
    @endif
  </tbody>
</table>
@endsection
```

<span data-ttu-id="1c7a0-117">Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-117">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="1c7a0-118">Remova a `return response()->json($events);` linha da `calendar` ação no `./app/Http/Controllers/CalendarController.php`e substitua-a pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-118">Remove the `return response()->json($events);` line from the `calendar` action in `./app/Http/Controllers/CalendarController.php`, and replace it with the following code.</span></span>

```php
$viewData['events'] = $events;
return view('calendar', $viewData);
```

<span data-ttu-id="1c7a0-119">Atualize a página e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="1c7a0-119">Refresh the page and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
