---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612070"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы добавите Microsoft Graph в приложение. Для этого приложения вы будете использовать [пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Реализация поставщика проверки подлинности

Для работы с пакетом SDK Microsoft Graph для Java требуется `IAuthenticationProvider` реализация интерфейса для создания экземпляра `GraphServiceClient` объекта.

1. Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **симплеауспровидер. Java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Получение сведений о пользователе

1. Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **Graph. Java** и добавьте следующий код.

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .get();

            return me;
        }
    }
    ```

1. Добавьте следующий `import` оператор в начало **app. Java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Добавьте следующий код в файл **app. Java** непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить пользователя и вывести отображаемое имя пользователя.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. Запустите приложение. После входа в приложение зайдите по имени.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Добавьте указанную ниже функцию в `Graph` класс в **Graph. Java** , чтобы получить события из календаря пользователя.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Рассмотрите, что делает этот код.

- URL-адрес, который будет вызываться — это `/me/events`.
- `select` Функция ограничит поля, возвращаемые для каждого события, только теми, которые приложение будет использовать в действительности.
- A `QueryOption` используется для сортировки результатов по дате и времени создания, начиная с последнего элемента.

## <a name="display-the-results"></a>Отображение результатов

1. Добавьте следующие `import` операторы в **app. Java**.

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. Добавьте указанную ниже функцию в `App` класс, чтобы отформатировать свойства [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в понятный для пользователя формат.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Добавьте указанную ниже функцию в `App` класс, чтобы получить события пользователя и вывести их на консоль.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Добавьте следующий код сразу после `// List the calendar` комментария в `main` функции.

    ```java
    listCalendarEvents(accessToken);
    ```

1. Сохраните все изменения, создайте приложение и запустите его. Выберите параметр **список событий календаря** , чтобы просмотреть список событий пользователя.

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
