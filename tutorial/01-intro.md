---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661076"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="125fa-101">В этом руководстве рассказывается, как создать консольное приложение Java, использующее API Microsoft Graph для получения сведений календаря для пользователя.</span><span class="sxs-lookup"><span data-stu-id="125fa-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="125fa-102">Если вы предпочитаете просто скачать завершенный учебник, вы можете скачать или клонировать [репозиторий GitHub.](https://github.com/microsoftgraph/msgraph-training-java)</span><span class="sxs-lookup"><span data-stu-id="125fa-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="125fa-103">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="125fa-103">Prerequisites</span></span>

<span data-ttu-id="125fa-104">Прежде чем приступить к этому учебнику, на компьютере разработчика должны быть установлены пакет java [SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) и [Gradle.](https://gradle.org/)</span><span class="sxs-lookup"><span data-stu-id="125fa-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="125fa-105">Если у вас нет JDK или Gradle, посетите предыдущие ссылки для скачивания.</span><span class="sxs-lookup"><span data-stu-id="125fa-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="125fa-106">У вас также должна быть личная учетная запись Майкрософт с почтовым Outlook.com или учетная запись Майкрософт для работы или учебного заведения.</span><span class="sxs-lookup"><span data-stu-id="125fa-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="125fa-107">Если у вас нет учетной записи Майкрософт, существует несколько вариантов получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="125fa-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="125fa-108">Вы можете [зарегистрироваться для новой личной учетной записи Майкрософт.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="125fa-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="125fa-109">Вы можете зарегистрироваться в программе для разработчиков [Office 365,](https://developer.microsoft.com/office/dev-program) чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="125fa-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="125fa-110">Это руководство было написано с помощью OpenJDK версии 14.0.0.36 и Gradle 6.7.1.</span><span class="sxs-lookup"><span data-stu-id="125fa-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="125fa-111">Действия в этом руководстве могут работать с другими версиями, но не были протестированы.</span><span class="sxs-lookup"><span data-stu-id="125fa-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="125fa-112">Отзывы</span><span class="sxs-lookup"><span data-stu-id="125fa-112">Feedback</span></span>

<span data-ttu-id="125fa-113">Поделитесь с этим учебником отзывами в [репозитории GitHub.](https://github.com/microsoftgraph/msgraph-training-java)</span><span class="sxs-lookup"><span data-stu-id="125fa-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
