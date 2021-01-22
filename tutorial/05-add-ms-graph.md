---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919531"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы включаете Microsoft Graph в приложение. Для этого приложения вы будете использовать [microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для вызова Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Реализация поставщика проверки подлинности

Для работы microsoft Graph SDK для Java требуется реализация интерфейса для `IAuthenticationProvider` его `GraphServiceClient` объекта.

1. Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **SimpleAuthProvider.java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Получить сведения о пользователе

1. Создайте файл **в каталоге ./graphtutorial/src/main/java/graphtutorial** **с именем Graph.java** и добавьте следующий код.

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

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
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. Добавьте следующий `import` выписки в верхней **части App.java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Добавьте следующий код в **App.java** сразу перед строкой, чтобы получить пользователя и выходные данные `Scanner input = new Scanner(System.in);` отображаемого имени пользователя.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Запустите приложение. После входа в приложение вас приветствует по имени.

## <a name="get-calendar-events-from-outlook"></a>Получить события календаря из Outlook

1. Добавьте в класс Graph.java следующую функцию для получения событий из `Graph` календаря пользователя. 

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Подумайте, что делает этот код.

- Будет вызван URL-адрес `/me/calendarview` .
  - `QueryOption` объекты используются для добавления и параметров, установки начала и окончания `startDateTime` `endDateTime` представления календаря.
  - Объект `QueryOption` используется для добавления параметра, `$orderby` сортировать результаты по времени начала.
  - Объект используется для добавления загона, в результате чего время начала и окончания настраивается в часовом `HeaderOption` `Prefer: outlook.timezone` поясе пользователя.
  - Функция ограничивает поля, возвращаемые для каждого события, только теми, которые будут `select` фактически использовать приложение.
  - Функция ограничивает число событий в отклике не `top` более 25.
- Функция используется для запроса дополнительных страниц результатов, если за текущую неделю имеется более `getNextPage` 25 событий.

1. Создайте файл **в каталоге ./graphtutorial/src/main/java/graphtutorial** **с именем GraphToIana.java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Этот класс реализует простой подыскак для преобразования имен часового пояса Windows в идентификаторы IANA и создания **ZoneId** на основе имени часового пояса Windows.

## <a name="display-the-results"></a>Отображение результатов

1. Добавьте следующие утверждения `import` в **App.java.**

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. Добавьте в класс следующую функцию для формата свойств `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в удобном для пользователя формате.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Добавьте в класс следующую функцию для получения событий пользователя и вывода `App` их в консоль.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Добавьте следующее сразу после `// List the calendar` комментария в `main` функции.

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. Сохраните все изменения, создайте приложение и запустите его. Выберите параметр **"Список событий** календаря", чтобы увидеть список событий пользователя.

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
