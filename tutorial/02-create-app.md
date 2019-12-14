---
ms.openlocfilehash: 298c78d0626e16899de402b3e9b537c8b8386886
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018843"
---
<!-- markdownlint-disable MD002 MD041 -->

Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект. Выполните следующую команду, чтобы создать новый проект Мавен.

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> Можно ввести разные значения для идентификатора группы (`DgroupId` параметра) и идентификатора артефакта (`DartifactId` параметра), чем указано выше. В примере кода в этом руководстве предполагается, что используется `com.contoso` идентификатор группы. Если вы используете другое значение, не забудьте заменить `com.contoso` в любом образце кода идентификатором вашей группы.

При появлении соответствующего запроса подтвердите конфигурацию и дождитесь создания проекта. После создания проекта убедитесь, что он работает, выполнив следующие команды для упаковки и запуска приложения в утилите CLI.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

Если это сработает, приложение должно выводить `Hello World!`результаты. Перед перемещением добавьте дополнительные зависимости, которые будут использоваться позже.

- [Библиотека проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) для проверки подлинности пользователя и получения маркеров доступа.
- [Пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.
- [Привязка НОП SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) для отмены ведения журнала из MSAL.

Открыть **./графтуториал/пом.ксмл**. Добавьте следующий `<dependencies>` элемент в элемент.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.6.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>1.1.0</version>
</dependency>
```

В следующий раз при построении проекта Мавен загрузит эти зависимости.

## <a name="design-the-app"></a>Проектирование приложения

Откройте файл **./графтуториал/СРК/Маин/Жава/ком/Контосо/АПП.Жава** и замените его содержимое на приведенный ниже код.

```java
package com.contoso;

import java.util.InputMismatchException;
import java.util.Scanner;

/**
 * Graph Tutorial
 *
 */
public class App {
    public static void main(String[] args) {
        System.out.println("Java Graph Tutorial");
        System.out.println();

        Scanner input = new Scanner(System.in);

        int choice = -1;

        while (choice != 0) {
            System.out.println("Please choose one of the following options:");
            System.out.println("0. Exit");
            System.out.println("1. Display access token");
            System.out.println("2. List calendar events");

            try {
                choice = input.nextInt();
            } catch (InputMismatchException ex) {
                // Skip over non-integer input
                input.nextLine();
            }

            // Process user choice
            switch(choice) {
                case 0:
                    // Exit the program
                    System.out.println("Goodbye...");
                    break;
                case 1:
                    // Display access token
                    break;
                case 2:
                    // List the calendar
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        input.close();
    }
}
```

В результате будет реализовано базовое меню, в котором считывается выбор пользователя из командной строки.
