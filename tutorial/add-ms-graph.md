---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634768"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="49a97-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="49a97-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="49a97-102">Для этого приложения вы будете использовать [пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="49a97-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="49a97-103">Реализация поставщика проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="49a97-103">Implement an authentication provider</span></span>

<span data-ttu-id="49a97-104">Для работы с пакетом SDK Microsoft Graph для Java требуется `IAuthenticationProvider` реализация интерфейса для создания экземпляра `GraphServiceClient` объекта.</span><span class="sxs-lookup"><span data-stu-id="49a97-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span> <span data-ttu-id="49a97-105">Сначала создайте простой класс, чтобы добавить маркер доступа к исходящим запросам.</span><span class="sxs-lookup"><span data-stu-id="49a97-105">Start by creating a simple class to add the access token to outgoing requests.</span></span> <span data-ttu-id="49a97-106">Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/ком/Контосо** с именем **симплеауспровидер. Java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="49a97-106">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.authentication.IAuthenticationProvider;
import com.microsoft.graph.http.IHttpRequest;

/**
 * SimpleAuthProvider
 */
public class SimpleAuthProvider implements IAuthenticationProvider {

    private String accessToken = null;

    public SimpleAuthProvider(String accessToken) {
        this.accessToken = accessToken;
    }

    @Override
    public void authenticateRequest(IHttpRequest request) {
        // Add the access token in the Authorization header
        request.addHeader("Authorization", "Bearer " + accessToken);
    }
}
```

## <a name="get-user-details"></a><span data-ttu-id="49a97-107">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="49a97-107">Get user details</span></span>

<span data-ttu-id="49a97-108">Сначала добавьте новый класс, содержащий все функциональные возможности Graph.</span><span class="sxs-lookup"><span data-stu-id="49a97-108">First, add a new class to contain all of the Graph functionality.</span></span> <span data-ttu-id="49a97-109">Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/ком/Контосо** с именем **Graph. Java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="49a97-109">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Graph.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.logger.DefaultLogger;
import com.microsoft.graph.logger.LoggerLevel;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import java.util.LinkedList;
import java.util.List;import com.microsoft.graph.models.extensions.Event;import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
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

<span data-ttu-id="49a97-110">Добавьте следующий код в файл **app. Java** непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить пользователя и вывести отображаемое имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="49a97-110">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

<span data-ttu-id="49a97-111">Если вы запустите приложение сейчас, после входа в приложение введите имя.</span><span class="sxs-lookup"><span data-stu-id="49a97-111">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="49a97-112">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="49a97-112">Get calendar events from Outlook</span></span>

<span data-ttu-id="49a97-113">Добавьте указанные ниже `import` операторы в **Graph. Java**.</span><span class="sxs-lookup"><span data-stu-id="49a97-113">Add the following `import` statements to **Graph.java**.</span></span>

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

<span data-ttu-id="49a97-114">Добавьте указанную ниже функцию в `Graph` класс в **Graph. Java** , чтобы получить события из календаря пользователя.</span><span class="sxs-lookup"><span data-stu-id="49a97-114">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

```java
public static List<Event> getEvents(String accessToken) {
    ensureGraphClient(accessToken);

    // Use QueryOption to specify the $orderby query parameter
    final List<Option> options = new LinkedList<Option>();
    // Sort results by createdDateTime, get newest first
    options.add(new QueryOption("orderby", "createdDateTime DESC"));

    // GET /me/events
    IEventCollectionPage eventPage = graphClient
        .me()
        .events()
        .buildRequest(options)
        .select("subject,organizer,start,end")
        .get();

    return eventPage.getCurrentPage();
}
```

<span data-ttu-id="49a97-115">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="49a97-115">Consider what this code is doing.</span></span>

- <span data-ttu-id="49a97-116">URL-адрес, который будет вызываться — это `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="49a97-116">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="49a97-117">`select` Функция ограничит поля, возвращаемые для каждого события, только теми, которые приложение будет использовать в действительности.</span><span class="sxs-lookup"><span data-stu-id="49a97-117">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="49a97-118">A `QueryOption` используется для сортировки результатов по дате и времени создания, начиная с последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="49a97-118">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="49a97-119">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="49a97-119">Display the results</span></span>

<span data-ttu-id="49a97-120">Для начала добавьте следующие `import` операторы в **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="49a97-120">Start by adding the following `import` statements in **App.java**.</span></span>

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

<span data-ttu-id="49a97-121">Затем добавьте указанную ниже функцию в `App` класс, чтобы отформатировать свойства [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в понятный для пользователя формат.</span><span class="sxs-lookup"><span data-stu-id="49a97-121">Then add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

<span data-ttu-id="49a97-122">Затем добавьте приведенную ниже функцию в `App` класс, чтобы получить события пользователя и вывести их на консоль.</span><span class="sxs-lookup"><span data-stu-id="49a97-122">Next, add the following function to the `App` class to get the user's events and output them to the console.</span></span>

```java
private static void listCalendarEvents(String accessToken) {
    // Get the user's events
    List<Event> events = Graph.getEvents(accessToken);

    System.out.println("Events:");

    for (Event event : events) {
        System.out.println("Subject: " + event.subject);
        System.out.println("  Organizer: " + event.organizer.emailAddress.name);
        System.out.println("  Start: " + formatDateTimeTimeZone(event.start));
        System.out.println("  End: " + formatDateTimeTimeZone(event.end));
    }

    System.out.println();
}
```

<span data-ttu-id="49a97-123">Наконец, добавьте следующий код сразу после `// List the calendar` комментария в `main` функции.</span><span class="sxs-lookup"><span data-stu-id="49a97-123">Finally, add the following just after the `// List the calendar` comment in the `main` function.</span></span>

```java
listCalendarEvents(accessToken);
```

<span data-ttu-id="49a97-124">Сохраните все изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="49a97-124">Save all of your changes and run the app.</span></span> <span data-ttu-id="49a97-125">Выберите параметр **список событий календаря** , чтобы просмотреть список событий пользователя.</span><span class="sxs-lookup"><span data-stu-id="49a97-125">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
