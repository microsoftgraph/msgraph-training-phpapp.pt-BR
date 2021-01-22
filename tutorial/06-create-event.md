<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

## <a name="create-new-event-form"></a>Criar novo formulário de evento

1. Crie um novo arquivo no diretório **./resources/views** nomeado `newevent.blade.php` e adicione o código a seguir.

    :::code language="php" source="../demo/graph-tutorial/resources/views/newevent.blade.php" id="NewEventFormSnippet":::

## <a name="add-controller-actions"></a>Adicionar ações do controlador

1. Abra **./app/Http/Controllers/CalendarController.php** e adicione a função a seguir para renderizar o formulário.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="getNewEventFormSnippet":::

1. Adicione a função a seguir para receber os dados do formulário quando o usuário enviar e criar um novo evento no calendário do usuário.

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="createNewEventSnippet":::

    Considere o que esse código faz.

    - Ele converte a entrada de campo dos participantes em uma matriz de objetos [participantes do](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) Graph.
    - Ele cria um [evento a partir](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) da entrada do formulário.
    - Ele envia um POST para o ponto de extremidade e redireciona de volta para a `/me/events` exibição de calendário.

1. Salve todas as suas alterações e reinicie o servidor. Use o **botão Novo** evento para navegar até o novo formulário de eventos.

1. Preencha os valores no formulário. Use uma data de início da semana atual. Selecione **Criar**.

    ![Uma captura de tela do novo formulário de eventos](images/create-event-01.png)

1. Quando o aplicativo redireciona para a exibição de calendário, verifique se o novo evento está presente nos resultados.
