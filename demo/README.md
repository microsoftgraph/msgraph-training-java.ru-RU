---
ms.openlocfilehash: 725648d1b8518c17938c17c160c5799258dc1d92
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612353"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="68aad-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="68aad-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68aad-102">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="68aad-102">Prerequisites</span></span>

<span data-ttu-id="68aad-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="68aad-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="68aad-104">[Набор для разработки Java SE (ЖДК)](https://java.com/en/download/faq/develop.xml) и [Gradle](https://gradle.org/) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="68aad-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="68aad-105">Если у вас нет ЖДК или Gradle, посетите предыдущие ссылки для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="68aad-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="68aad-106">(**Примечание:** это руководство написано с опенждк версии 14.0.0.36 и Gradle 6,3.</span><span class="sxs-lookup"><span data-stu-id="68aad-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="68aad-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="68aad-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="68aad-108">Рабочая или учебная учетная запись Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="68aad-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="68aad-109">Если у вас нет учетной записи Майкрософт, вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="68aad-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="68aad-110">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="68aad-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="68aad-111">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="68aad-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="68aad-112">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="68aad-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="68aad-113">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="68aad-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="68aad-114">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="68aad-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="68aad-115">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="68aad-115">Select **New registration**.</span></span> <span data-ttu-id="68aad-116">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="68aad-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="68aad-117">Введите **имя** `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="68aad-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="68aad-118">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге**.</span><span class="sxs-lookup"><span data-stu-id="68aad-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="68aad-119">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="68aad-119">Leave **Redirect URI** empty.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="68aad-121">Нажмите кнопку **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="68aad-121">Choose **Register**.</span></span> <span data-ttu-id="68aad-122">На странице **учебника по Java Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="68aad-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="68aad-124">Выберите ссылку **Добавить URI перенаправления** .</span><span class="sxs-lookup"><span data-stu-id="68aad-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="68aad-125">На странице " **Перенаправление URI** " выберите раздел **Перенаправление URI для общедоступных клиентов (мобильный, Настольный)** .</span><span class="sxs-lookup"><span data-stu-id="68aad-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="68aad-126">Выберите `https://login.microsoftonline.com/common/oauth2/nativeclient` универсальный код ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="68aad-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Снимок экрана со страницей URI перенаправления](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="68aad-128">Найдите раздел **Тип клиента по умолчанию** и измените **приложение рассматривать как общедоступный клиент** на **Да**, а затем нажмите кнопку **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="68aad-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Снимок экрана: раздел "тип клиента по умолчанию"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="68aad-130">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="68aad-130">Configure the sample</span></span>

1. <span data-ttu-id="68aad-131">Переименуйте `oAuth.properties.example` файл в `oAuth.properties`.</span><span class="sxs-lookup"><span data-stu-id="68aad-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="68aad-132">Измените `oAuth.properties` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="68aad-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="68aad-133">Замените `YOUR_APP_ID_HERE` **идентификатором приложения** , полученным на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="68aad-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="68aad-134">Сборка и запуск примера</span><span class="sxs-lookup"><span data-stu-id="68aad-134">Build and run the sample</span></span>

<span data-ttu-id="68aad-135">В интерфейсе командной строки (CLI) перейдите в каталог проекта и выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="68aad-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
