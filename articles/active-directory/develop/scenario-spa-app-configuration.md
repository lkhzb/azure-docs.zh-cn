---
title: 配置单页应用-Microsoft 标识平台 |Microsoft
description: 了解如何生成单页面应用程序（应用程序的代码配置）
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f72b2b85fcaae4e0a21e4bbe2f3af79359ca2fa6
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701903"
---
# <a name="single-page-application-code-configuration"></a>单页应用程序：代码配置

了解如何为单页应用程序（SPA）配置代码。

## <a name="msal-libraries-that-support-implicit-flow"></a>支持隐式流的 MSAL 库

Microsoft 标识平台提供以下 Microsoft 身份验证库（MSAL）库，以支持使用行业推荐的安全实践的隐式流：  

| MSAL 库 | Description |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | 无格式的 JavaScript 库，可用于通过 JavaScript 或 SPA 框架（如角度、Vue 和响应）生成的任何客户端 web 应用。 |
| ![MSAL 角](media/sample-v2-code/logo_angular.png) <br/> [MSAL 角](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | 用于简化在通过角度框架构建的单页面应用程序中使用的核心 MSAL 库包装。 此库处于预览阶段，其中包含某些角度版本和浏览器的[已知问题](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular)。 |

## <a name="application-code-configuration"></a>应用程序代码配置

在 MSAL 库中，应用程序注册信息在库初始化过程中作为配置传递。

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
有关可配置选项的详细信息，请参阅[用 MSAL 初始化应用程序](msal-js-initializing-client-applications.md)。

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [登录和注销](scenario-spa-sign-in.md)
