---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018844"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="417ca-101">В этом руководстве рассказывается, как создать консольное приложение Java, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="417ca-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="417ca-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="417ca-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="417ca-103">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="417ca-103">Prerequisites</span></span>

<span data-ttu-id="417ca-104">Прежде чем приступить к работе с этим руководством, на компьютере для разработки должен быть установлен [набор средств разработки Java SE (ЖДК)](https://java.com/en/download/faq/develop.xml) и [Мавен](https://maven.apache.org/) .</span><span class="sxs-lookup"><span data-stu-id="417ca-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="417ca-105">Если у вас нет ЖДК или Мавен, посетите предыдущие ссылки для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="417ca-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="417ca-106">Это руководство было написано с помощью Опенждк версии 12.0.1 и Мавен 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="417ca-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="417ca-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="417ca-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="417ca-108">Обратная связь</span><span class="sxs-lookup"><span data-stu-id="417ca-108">Feedback</span></span>

<span data-ttu-id="417ca-109">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="417ca-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
