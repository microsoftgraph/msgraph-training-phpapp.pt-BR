<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4a273-101">Este tutorial ensina como criar um aplicativo Web PHP que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="4a273-101">This tutorial teaches you how to build a PHP web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="4a273-102">Se preferir baixar o tutorial concluído, você pode baixá-lo de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="4a273-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="4a273-103">Baixe o [início rápido do PHP](https://developer.microsoft.com/graph/quick-start?platform=option-php) para obter o código de trabalho em minutos.</span><span class="sxs-lookup"><span data-stu-id="4a273-103">Download the [PHP quick start](https://developer.microsoft.com/graph/quick-start?platform=option-php) to get working code in minutes.</span></span>
> - <span data-ttu-id="4a273-104">Baixe ou clone o [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-phpapp).</span><span class="sxs-lookup"><span data-stu-id="4a273-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a273-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="4a273-105">Prerequisites</span></span>

<span data-ttu-id="4a273-106">Antes de iniciar este tutorial, você deve ter o [php](http://php.net/downloads.php), o [Composer](https://getcomposer.org/)e o [Laravel](https://laravel.com/) instalados em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="4a273-106">Before you start this tutorial, you should have [PHP](http://php.net/downloads.php), [Composer](https://getcomposer.org/), and [Laravel](https://laravel.com/) installed on your development machine.</span></span>

> [!NOTE]
> <span data-ttu-id="4a273-107">Este tutorial foi escrito com a versão 7,2 do PHP.</span><span class="sxs-lookup"><span data-stu-id="4a273-107">This tutorial was written with PHP version 7.2.</span></span> <span data-ttu-id="4a273-108">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="4a273-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="4a273-109">Comentários</span><span class="sxs-lookup"><span data-stu-id="4a273-109">Feedback</span></span>

<span data-ttu-id="4a273-110">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-phpapp).</span><span class="sxs-lookup"><span data-stu-id="4a273-110">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>