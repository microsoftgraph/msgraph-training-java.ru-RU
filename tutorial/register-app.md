---
ms.openlocfilehash: 97763f135ab7529cfbcebf2eaf4d4d98d234fe10
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634733"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f2422-101">В этом упражнении вы создадите новое приложение Azure AD с помощью центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f2422-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="f2422-102">Откройте браузер и перейдите в [Центр администрирования Azure Active Directory](https://aad.portal.azure.com) и войдите с помощью **личной учетной записи** (т.е. учетной записи Майкрософт) или **рабочей или учебной учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="f2422-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="f2422-103">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="f2422-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="f2422-104">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="f2422-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="f2422-105">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="f2422-105">Select **New registration**.</span></span> <span data-ttu-id="f2422-106">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f2422-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="f2422-107">Введите **имя** `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="f2422-107">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="f2422-108">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге**.</span><span class="sxs-lookup"><span data-stu-id="f2422-108">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="f2422-109">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="f2422-109">Leave **Redirect URI** empty.</span></span>

    ![Снимок страницы "регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="f2422-111">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="f2422-111">Choose **Register**.</span></span> <span data-ttu-id="f2422-112">На странице **учебника по Java Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="f2422-112">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](./images/aad-application-id.png)

1. <span data-ttu-id="f2422-114">Выберите ссылку **Добавить URI перенаправления** .</span><span class="sxs-lookup"><span data-stu-id="f2422-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="f2422-115">На странице " **Перенаправление URI** " выберите раздел **Перенаправление URI для общедоступных клиентов (мобильный, Настольный)** .</span><span class="sxs-lookup"><span data-stu-id="f2422-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="f2422-116">Выберите `https://login.microsoftonline.com/common/oauth2/nativeclient` универсальный код ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="f2422-116">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Снимок экрана со страницей URI перенаправления](./images/aad-redirect-uris.png)

1. <span data-ttu-id="f2422-118">Найдите раздел **Тип клиента по умолчанию** и измените **приложение рассматривать как общедоступный клиент** на **Да**, а затем нажмите кнопку **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="f2422-118">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Снимок экрана: раздел "тип клиента по умолчанию"](./images/aad-default-client-type.png)
