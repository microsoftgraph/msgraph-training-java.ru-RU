---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695802"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="62c49-101">В этом разделе вы создадим базовое приложение для консоли Java.</span><span class="sxs-lookup"><span data-stu-id="62c49-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="62c49-102">Откройте интерфейс командной строки (CLI) в каталоге, в котором необходимо создать проект.</span><span class="sxs-lookup"><span data-stu-id="62c49-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="62c49-103">Запустите следующую команду, чтобы создать новый проект Gradle.</span><span class="sxs-lookup"><span data-stu-id="62c49-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="62c49-104">После создания проекта убедитесь, что он работает, выполив следующую команду для запуска приложения в CLI.</span><span class="sxs-lookup"><span data-stu-id="62c49-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="62c49-105">Если оно работает, приложение должно выводить `Hello World.` .</span><span class="sxs-lookup"><span data-stu-id="62c49-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="62c49-106">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="62c49-106">Install dependencies</span></span>

<span data-ttu-id="62c49-107">Прежде чем двигаться дальше, добавьте дополнительные зависимости, которые вы будете использовать позже.</span><span class="sxs-lookup"><span data-stu-id="62c49-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="62c49-108">[Клиентская библиотека Azure Identity для Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) для проверки подлинности пользователя и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="62c49-108">[Azure Identity client library for Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="62c49-109">[Microsoft Graph SDK для Java](https://github.com/microsoftgraph/msgraph-sdk-java) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="62c49-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="62c49-110">Откройте **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="62c49-110">Open **./build.gradle**.</span></span> <span data-ttu-id="62c49-111">`dependencies`Обнови раздел, чтобы добавить эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="62c49-111">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. <span data-ttu-id="62c49-112">Добавьте следующее в конец **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="62c49-112">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="62c49-113">При следующем создании проекта Gradle загрузит эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="62c49-113">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="62c49-114">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="62c49-114">Design the app</span></span>

1. <span data-ttu-id="62c49-115">Откройте **файл ./src/main/java/graphtutorial/App.java** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="62c49-115">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="62c49-116">Это реализует основное меню и считыванию выбора пользователя из командной строки.</span><span class="sxs-lookup"><span data-stu-id="62c49-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
