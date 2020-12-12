---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661083"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b826a-101">В этом разделе вы создадим базовое консольное приложение Java.</span><span class="sxs-lookup"><span data-stu-id="b826a-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="b826a-102">Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="b826a-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="b826a-103">Чтобы создать новый проект Gradle, запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="b826a-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="b826a-104">После создания проекта убедитесь, что он работает, выполив следующую команду для запуска приложения в CLI.</span><span class="sxs-lookup"><span data-stu-id="b826a-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="b826a-105">Если он работает, приложение должно вы выходные `Hello World.` данные .</span><span class="sxs-lookup"><span data-stu-id="b826a-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="b826a-106">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="b826a-106">Install dependencies</span></span>

<span data-ttu-id="b826a-107">Прежде чем двигаться дальше, добавьте некоторые дополнительные зависимости, которые вы будете использовать позже.</span><span class="sxs-lookup"><span data-stu-id="b826a-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="b826a-108">[Microsoft Authentication Library (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) для проверки подлинности пользователя и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="b826a-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="b826a-109">[Microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b826a-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="b826a-110">[Привязка SLF4J NOP для](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) подавления ведения журнала из MSAL.</span><span class="sxs-lookup"><span data-stu-id="b826a-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="b826a-111">Откройте **./build.gradle.**</span><span class="sxs-lookup"><span data-stu-id="b826a-111">Open **./build.gradle**.</span></span> <span data-ttu-id="b826a-112">`dependencies`Обновим раздел, чтобы добавить эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="b826a-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="b826a-113">Добавьте следующее в конец **./build.gradle.**</span><span class="sxs-lookup"><span data-stu-id="b826a-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="b826a-114">При следующей сборке проекта Gradle загрузит эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="b826a-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="b826a-115">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="b826a-115">Design the app</span></span>

1. <span data-ttu-id="b826a-116">Откройте файл **./src/main/java/graphtutorial/App.java** и замените его содержимое следующим:</span><span class="sxs-lookup"><span data-stu-id="b826a-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="b826a-117">При этом реализуется базовое меню и считыется выбор пользователя из командной строки.</span><span class="sxs-lookup"><span data-stu-id="b826a-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
