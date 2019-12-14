---
ms.openlocfilehash: 377db05d9b238d3f6f6e45032f0b85b9217df3db
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018848"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе выполняется интеграция [библиотеки проверки подлинности Microsoft (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) в приложение.

Создайте новый каталог **Resources** in **/графтуториал/СРК/Маин** Directory. Затем создайте новый каталог с именем **com** в каталоге **Resources** , а затем новый каталог с именем **contoso** в каталоге **com** . Наконец, создайте новый файл в каталоге **./графтуториал/СРК/Маин/ресаурцес/ком/Контосо** с именем **OAuth. Properties**и добавьте в этот файл приведенный ниже текст.

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

Замените `YOUR_APP_ID_HERE` идентификатором приложения, созданным на портале Azure.

> [!IMPORTANT]
> Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **OAuth. Properties** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

Откройте **Приложение App. Java** и добавьте приведенные ниже `import` операторы.

```java
import java.io.IOException;
import java.util.Properties;
```

Затем добавьте приведенный ниже код перед `Scanner input = new Scanner(System.in);` строкой, чтобы загрузить файл **OAuth. Properties** .

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a>Реализация входа

Создайте новый файл в каталоге **./графтуториал/СРК/Маин/Жава/ком/Контосо** с именем **authentication. Java** и добавьте следующий код.

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/common/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

В файле **app. Java**добавьте следующий код непосредственно перед `Scanner input = new Scanner(System.in);` строкой, чтобы получить маркер доступа.

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

Затем добавьте указанную ниже строку после `// Display access token` комментария.

```java
System.out.println("Access token: " + accessToken);
```

Постройте и запустите приложение. Приложение отображает URL-адрес и код устройства.

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

Откройте браузер и перейдите к отображаемому URL-адресу. Введите предоставленный код и войдите в систему. После завершения вернитесь к приложению и выберите **1. Показать маркер доступа** для отображения маркера доступа.
