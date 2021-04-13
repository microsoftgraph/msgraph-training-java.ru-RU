---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695788"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы добавим возможность создания событий в календаре пользователя.

1. Откройте **./graphtutorial/src/main/java/graphtutorial/Graph.java** и добавьте следующую функцию в **класс Graph.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Откройте **./graphtutorial/src/main/java/graphtutorial/App.java** и добавьте следующую функцию в **класс App.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Эта функция подсказывая пользователю для субъекта, участников, запуска, конца и тела, а затем использует эти значения для вызова `Graph.createEvent` .

1. Добавьте следующее сразу после `// Create a new event` комментария в `Main` функции.

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. Сохраните все изменения и запустите приложение. Выберите параметр **Добавить событие.** Откликайся на запросы о создании нового события в календаре пользователя.
