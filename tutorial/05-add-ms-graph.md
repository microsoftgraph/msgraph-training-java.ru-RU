---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695809"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете включать Microsoft Graph в приложение. Для этого приложения для звонков в Microsoft Graph используется [SDK Microsoft Graph для Java.](https://github.com/microsoftgraph/msgraph-sdk-java)

## <a name="get-user-details"></a>Получение сведений о пользователе

1. Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **Graph.java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. Добавьте следующее `import` утверждение в верхней части **App.java.**

    ```java
    import com.microsoft.graph.models.User;
    ```

1. Добавьте следующий код в **App.java** незадолго до строки, чтобы получить имя пользователя и выйти из `Scanner input = new Scanner(System.in);` него.

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Запустите приложение. После входа в приложение приветствует вас по имени.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Добавьте следующую функцию `Graph` в класс **в Graph.java,** чтобы получить события из календаря пользователя.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Давайте посмотрим, что делает этот код.

- Вызывается URL-адрес `/me/calendarview`.
  - `QueryOption` объекты используются для добавления `startDateTime` `endDateTime` параметров и параметров, устанавливая начало и конец представления календаря.
  - Объект `QueryOption` используется для добавления `$orderby` параметра, сортировки результатов по времени начала.
  - Объект используется для добавления загона, в результате чего время начала и окончания должно быть скорректировано в `HeaderOption` `Prefer: outlook.timezone` часовой пояс пользователя.
  - Функция `select` ограничивает поля, возвращаемые для каждого события, только теми, которые приложение будет фактически использовать.
  - Функция ограничивает количество событий в ответе не `top` более 25.
- Функция используется для запроса дополнительных страниц результатов, если на текущей неделе происходит более `getNextPage` 25 событий.

1. Создайте новый файл в **каталоге ./graphtutorial/src/main/java/graphtutorial** с именем **GraphToIana.java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Этот класс реализует простой внешний вид, чтобы преобразовать имена часового пояса Windows в идентификаторы IANA и создать **ZoneId** на основе имени часового пояса Windows.

## <a name="display-the-results"></a>Отображение результатов

1. Добавьте следующие `import` утверждения в **App.java.**

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

1. Добавьте в класс следующую функцию, чтобы форматировать свойства `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) из Microsoft Graph в удобном для пользователя формате.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Добавьте в класс следующую функцию, чтобы получить события пользователя и выложить `App` их на консоль.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Добавьте следующее сразу после `// List the calendar` комментария в `main` функции.

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. Сохраните все изменения, создайте приложение и запустите его. Выберите параметр **"События календаря** списка", чтобы увидеть список событий пользователя.

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
