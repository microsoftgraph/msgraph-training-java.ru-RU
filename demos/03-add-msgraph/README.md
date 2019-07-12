---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634719"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="74325-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="74325-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74325-102">Необходимые условия</span><span class="sxs-lookup"><span data-stu-id="74325-102">Prerequisites</span></span>

<span data-ttu-id="74325-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="74325-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="74325-104">[Набор для разработки Java SE (ЖДК)](https://java.com/en/download/faq/develop.xml) и [Мавен](https://maven.apache.org/) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="74325-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="74325-105">Если у вас нет ЖДК или Мавен, посетите предыдущие ссылки для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="74325-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span> <span data-ttu-id="74325-106">(**Примечание:** это руководство было написано с помощью опенждк версии 12.0.1 и Мавен 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="74325-106">(**Note:** This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="74325-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="74325-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="74325-108">Рабочая или учебная учетная запись Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="74325-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="74325-109">Если у вас нет учетной записи Майкрософт, вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="74325-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="74325-110">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74325-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="74325-111">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="74325-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="74325-112">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="74325-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="74325-113">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="74325-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="74325-114">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="74325-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="74325-115">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="74325-115">Select **New registration**.</span></span> <span data-ttu-id="74325-116">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="74325-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="74325-117">Введите **имя** `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="74325-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="74325-118">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге**.</span><span class="sxs-lookup"><span data-stu-id="74325-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="74325-119">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="74325-119">Leave **Redirect URI** empty.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="74325-121">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="74325-121">Choose **Register**.</span></span> <span data-ttu-id="74325-122">На странице **учебника по Java Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="74325-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="74325-124">Выберите ссылку **Добавить URI перенаправления** .</span><span class="sxs-lookup"><span data-stu-id="74325-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="74325-125">На странице " **Перенаправление URI** " выберите раздел **Перенаправление URI для общедоступных клиентов (мобильный, Настольный)** .</span><span class="sxs-lookup"><span data-stu-id="74325-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="74325-126">Выберите `https://login.microsoftonline.com/common/oauth2/nativeclient` универсальный код ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="74325-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Снимок экрана со страницей URI перенаправления](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="74325-128">Найдите раздел **Тип клиента по умолчанию** и измените **приложение рассматривать как общедоступный клиент** на **Да**, а затем нажмите кнопку **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="74325-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Снимок экрана: раздел "тип клиента по умолчанию"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="74325-130">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="74325-130">Configure the sample</span></span>

1. <span data-ttu-id="74325-131">Переименуйте `oAuth.properties.example` файл в `oAuth.properties`.</span><span class="sxs-lookup"><span data-stu-id="74325-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="74325-132">Измените `oAuth.properties` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="74325-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="74325-133">Замените `YOUR_APP_ID_HERE` идентификатором **приложения** , полученным на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="74325-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="74325-134">Сборка и запуск примера</span><span class="sxs-lookup"><span data-stu-id="74325-134">Build and run the sample</span></span>

<span data-ttu-id="74325-135">В интерфейсе командной строки (CLI) перейдите в каталог проекта и выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="74325-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```
