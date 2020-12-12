---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661138"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="86874-101">В этом разделе вы добавим возможность создания событий в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="86874-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="86874-102">Откройте **./graphtutorial/src/main/java/graphtutorial/Graph.java** и добавьте следующую функцию в **класс Graph.**</span><span class="sxs-lookup"><span data-stu-id="86874-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="86874-103">Откройте **./graphtutorial/src/main/java/graphtutorial/App.java** и добавьте следующую функцию в **класс App.**</span><span class="sxs-lookup"><span data-stu-id="86874-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="86874-104">Эта функция запросит у пользователя тему, участников, начало, конец и тело, а затем использует эти значения для `Graph.createEvent` вызова.</span><span class="sxs-lookup"><span data-stu-id="86874-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="86874-105">Добавьте следующее сразу после `// Create a new event` комментария в `Main` функции.</span><span class="sxs-lookup"><span data-stu-id="86874-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="86874-106">Сохраните все изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="86874-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="86874-107">Выберите параметр **"Добавить событие".**</span><span class="sxs-lookup"><span data-stu-id="86874-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="86874-108">Ответьте на запросы, чтобы создать новое событие в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="86874-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
