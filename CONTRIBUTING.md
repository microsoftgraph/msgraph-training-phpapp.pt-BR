# <a name="contributing-to-microsoft-graph-training-repositories"></a>Contribuindo para os repositórios de treinamento do Microsoft Graph

Obrigado por contribuir com este projeto! Antes de enviar sua solicitação pull, considere o seguinte.

## <a name="overview"></a>Visão Geral

O código neste repositório serve três finalidades:

- Os arquivos de redução na pasta [tutorial](/tutorial) são publicados como um tutorial na página [tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) .
- O projeto de exemplo na pasta de [demonstração](/demo) é a fonte de um [início rápido do Microsoft Graph](https://developer.microsoft.com/graph/quick-start).**\***
- O projeto de exemplo na pasta de demonstração também é baixável diretamente do GitHub e deve ser executado como ocorre após algumas configurações simples.

> **\*** Nem todos os repositórios de treinamento estão disponíveis como início rápido (ainda).

É importante ter em mente que as alterações em um local *podem* exigir alterações em outro, para manter as coisas sincronizadas. Whereever possível, os arquivos de redução se referem aos arquivos de código-fonte diretamente (usando uma `:::code` sintaxe personalizada), de modo que a atualização do código na fonte atualizará automaticamente o código em redução.

## <a name="updating-code"></a>Atualizando código

A `:::code` sintaxe usada na redução depende de comentários específicos no arquivo de código-fonte. Estes comentários têm a seguinte aparência:

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

Se você atualizar o código entre esses comentários de "marcador", os arquivos de redução receberão automaticamente essas alterações quando forem publicados no site de documentação do Microsoft Graph. Se você atualizar o código fora desses comentários, é muito provável que você precise atualizar a redução correspondente.

## <a name="adding-features"></a>Adicionando recursos

Embora o entusiasmo seja apreciado, não envie solicitações pull para adicionar novos recursos ao exemplo. Como esse repositório é basicamente um tutorial "criar seu primeiro aplicativo", o conjunto de recursos é limitado por design.

## <a name="submitting-pull-requests"></a>Enviando solicitações pull

Envie todas as solicitações pull para a `master` ramificação.

## <a name="when-do-changes-get-published"></a>Quando as alterações são publicadas?

A publicação de atualizações no site de [tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) não é automática. As alterações devem ser promovidas primeiro para a `live` ramificação, então uma compilação deve ser disparada pelos administradores do site. Isso geralmente é feito em uma base "conforme necessário".

## <a name="code-of-conduct"></a>Código de conduta

Este projeto adotou o [Código de Conduta do Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/). Para saber mais, confira as [Perguntas frequentes do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou contate [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.
