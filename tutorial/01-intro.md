---
ms.openlocfilehash: f13ee9baa26bb487d5b50bd966e40cee9ba64378
ms.sourcegitcommit: 22ec2c0e4803b82c9d360f3609f8b15ba5d017ca
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "43940478"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом руководстве рассказывается, как создать консольное приложение Java, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.

> [!TIP]
> Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).

## <a name="prerequisites"></a>Необходимые компоненты

Прежде чем приступить к работе с этим руководством, на компьютере для разработки должен быть установлен [набор средств разработки Java SE (ЖДК)](https://java.com/en/download/faq/develop.xml) и [Gradle](https://gradle.org/) . Если у вас нет ЖДК или Gradle, посетите предыдущие ссылки для получения вариантов загрузки.

Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт. Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:

- Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.

> [!NOTE]
> Это руководство было написано с Опенждк версии 14.0.0.36 и Gradle 6,3. Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.

## <a name="feedback"></a>Обратная связь

Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-java).
