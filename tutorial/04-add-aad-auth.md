---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695795"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f1b13-101">В этом упражнении вы расширит приложение от предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f1b13-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="f1b13-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f1b13-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="f1b13-103">На этом этапе вы интегрируете Библиотеку проверки подлинности [Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.</span><span class="sxs-lookup"><span data-stu-id="f1b13-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="f1b13-104">Создайте новый каталог с именем **graphtutorial** в **каталоге ./src/main/resources.**</span><span class="sxs-lookup"><span data-stu-id="f1b13-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="f1b13-105">Создайте новый файл в **каталоге ./src/main/resources/graphtutorial** с именем **oAuth.properties** и добавьте следующий текст в этот файл.</span><span class="sxs-lookup"><span data-stu-id="f1b13-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="f1b13-106">Замените созданный на портале `YOUR_APP_ID_HERE` Azure ID приложения.</span><span class="sxs-lookup"><span data-stu-id="f1b13-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="f1b13-107">Значение содержит `app.scopes` области разрешений, которые требуется приложению.</span><span class="sxs-lookup"><span data-stu-id="f1b13-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="f1b13-108">**User.Read** позволяет приложению получить доступ к профилю пользователя.</span><span class="sxs-lookup"><span data-stu-id="f1b13-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="f1b13-109">**MailboxSettings.Read** позволяет приложению получать доступ к настройкам из почтового ящика пользователя, включая настроенный часовой пояс пользователя.</span><span class="sxs-lookup"><span data-stu-id="f1b13-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="f1b13-110">**Calendars.ReadWrite** позволяет приложению перечислять календарь пользователя и добавлять новые события в календарь.</span><span class="sxs-lookup"><span data-stu-id="f1b13-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f1b13-111">Если вы используете управление исходными данными, например git, сейчас самое время исключить файл **oAuth.properties** из-под контроля источника, чтобы не допустить случайной утечки вашего ID приложения.</span><span class="sxs-lookup"><span data-stu-id="f1b13-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="f1b13-112">Откройте **App.java** и добавьте следующие `import` утверждения.</span><span class="sxs-lookup"><span data-stu-id="f1b13-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="f1b13-113">Добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить **файл oAuth.properties.**</span><span class="sxs-lookup"><span data-stu-id="f1b13-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="f1b13-114">Реализация входа в систему</span><span class="sxs-lookup"><span data-stu-id="f1b13-114">Implement sign-in</span></span>

1. <span data-ttu-id="f1b13-115">Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **Graph.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f1b13-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.net.URL;
    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import okhttp3.Request;

    import com.azure.identity.DeviceCodeCredential;
    import com.azure.identity.DeviceCodeCredentialBuilder;

    import com.microsoft.graph.authentication.TokenCredentialAuthProvider;
    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.Attendee;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.EmailAddress;
    import com.microsoft.graph.models.Event;
    import com.microsoft.graph.models.ItemBody;
    import com.microsoft.graph.models.User;
    import com.microsoft.graph.models.AttendeeType;
    import com.microsoft.graph.models.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.GraphServiceClient;
    import com.microsoft.graph.requests.EventCollectionPage;
    import com.microsoft.graph.requests.EventCollectionRequestBuilder;

    public class Graph {

        private static GraphServiceClient<Request> graphClient = null;
        private static TokenCredentialAuthProvider authProvider = null;

        public static void initializeGraphAuth(String applicationId, List<String> scopes) {
            // Create the auth provider
            final DeviceCodeCredential credential = new DeviceCodeCredentialBuilder()
                .clientId(applicationId)
                .challengeConsumer(challenge -> System.out.println(challenge.getMessage()))
                .build();

            authProvider = new TokenCredentialAuthProvider(scopes, credential);

            // Create default logger to only log errors
            DefaultLogger logger = new DefaultLogger();
            logger.setLoggingLevel(LoggerLevel.ERROR);

            // Build a Graph client
            graphClient = GraphServiceClient.builder()
                .authenticationProvider(authProvider)
                .logger(logger)
                .buildClient();
        }

        public static String getUserAccessToken()
        {
            try {
                URL meUrl = new URL("https://graph.microsoft.com/v1.0/me");
                return authProvider.getAuthorizationTokenAsync(meUrl).get();
            } catch(Exception ex) {
                return null;
            }
        }
    }
    ```

1. <span data-ttu-id="f1b13-116">В **App.java** добавьте следующий код перед строкой, чтобы `Scanner input = new Scanner(System.in);` получить маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="f1b13-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. <span data-ttu-id="f1b13-117">Добавьте следующую строку после `// Display access token` комментария.</span><span class="sxs-lookup"><span data-stu-id="f1b13-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="f1b13-118">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f1b13-118">Run the app.</span></span> <span data-ttu-id="f1b13-119">Приложение отображает URL-адрес и код устройства.</span><span class="sxs-lookup"><span data-stu-id="f1b13-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="f1b13-120">Откройте браузер и просмотрите отображаемый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="f1b13-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="f1b13-121">Введите предоставленный код и войдите.</span><span class="sxs-lookup"><span data-stu-id="f1b13-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="f1b13-122">После завершения вернись в приложение и выберите **1. Отображение параметра маркера** доступа для отображения маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="f1b13-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="f1b13-123">Маркеры доступа для учетных записей microsoft work или school можно разбора для устранения неполадок по [https://jwt.ms](https://jwt.ms) данным .</span><span class="sxs-lookup"><span data-stu-id="f1b13-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="f1b13-124">Маркеры доступа для личных учетных записей Майкрософт используют несвободный формат и не могут быть размыты.</span><span class="sxs-lookup"><span data-stu-id="f1b13-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
