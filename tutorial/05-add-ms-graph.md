---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695809"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="25760-101">В этом упражнении вы будете включать Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="25760-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="25760-102">Для этого приложения для звонков в Microsoft Graph используется [SDK Microsoft Graph для Java.](https://github.com/microsoftgraph/msgraph-sdk-java)</span><span class="sxs-lookup"><span data-stu-id="25760-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="25760-103">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="25760-103">Get user details</span></span>

1. <span data-ttu-id="25760-104">Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **Graph.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="25760-104">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. <span data-ttu-id="25760-105">Добавьте следующее `import` утверждение в верхней части **App.java.**</span><span class="sxs-lookup"><span data-stu-id="25760-105">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.User;
    ```

1. <span data-ttu-id="25760-106">Добавьте следующий код в **App.java** незадолго до строки, чтобы получить имя пользователя и выйти из `Scanner input = new Scanner(System.in);` него.</span><span class="sxs-lookup"><span data-stu-id="25760-106">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="25760-107">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="25760-107">Run the app.</span></span> <span data-ttu-id="25760-108">После входа в приложение приветствует вас по имени.</span><span class="sxs-lookup"><span data-stu-id="25760-108">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="25760-109">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="25760-109">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="25760-110">Добавьте следующую функцию `Graph` в класс **в Graph.java,** чтобы получить события из календаря пользователя.</span><span class="sxs-lookup"><span data-stu-id="25760-110">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="25760-111">Давайте посмотрим, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="25760-111">Consider what this code is doing.</span></span>

- <span data-ttu-id="25760-112">Вызывается URL-адрес `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="25760-112">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="25760-113">`QueryOption` объекты используются для добавления `startDateTime` `endDateTime` параметров и параметров, устанавливая начало и конец представления календаря.</span><span class="sxs-lookup"><span data-stu-id="25760-113">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="25760-114">Объект `QueryOption` используется для добавления `$orderby` параметра, сортировки результатов по времени начала.</span><span class="sxs-lookup"><span data-stu-id="25760-114">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="25760-115">Объект используется для добавления загона, в результате чего время начала и окончания должно быть скорректировано в `HeaderOption` `Prefer: outlook.timezone` часовой пояс пользователя.</span><span class="sxs-lookup"><span data-stu-id="25760-115">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="25760-116">Функция `select` ограничивает поля, возвращаемые для каждого события, только теми, которые приложение будет фактически использовать.</span><span class="sxs-lookup"><span data-stu-id="25760-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="25760-117">Функция ограничивает количество событий в ответе не `top` более 25.</span><span class="sxs-lookup"><span data-stu-id="25760-117">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="25760-118">Функция используется для запроса дополнительных страниц результатов, если на текущей неделе происходит более `getNextPage` 25 событий.</span><span class="sxs-lookup"><span data-stu-id="25760-118">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="25760-119">Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **GraphToIana.java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="25760-119">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="25760-120">Этот класс реализует простой внешний вид, чтобы преобразовать имена часового пояса Windows в идентификаторы IANA и создать **ZoneId** на основе имени часового пояса Windows.</span><span class="sxs-lookup"><span data-stu-id="25760-120">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="25760-121">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="25760-121">Display the results</span></span>

1. <span data-ttu-id="25760-122">Добавьте следующие `import` утверждения в **App.java.**</span><span class="sxs-lookup"><span data-stu-id="25760-122">Add the following `import` statements in **App.java**.</span></span>

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
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. <span data-ttu-id="25760-123">Добавьте в класс следующую функцию, чтобы форматировать свойства `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в удобном для пользователя формате.</span><span class="sxs-lookup"><span data-stu-id="25760-123">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="25760-124">Добавьте в класс следующую функцию, чтобы получить события пользователя и выложить `App` их на консоль.</span><span class="sxs-lookup"><span data-stu-id="25760-124">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="25760-125">Добавьте следующее сразу после `// List the calendar` комментария в `main` функции.</span><span class="sxs-lookup"><span data-stu-id="25760-125">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="25760-126">Сохраните все изменения, создайте приложение и запустите его.</span><span class="sxs-lookup"><span data-stu-id="25760-126">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="25760-127">Выберите параметр **"События календаря** списка", чтобы увидеть список событий пользователя.</span><span class="sxs-lookup"><span data-stu-id="25760-127">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
