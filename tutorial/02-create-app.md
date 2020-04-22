---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612021"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="aa157-101">В этом разделе показано, как создать базовое консольное приложение Java.</span><span class="sxs-lookup"><span data-stu-id="aa157-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="aa157-102">Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="aa157-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="aa157-103">Выполните следующую команду, чтобы создать новый проект Gradle.</span><span class="sxs-lookup"><span data-stu-id="aa157-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="aa157-104">После создания проекта убедитесь, что он работает, выполнив приведенную ниже команду, чтобы запустить приложение в утилите CLI.</span><span class="sxs-lookup"><span data-stu-id="aa157-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="aa157-105">Если это сработает, приложение должно выводить `Hello World.`результаты.</span><span class="sxs-lookup"><span data-stu-id="aa157-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="aa157-106">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="aa157-106">Install dependencies</span></span>

<span data-ttu-id="aa157-107">Перед перемещением добавьте дополнительные зависимости, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="aa157-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="aa157-108">[Библиотека проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) для проверки подлинности пользователя и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="aa157-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="aa157-109">[Пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="aa157-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="aa157-110">[Привязка НОП SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) для отмены ведения журнала из MSAL.</span><span class="sxs-lookup"><span data-stu-id="aa157-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="aa157-111">Открыть **./буилд.градле**.</span><span class="sxs-lookup"><span data-stu-id="aa157-111">Open **./build.gradle**.</span></span> <span data-ttu-id="aa157-112">Обновите `dependencies` раздел, чтобы добавить эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="aa157-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="aa157-113">Добавьте следующий элемент в конец файла **./буилд.градле**.</span><span class="sxs-lookup"><span data-stu-id="aa157-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="aa157-114">В следующий раз при построении проекта Gradle загрузит эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="aa157-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="aa157-115">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="aa157-115">Design the app</span></span>

1. <span data-ttu-id="aa157-116">Откройте файл **./СРК/Маин/Жава/графтуториал/АПП.Жава** и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="aa157-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="aa157-117">В результате будет реализовано базовое меню, в котором считывается выбор пользователя из командной строки.</span><span class="sxs-lookup"><span data-stu-id="aa157-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
