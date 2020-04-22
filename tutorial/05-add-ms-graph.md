---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612070"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="33bc4-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="33bc4-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="33bc4-102">Для этого приложения вы будете использовать [пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="33bc4-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="33bc4-103">Реализация поставщика проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="33bc4-103">Implement an authentication provider</span></span>

<span data-ttu-id="33bc4-104">Для работы с пакетом SDK Microsoft Graph для Java требуется `IAuthenticationProvider` реализация интерфейса для создания экземпляра `GraphServiceClient` объекта.</span><span class="sxs-lookup"><span data-stu-id="33bc4-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="33bc4-105">Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **симплеауспровидер. Java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="33bc4-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="33bc4-106">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="33bc4-106">Get user details</span></span>

1. <span data-ttu-id="33bc4-107">Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **Graph. Java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="33bc4-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="33bc4-108">Добавьте следующий `import` оператор в начало **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="33bc4-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="33bc4-109">Добавьте следующий код в файл **app. Java** непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить пользователя и вывести отображаемое имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="33bc4-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. <span data-ttu-id="33bc4-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="33bc4-110">Run the app.</span></span> <span data-ttu-id="33bc4-111">После входа в приложение зайдите по имени.</span><span class="sxs-lookup"><span data-stu-id="33bc4-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="33bc4-112">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="33bc4-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="33bc4-113">Добавьте указанную ниже функцию в `Graph` класс в **Graph. Java** , чтобы получить события из календаря пользователя.</span><span class="sxs-lookup"><span data-stu-id="33bc4-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="33bc4-114">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="33bc4-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="33bc4-115">URL-адрес, который будет вызываться — это `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="33bc4-115">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="33bc4-116">`select` Функция ограничит поля, возвращаемые для каждого события, только теми, которые приложение будет использовать в действительности.</span><span class="sxs-lookup"><span data-stu-id="33bc4-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="33bc4-117">A `QueryOption` используется для сортировки результатов по дате и времени создания, начиная с последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="33bc4-117">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="33bc4-118">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="33bc4-118">Display the results</span></span>

1. <span data-ttu-id="33bc4-119">Добавьте следующие `import` операторы в **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="33bc4-119">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="33bc4-120">Добавьте указанную ниже функцию в `App` класс, чтобы отформатировать свойства [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в понятный для пользователя формат.</span><span class="sxs-lookup"><span data-stu-id="33bc4-120">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="33bc4-121">Добавьте указанную ниже функцию в `App` класс, чтобы получить события пользователя и вывести их на консоль.</span><span class="sxs-lookup"><span data-stu-id="33bc4-121">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="33bc4-122">Добавьте следующий код сразу после `// List the calendar` комментария в `main` функции.</span><span class="sxs-lookup"><span data-stu-id="33bc4-122">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="33bc4-123">Сохраните все изменения, создайте приложение и запустите его.</span><span class="sxs-lookup"><span data-stu-id="33bc4-123">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="33bc4-124">Выберите параметр **список событий календаря** , чтобы просмотреть список событий пользователя.</span><span class="sxs-lookup"><span data-stu-id="33bc4-124">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
