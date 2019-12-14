---
ms.openlocfilehash: 298c78d0626e16899de402b3e9b537c8b8386886
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018843"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5c6ed-101">Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="5c6ed-102">Выполните следующую команду, чтобы создать новый проект Мавен.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="5c6ed-103">Можно ввести разные значения для идентификатора группы (`DgroupId` параметра) и идентификатора артефакта (`DartifactId` параметра), чем указано выше.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="5c6ed-104">В примере кода в этом руководстве предполагается, что используется `com.contoso` идентификатор группы.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="5c6ed-105">Если вы используете другое значение, не забудьте заменить `com.contoso` в любом образце кода идентификатором вашей группы.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="5c6ed-106">При появлении соответствующего запроса подтвердите конфигурацию и дождитесь создания проекта.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="5c6ed-107">После создания проекта убедитесь, что он работает, выполнив следующие команды для упаковки и запуска приложения в утилите CLI.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="5c6ed-108">Если это сработает, приложение должно выводить `Hello World!`результаты.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="5c6ed-109">Перед перемещением добавьте дополнительные зависимости, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="5c6ed-110">[Библиотека проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) для проверки подлинности пользователя и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="5c6ed-111">[Пакет SDK Microsoft Graph для Java](https://github.com/microsoftgraph/msgraph-sdk-java) , чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="5c6ed-112">[Привязка НОП SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) для отмены ведения журнала из MSAL.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="5c6ed-113">Открыть **./графтуториал/пом.ксмл**.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="5c6ed-114">Добавьте следующий `<dependencies>` элемент в элемент.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-114">Add the following inside the `<dependencies>` element.</span></span>

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

<span data-ttu-id="5c6ed-115">В следующий раз при построении проекта Мавен загрузит эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="5c6ed-116">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="5c6ed-116">Design the app</span></span>

<span data-ttu-id="5c6ed-117">Откройте файл **./графтуториал/СРК/Маин/Жава/ком/Контосо/АПП.Жава** и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="5c6ed-118">В результате будет реализовано базовое меню, в котором считывается выбор пользователя из командной строки.</span><span class="sxs-lookup"><span data-stu-id="5c6ed-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
