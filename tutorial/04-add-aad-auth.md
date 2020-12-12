---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661069"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете библиотеку проверки подлинности [Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.

1. Создайте новый каталог с именем **graphtutorial** в **каталоге ./src/main/resources.**

1. Создайте файл с именем **oAuth.properties** в **каталоге ./src/main/resources/graphtutorial** и добавьте в него следующий текст. Замените `YOUR_APP_ID_HERE` ид приложения, созданный на портале Azure.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    Значение содержит `app.scopes` области разрешений, необходимые приложению.

    - **User.Read** позволяет приложению получить доступ к профилю пользователя.
    - **MailboxSettings.Read** позволяет приложению получать доступ к настройкам из почтового ящика пользователя, включая настроенный часовой пояс пользователя.
    - **Calendars.ReadWrite** позволяет приложению перечислять календарь пользователя и добавлять в него новые события.

    > [!IMPORTANT]
    > Если вы используете управление исходным кодом, например git, сейчас хорошо бы исключить файл **oAuth.properties** из системы управления исходным кодом, чтобы не допустить случайной утечки вашего ИД приложения.

1. Откройте **App.java** и добавьте следующие `import` утверждения.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить **файл oAuth.properties.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Реализация входов

1. Создайте файл с именем **Authentication.java** в **каталоге ./graphtutorial/src/main/java/graphtutorial** и добавьте следующий код.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. В **App.java** добавьте следующий код перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить маркер доступа.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. Добавьте следующую строку после `// Display access token` комментария.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Запустите приложение. Приложение отображает URL-адрес и код устройства.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Откройте браузер и перейдите по отображаемого URL-адресу. Введите предоставленный код и войдите в нее. После завершения вернись к приложению и выберите **1. Отображение параметра маркера** доступа для отображения маркера доступа.

> [!TIP]
> Маркеры доступа для учетных записей майкрософт для работы или учебного заведения можно разбор для устранения неполадок в [https://jwt.ms](https://jwt.ms) . Маркеры доступа для личных учетных записей Майкрософт используют собственный формат и не могут быть разчетными.
