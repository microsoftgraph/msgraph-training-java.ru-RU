---
ms.openlocfilehash: f13ee9baa26bb487d5b50bd966e40cee9ba64378
ms.sourcegitcommit: 22ec2c0e4803b82c9d360f3609f8b15ba5d017ca
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "43940478"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="57717-101">В этом руководстве рассказывается, как создать консольное приложение Java, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="57717-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="57717-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="57717-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57717-103">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="57717-103">Prerequisites</span></span>

<span data-ttu-id="57717-104">Прежде чем приступить к работе с этим руководством, на компьютере для разработки должен быть установлен [набор средств разработки Java SE (ЖДК)](https://java.com/en/download/faq/develop.xml) и [Gradle](https://gradle.org/) .</span><span class="sxs-lookup"><span data-stu-id="57717-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="57717-105">Если у вас нет ЖДК или Gradle, посетите предыдущие ссылки для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="57717-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="57717-106">Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="57717-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="57717-107">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="57717-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="57717-108">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="57717-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="57717-109">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="57717-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="57717-110">Это руководство было написано с Опенждк версии 14.0.0.36 и Gradle 6,3.</span><span class="sxs-lookup"><span data-stu-id="57717-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="57717-111">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="57717-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="57717-112">Отзывы</span><span class="sxs-lookup"><span data-stu-id="57717-112">Feedback</span></span>

<span data-ttu-id="57717-113">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="57717-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
