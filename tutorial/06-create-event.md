---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695788"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9f75-101">В этом разделе вы добавим возможность создания событий в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="b9f75-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="b9f75-102">Откройте **./graphtutorial/src/main/java/graphtutorial/Graph.java** и добавьте следующую функцию в **класс Graph.**</span><span class="sxs-lookup"><span data-stu-id="b9f75-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="b9f75-103">Откройте **./graphtutorial/src/main/java/graphtutorial/App.java** и добавьте следующую функцию в **класс App.**</span><span class="sxs-lookup"><span data-stu-id="b9f75-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="b9f75-104">Эта функция подсказывая пользователю для субъекта, участников, запуска, конца и тела, а затем использует эти значения для вызова `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="b9f75-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="b9f75-105">Добавьте следующее сразу после `// Create a new event` комментария в `Main` функции.</span><span class="sxs-lookup"><span data-stu-id="b9f75-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="b9f75-106">Сохраните все изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="b9f75-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="b9f75-107">Выберите параметр **Добавить событие.**</span><span class="sxs-lookup"><span data-stu-id="b9f75-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="b9f75-108">Откликайся на запросы о создании нового события в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="b9f75-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
