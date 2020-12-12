---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661083"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы создадим базовое консольное приложение Java.

1. Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект. Чтобы создать новый проект Gradle, запустите следующую команду.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. После создания проекта убедитесь, что он работает, выполив следующую команду для запуска приложения в CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Если он работает, приложение должно вы выходные `Hello World.` данные .

## <a name="install-dependencies"></a>Установка зависимостей

Прежде чем двигаться дальше, добавьте некоторые дополнительные зависимости, которые вы будете использовать позже.

- [Microsoft Authentication Library (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) для проверки подлинности пользователя и получения маркеров доступа.
- [Microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для вызова Microsoft Graph.
- [Привязка SLF4J NOP для](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) подавления ведения журнала из MSAL.

1. Откройте **./build.gradle.** `dependencies`Обновим раздел, чтобы добавить эти зависимости.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. Добавьте следующее в конец **./build.gradle.**

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

При следующей сборке проекта Gradle загрузит эти зависимости.

## <a name="design-the-app"></a>Проектирование приложения

1. Откройте файл **./src/main/java/graphtutorial/App.java** и замените его содержимое следующим:

    ```java
    package graphtutorial;

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

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
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    При этом реализуется базовое меню и считыется выбор пользователя из командной строки.
