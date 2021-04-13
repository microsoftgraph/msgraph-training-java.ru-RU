---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695802"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы создадим базовое приложение для консоли Java.

1. Откройте интерфейс командной строки (CLI) в каталоге, в котором необходимо создать проект. Запустите следующую команду, чтобы создать новый проект Gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. После создания проекта убедитесь, что он работает, выполив следующую команду для запуска приложения в CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Если оно работает, приложение должно выводить `Hello World.` .

## <a name="install-dependencies"></a>Установка зависимостей

Прежде чем двигаться дальше, добавьте дополнительные зависимости, которые вы будете использовать позже.

- [Клиентская библиотека Azure Identity для Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) для проверки подлинности пользователя и получения маркеров доступа.
- [Microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для звонков в Microsoft Graph.

1. Откройте **./build.gradle**. `dependencies`Обнови раздел, чтобы добавить эти зависимости.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. Добавьте следующее в конец **./build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

При следующем создании проекта Gradle загрузит эти зависимости.

## <a name="design-the-app"></a>Проектирование приложения

1. Откройте **файл ./src/main/java/graphtutorial/App.java** и замените его содержимое следующим.

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

    Это реализует основное меню и считыванию выбора пользователя из командной строки.
