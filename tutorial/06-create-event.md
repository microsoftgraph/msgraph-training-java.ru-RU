---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661138"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы добавим возможность создания событий в календаре пользователя.

1. Откройте **./graphtutorial/src/main/java/graphtutorial/Graph.java** и добавьте следующую функцию в **класс Graph.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Откройте **./graphtutorial/src/main/java/graphtutorial/App.java** и добавьте следующую функцию в **класс App.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Эта функция запросит у пользователя тему, участников, начало, конец и тело, а затем использует эти значения для `Graph.createEvent` вызова.

1. Добавьте следующее сразу после `// Create a new event` комментария в `Main` функции.

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. Сохраните все изменения и запустите приложение. Выберите параметр **"Добавить событие".** Ответьте на запросы, чтобы создать новое событие в календаре пользователя.
