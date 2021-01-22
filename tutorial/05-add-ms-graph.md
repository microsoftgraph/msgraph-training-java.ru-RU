---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919531"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f03ba-101">В этом упражнении вы включаете Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="f03ba-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="f03ba-102">Для этого приложения вы будете использовать [microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f03ba-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="f03ba-103">Реализация поставщика проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="f03ba-103">Implement an authentication provider</span></span>

<span data-ttu-id="f03ba-104">Для работы microsoft Graph SDK для Java требуется реализация интерфейса для `IAuthenticationProvider` его `GraphServiceClient` объекта.</span><span class="sxs-lookup"><span data-stu-id="f03ba-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="f03ba-105">Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **SimpleAuthProvider.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f03ba-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="f03ba-106">Получить сведения о пользователе</span><span class="sxs-lookup"><span data-stu-id="f03ba-106">Get user details</span></span>

1. <span data-ttu-id="f03ba-107">Создайте файл **в каталоге ./graphtutorial/src/main/java/graphtutorial** **с именем Graph.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f03ba-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="f03ba-108">Добавьте следующий `import` выписки в верхней **части App.java**.</span><span class="sxs-lookup"><span data-stu-id="f03ba-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="f03ba-109">Добавьте следующий код в **App.java** сразу перед строкой, чтобы получить пользователя и выходные данные `Scanner input = new Scanner(System.in);` отображаемого имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="f03ba-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="f03ba-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f03ba-110">Run the app.</span></span> <span data-ttu-id="f03ba-111">После входа в приложение вас приветствует по имени.</span><span class="sxs-lookup"><span data-stu-id="f03ba-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="f03ba-112">Получить события календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="f03ba-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="f03ba-113">Добавьте в класс Graph.java следующую функцию для получения событий из `Graph` календаря пользователя. </span><span class="sxs-lookup"><span data-stu-id="f03ba-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="f03ba-114">Подумайте, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="f03ba-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="f03ba-115">Будет вызван URL-адрес `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="f03ba-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="f03ba-116">`QueryOption` объекты используются для добавления и параметров, установки начала и окончания `startDateTime` `endDateTime` представления календаря.</span><span class="sxs-lookup"><span data-stu-id="f03ba-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="f03ba-117">Объект `QueryOption` используется для добавления параметра, `$orderby` сортировать результаты по времени начала.</span><span class="sxs-lookup"><span data-stu-id="f03ba-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="f03ba-118">Объект используется для добавления загона, в результате чего время начала и окончания настраивается в часовом `HeaderOption` `Prefer: outlook.timezone` поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="f03ba-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="f03ba-119">Функция ограничивает поля, возвращаемые для каждого события, только теми, которые будут `select` фактически использовать приложение.</span><span class="sxs-lookup"><span data-stu-id="f03ba-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="f03ba-120">Функция ограничивает число событий в отклике не `top` более 25.</span><span class="sxs-lookup"><span data-stu-id="f03ba-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="f03ba-121">Функция используется для запроса дополнительных страниц результатов, если за текущую неделю имеется более `getNextPage` 25 событий.</span><span class="sxs-lookup"><span data-stu-id="f03ba-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="f03ba-122">Создайте файл **в каталоге ./graphtutorial/src/main/java/graphtutorial** **с именем GraphToIana.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f03ba-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="f03ba-123">Этот класс реализует простой подыскак для преобразования имен часового пояса Windows в идентификаторы IANA и создания **ZoneId** на основе имени часового пояса Windows.</span><span class="sxs-lookup"><span data-stu-id="f03ba-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="f03ba-124">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="f03ba-124">Display the results</span></span>

1. <span data-ttu-id="f03ba-125">Добавьте следующие утверждения `import` в **App.java.**</span><span class="sxs-lookup"><span data-stu-id="f03ba-125">Add the following `import` statements in **App.java**.</span></span>

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

1. <span data-ttu-id="f03ba-126">Добавьте в класс следующую функцию для формата свойств `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в удобном для пользователя формате.</span><span class="sxs-lookup"><span data-stu-id="f03ba-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="f03ba-127">Добавьте в класс следующую функцию для получения событий пользователя и вывода `App` их в консоль.</span><span class="sxs-lookup"><span data-stu-id="f03ba-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="f03ba-128">Добавьте следующее сразу после `// List the calendar` комментария в `main` функции.</span><span class="sxs-lookup"><span data-stu-id="f03ba-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="f03ba-129">Сохраните все изменения, создайте приложение и запустите его.</span><span class="sxs-lookup"><span data-stu-id="f03ba-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="f03ba-130">Выберите параметр **"Список событий** календаря", чтобы увидеть список событий пользователя.</span><span class="sxs-lookup"><span data-stu-id="f03ba-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
