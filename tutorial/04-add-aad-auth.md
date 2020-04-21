---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612063"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе выполняется интеграция [библиотеки проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.

1. Создайте новый каталог с именем **графтуториал** в каталоге **./СРК/Маин/ресаурцес** .

1. Создайте новый файл в каталоге **./СРК/Маин/ресаурцес/графтуториал** с именем **OAuth. Properties**и добавьте в этот файл приведенный ниже текст.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    Замените `YOUR_APP_ID_HERE` идентификатором приложения, созданным на портале Azure.

    > [!IMPORTANT]
    > Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **OAuth. Properties** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

1. Откройте **Приложение App. Java** и добавьте приведенные ниже `import` операторы.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Добавьте приведенный ниже код непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить файл **OAuth. Properties** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Реализация входа

1. Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/графтуториал** с именем **authentication. Java** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. В файле **app. Java**добавьте следующий код непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить маркер доступа.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. Добавьте указанную ниже строку после `// Display access token` комментария.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Запустите приложение. Приложение отображает URL-адрес и код устройства.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Откройте браузер и перейдите к отображаемому URL-адресу. Введите предоставленный код и войдите в систему. После завершения вернитесь к приложению и выберите **1. Показать маркер доступа** для отображения маркера доступа.
