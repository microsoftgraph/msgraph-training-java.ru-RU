---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661069"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2e062-101">В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e062-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="2e062-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2e062-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="2e062-103">На этом этапе вы интегрируете библиотеку проверки подлинности [Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.</span><span class="sxs-lookup"><span data-stu-id="2e062-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="2e062-104">Создайте новый каталог с именем **graphtutorial** в **каталоге ./src/main/resources.**</span><span class="sxs-lookup"><span data-stu-id="2e062-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="2e062-105">Создайте файл с именем **oAuth.properties** в **каталоге ./src/main/resources/graphtutorial** и добавьте в него следующий текст.</span><span class="sxs-lookup"><span data-stu-id="2e062-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="2e062-106">Замените `YOUR_APP_ID_HERE` ид приложения, созданный на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="2e062-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="2e062-107">Значение содержит `app.scopes` области разрешений, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="2e062-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="2e062-108">**User.Read** позволяет приложению получить доступ к профилю пользователя.</span><span class="sxs-lookup"><span data-stu-id="2e062-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="2e062-109">**MailboxSettings.Read** позволяет приложению получать доступ к настройкам из почтового ящика пользователя, включая настроенный часовой пояс пользователя.</span><span class="sxs-lookup"><span data-stu-id="2e062-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="2e062-110">**Calendars.ReadWrite** позволяет приложению перечислять календарь пользователя и добавлять в него новые события.</span><span class="sxs-lookup"><span data-stu-id="2e062-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2e062-111">Если вы используете управление исходным кодом, например git, сейчас хорошо бы исключить файл **oAuth.properties** из системы управления исходным кодом, чтобы не допустить случайной утечки вашего ИД приложения.</span><span class="sxs-lookup"><span data-stu-id="2e062-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="2e062-112">Откройте **App.java** и добавьте следующие `import` утверждения.</span><span class="sxs-lookup"><span data-stu-id="2e062-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="2e062-113">Добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить **файл oAuth.properties.**</span><span class="sxs-lookup"><span data-stu-id="2e062-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="2e062-114">Реализация входов</span><span class="sxs-lookup"><span data-stu-id="2e062-114">Implement sign-in</span></span>

1. <span data-ttu-id="2e062-115">Создайте файл с именем **Authentication.java** в **каталоге ./graphtutorial/src/main/java/graphtutorial** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="2e062-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="2e062-116">В **App.java** добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="2e062-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="2e062-117">Добавьте следующую строку после `// Display access token` комментария.</span><span class="sxs-lookup"><span data-stu-id="2e062-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="2e062-118">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="2e062-118">Run the app.</span></span> <span data-ttu-id="2e062-119">Приложение отображает URL-адрес и код устройства.</span><span class="sxs-lookup"><span data-stu-id="2e062-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="2e062-120">Откройте браузер и перейдите по отображаемого URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="2e062-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="2e062-121">Введите предоставленный код и войдите в нее.</span><span class="sxs-lookup"><span data-stu-id="2e062-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="2e062-122">После завершения вернись к приложению и выберите **1. Отображение параметра маркера** доступа для отображения маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="2e062-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="2e062-123">Маркеры доступа для учетных записей майкрософт для работы или учебного заведения можно разбор для устранения неполадок в [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="2e062-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="2e062-124">Маркеры доступа для личных учетных записей Майкрософт используют собственный формат и не могут быть разчетными.</span><span class="sxs-lookup"><span data-stu-id="2e062-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
