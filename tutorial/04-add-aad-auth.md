---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695795"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширит приложение от предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете Библиотеку проверки подлинности [Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.

1. Создайте новый каталог с именем **graphtutorial** в **каталоге ./src/main/resources.**

1. Создайте новый файл в **каталоге ./src/main/resources/graphtutorial** с именем **oAuth.properties** и добавьте следующий текст в этот файл. Замените созданный на портале `YOUR_APP_ID_HERE` Azure ID приложения.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    Значение содержит `app.scopes` области разрешений, которые требуется приложению.

    - **User.Read** позволяет приложению получить доступ к профилю пользователя.
    - **MailboxSettings.Read** позволяет приложению получать доступ к настройкам из почтового ящика пользователя, включая настроенный часовой пояс пользователя.
    - **Calendars.ReadWrite** позволяет приложению перечислять календарь пользователя и добавлять новые события в календарь.

    > [!IMPORTANT]
    > Если вы используете управление исходными данными, например git, сейчас самое время исключить файл **oAuth.properties** из-под контроля источника, чтобы не допустить случайной утечки вашего ID приложения.

1. Откройте **App.java** и добавьте следующие `import` утверждения.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить **файл oAuth.properties.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Реализация входа в систему

1. Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **Graph.java** и добавьте следующий код.

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

1. В **App.java** добавьте следующий код перед строкой, чтобы `Scanner input = new Scanner(System.in);` получить маркер доступа.

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. Добавьте следующую строку после `// Display access token` комментария.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Запустите приложение. Приложение отображает URL-адрес и код устройства.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Откройте браузер и просмотрите отображаемый URL-адрес. Введите предоставленный код и войдите. После завершения вернись в приложение и выберите **1. Отображение параметра маркера** доступа для отображения маркера доступа.

> [!TIP]
> Маркеры доступа для учетных записей microsoft work или school можно разбора для устранения неполадок по [https://jwt.ms](https://jwt.ms) данным . Маркеры доступа для личных учетных записей Майкрософт используют несвободный формат и не могут быть размыты.
