<!-- markdownlint-disable MD002 MD041 -->

Este tutorial ensina a criar um aplicativo Web PHP que usa a API do Microsoft Graph para recuperar informações de calendário para um usuário.

> [!TIP]
> Se preferir apenas baixar o tutorial concluído, você pode baixá-lo de duas maneiras.
>
> - Baixe o [início rápido do PHP](https://developer.microsoft.com/graph/quick-start?platform=option-php) para obter o código de trabalho em minutos.
> - Baixe ou clone o [repositório do GitHub.](https://github.com/microsoftgraph/msgraph-training-phpapp)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial, você deve ter [o PHP,](http://php.net/downloads.php) [o Composer](https://getcomposer.org/)e o [Laravel](https://laravel.com/) instalados em seu computador de desenvolvimento.

Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio Outlook.com ou uma conta de trabalho ou de estudante da Microsoft. Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Você pode se inscrever no Programa para Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

> [!NOTE]
> Este tutorial foi escrito com a versão PHP 8.0.1, o Composer versão 2.0.8 e o instalador do Laravel versão 4.1.1. As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.

## <a name="feedback"></a>Comentários

Faça comentários sobre este tutorial no repositório [do GitHub.](https://github.com/microsoftgraph/msgraph-training-phpapp)
