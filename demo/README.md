---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661055"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="11b2b-101">Как запустить завершенный проект</span><span class="sxs-lookup"><span data-stu-id="11b2b-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11b2b-102">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="11b2b-102">Prerequisites</span></span>

<span data-ttu-id="11b2b-103">Чтобы запустить завершенный проект в этой папке, необходимо следующее:</span><span class="sxs-lookup"><span data-stu-id="11b2b-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="11b2b-104">[Пакет java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) и [Gradle,](https://gradle.org/) установленные на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="11b2b-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="11b2b-105">Если у вас нет JDK или Gradle, посетите предыдущие ссылки для скачивания.</span><span class="sxs-lookup"><span data-stu-id="11b2b-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="11b2b-106">(**Примечание.** Это руководство было написано с помощью OpenJDK версии 14.0.0.36 и Gradle 6.7.1.</span><span class="sxs-lookup"><span data-stu-id="11b2b-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="11b2b-107">Действия в этом руководстве могут работать с другими версиями, но не были протестированы.)</span><span class="sxs-lookup"><span data-stu-id="11b2b-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="11b2b-108">Учетная запись Майкрософт для работы или учебного заведения.</span><span class="sxs-lookup"><span data-stu-id="11b2b-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="11b2b-109">Если у вас нет учетной записи Майкрософт, вы можете зарегистрироваться в программе для разработчиков [Office 365,](https://developer.microsoft.com/office/dev-program) чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="11b2b-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="11b2b-110">Регистрация веб-приложения в Центре администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="11b2b-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="11b2b-111">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11b2b-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="11b2b-112">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="11b2b-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="11b2b-113">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="11b2b-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="11b2b-114">Снимок экрана с регистрацией приложений</span><span class="sxs-lookup"><span data-stu-id="11b2b-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="11b2b-115">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="11b2b-115">Select **New registration**.</span></span> <span data-ttu-id="11b2b-116">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="11b2b-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="11b2b-117">Введите **имя** `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="11b2b-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="11b2b-118">Установите **поддерживаемые типы учетных** **записей для учетных записей в любом каталоге организации.**</span><span class="sxs-lookup"><span data-stu-id="11b2b-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="11b2b-119">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="11b2b-119">Leave **Redirect URI** empty.</span></span>

    ![Снимок экрана: страница "Регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="11b2b-121">Нажмите кнопку **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="11b2b-121">Choose **Register**.</span></span> <span data-ttu-id="11b2b-122">На странице **учебника по Java Graph** скопируйте значение ИД приложения **(клиента)** и сохраните его, оно потребуется вам на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="11b2b-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана: ИД нового приложения для регистрации](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="11b2b-124">Выберите **ссылку "Добавить URI перенаправления".**</span><span class="sxs-lookup"><span data-stu-id="11b2b-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="11b2b-125">На странице **ОКРИС** перенаправления найдите раздел "Рекомендуемые URIS перенаправления для общедоступных клиентов **(мобильных, настольных)** ".</span><span class="sxs-lookup"><span data-stu-id="11b2b-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="11b2b-126">Выберите `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="11b2b-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Снимок экрана: страница ОКРЕС перенаправления](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="11b2b-128">Найдите **раздел "Тип клиента** по умолчанию" и измените приложение **"Обрабатывать** как общедоступный" на **"Да"** и выберите **"Сохранить".**</span><span class="sxs-lookup"><span data-stu-id="11b2b-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Снимок экрана: раздел "Тип клиента по умолчанию"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="11b2b-130">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="11b2b-130">Configure the sample</span></span>

1. <span data-ttu-id="11b2b-131">Переименуем `oAuth.properties.example` файл в `oAuth.properties` .</span><span class="sxs-lookup"><span data-stu-id="11b2b-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="11b2b-132">`oAuth.properties`Отредактируете файл и внести следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="11b2b-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="11b2b-133">`YOUR_APP_ID_HERE`Замените **ид приложения,** который вы получили с портала регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="11b2b-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="11b2b-134">Сборка и запуск примера</span><span class="sxs-lookup"><span data-stu-id="11b2b-134">Build and run the sample</span></span>

<span data-ttu-id="11b2b-135">В интерфейсе командной строки перейдите в каталог проекта и запустите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="11b2b-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
