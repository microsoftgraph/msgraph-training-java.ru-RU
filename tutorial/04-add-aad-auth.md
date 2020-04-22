---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612063"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="eb227-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb227-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="eb227-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="eb227-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="eb227-103">На этом этапе выполняется интеграция [библиотеки проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.</span><span class="sxs-lookup"><span data-stu-id="eb227-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="eb227-104">Создайте новый каталог с именем **графтуториал** в каталоге **./СРК/Маин/ресаурцес** .</span><span class="sxs-lookup"><span data-stu-id="eb227-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="eb227-105">Создайте новый файл в каталоге **./СРК/Маин/ресаурцес/графтуториал** с именем **OAuth. Properties**и добавьте в этот файл приведенный ниже текст.</span><span class="sxs-lookup"><span data-stu-id="eb227-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="eb227-106">Замените `YOUR_APP_ID_HERE` идентификатором приложения, созданным на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="eb227-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eb227-107">Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **OAuth. Properties** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="eb227-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="eb227-108">Откройте **Приложение App. Java** и добавьте приведенные ниже `import` операторы.</span><span class="sxs-lookup"><span data-stu-id="eb227-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="eb227-109">Добавьте приведенный ниже код непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить файл **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="eb227-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="eb227-110">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="eb227-110">Implement sign-in</span></span>

1. <span data-ttu-id="eb227-111">Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **authentication. Java** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="eb227-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="eb227-112">В файле **app. Java**добавьте следующий код непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="eb227-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="eb227-113">Добавьте указанную ниже строку после `// Display access token` комментария.</span><span class="sxs-lookup"><span data-stu-id="eb227-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="eb227-114">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="eb227-114">Run the app.</span></span> <span data-ttu-id="eb227-115">Приложение отображает URL-адрес и код устройства.</span><span class="sxs-lookup"><span data-stu-id="eb227-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="eb227-116">Откройте браузер и перейдите к отображаемому URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="eb227-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="eb227-117">Введите предоставленный код и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="eb227-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="eb227-118">После завершения вернитесь к приложению и выберите **1. Показать маркер доступа** для отображения маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="eb227-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
