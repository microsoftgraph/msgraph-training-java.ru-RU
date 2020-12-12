---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661076"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом руководстве рассказывается, как создать консольное приложение Java, использующее API Microsoft Graph для получения сведений календаря для пользователя.

> [!TIP]
> Если вы предпочитаете просто скачать завершенный учебник, вы можете скачать или клонировать [репозиторий GitHub.](https://github.com/microsoftgraph/msgraph-training-java)

## <a name="prerequisites"></a>Предварительные условия

Прежде чем приступить к этому учебнику, на компьютере разработчика должны быть установлены пакет java [SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) и [Gradle.](https://gradle.org/) Если у вас нет JDK или Gradle, посетите предыдущие ссылки для скачивания.

У вас также должна быть личная учетная запись Майкрософт с почтовым Outlook.com или учетная запись Майкрософт для работы или учебного заведения. Если у вас нет учетной записи Майкрософт, существует несколько вариантов получения бесплатной учетной записи:

- Вы можете [зарегистрироваться для новой личной учетной записи Майкрософт.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Вы можете зарегистрироваться в программе для разработчиков [Office 365,](https://developer.microsoft.com/office/dev-program) чтобы получить бесплатную подписку на Office 365.

> [!NOTE]
> Это руководство было написано с помощью OpenJDK версии 14.0.0.36 и Gradle 6.7.1. Действия в этом руководстве могут работать с другими версиями, но не были протестированы.

## <a name="feedback"></a>Отзывы

Поделитесь с этим учебником отзывами в [репозитории GitHub.](https://github.com/microsoftgraph/msgraph-training-java)
