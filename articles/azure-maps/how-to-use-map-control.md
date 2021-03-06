---
title: Web 地图控件入门 |Microsoft Azure 映射
description: 了解如何使用 Microsoft Azure map map control 客户端 Javascript 库，将地图和嵌入 Azure Maps 功能呈现到 web 或移动应用程序中。
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 90cd752ddd46999627de538abe3ca6b1926005aa
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264113"
---
# <a name="use-the-azure-maps-map-control"></a>使用 Azure Maps map 控件

可通过 Map Control 客户端 Javascript 库呈现地图，并将 Azure Maps 功能嵌入到 Web 或移动应用中。

## <a name="create-a-new-map-in-a-web-page"></a>在网页中创建新地图

通过使用 Map Control 客户端 Javascript 库，可以在网页中嵌入地图。

1. 创建新的 HTML 文件。

2. 载入 Azure Maps Web SDK。 您可以选择以下两个选项之一：

a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 通过在该文件的 `<head>` 元素中添加样式表和脚本引用的 URL 终结点，使用 Azure Maps Web SDK 的全局承载的 CDN 版本：

```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
```

b.保留“数据库类型”设置，即设置为“共享”。 使用[Azure Maps control](https://www.npmjs.com/package/azure-maps-control) NPM 包在本地加载 AZURE MAPS Web SDK 源代码，并将其与应用程序一起托管。 此程序包还包括了 TypeScript 定义。

> **npm 安装 azure-maps**

然后，在该文件的 `<head>` 元素中添加对 Azure Maps 样式表和脚本源的引用：

```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
    <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
```

    >[!Note]
    > Typescript definitions can be imported into your application by adding the following code:
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

3. 若要以填满整个页面正文的方式呈现地图，请向 `<head>` 元素中添加以下 `<style>` 元素。

```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
```

4. 在页面的正文中，添加一个 `<div>` 元素并将其 `id` 指定为 **myMap**。

```HTML
    <body>
        <div id="myMap"></div>
    </body>
```

5. 若要初始化地图控件，请在 html 正文中定义一个新的脚本标记。 在创建 `Map` 类的实例时，将映射的 `id` 传入 `<div>` 或 `HTMLElement` （例如 `document.getElementById('myMap')`）作为第一个参数。 通过[身份验证选项](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions)使用你自己的 Azure Maps 帐户密钥或 Azure Active Directory (AAD) 凭据对地图进行身份验证。 

如果需要创建帐户或查找密钥，请按照[创建帐户](quick-demo-map-app.md#create-an-account-with-azure-maps)和[获取](quick-demo-map-app.md#get-the-primary-key-for-your-account)主密钥中的说明进行操作。 

**language** 选项指定用于地图标签和控件的语言。 有关支持的语言的详细信息，请参阅[支持的语言](supported-languages.md)。如果使用订阅密钥进行身份验证，请使用以下内容：

```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
 ```

如果使用 Azure Active Directory （AAD）进行身份验证，请使用以下内容：

```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
```

此列表显示了如何将 Azure Active Directory （AAD）与 Azure Maps 集成的示例可[在此处](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)找到。 
    
有关详细信息，请参阅[Azure Maps 的身份验证](azure-maps-authentication.md)文档和[Azure Maps Azure AD 身份验证示例](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)。

6. 此外，你可能会发现将下列 meta 标记元素添加到页面的开头非常有用：

```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

7. 将所有 HTML 文件放在一起应类似于以下代码：

```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
 ```

8. 在 Web 浏览器中打开该文件并查看呈现的地图。 它应该如下图所示：

![显示呈现结果的地图图像](./media/how-to-use-map-control/map-of-seattle.png)

## <a name="localizing-the-map"></a>本地化地图

Azure Maps 提供了两种不同的方法来设置呈现地图的语言和区域视图。 第一种方法是将此信息添加到全局 `atlas` 命名空间，这将导致应用程序中的所有地图控件实例默认设置为这些设置。 下面将语言设置为法语（"fr"），并将区域视图设置为 "Auto"：

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('Auto');
```

第二种方法是在加载映射时将此信息传递到映射选项，如下所示：

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'Auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> 利用 Web SDK，可以在具有不同语言和区域设置的同一页上加载多个映射实例。 此外，在使用映射的 `setStyle` 函数加载映射后，可以更新这些设置。 

下面是将语言设置为 "fr" 并将区域视图设置为 "自动" 的 Azure Maps 的示例。

![以法语显示标签的地图图像](./media/how-to-use-map-control/websdk-localization.png)

[此处](supported-languages.md)介绍了支持的语言和区域视图的完整列表。

## <a name="azure-government-cloud-support"></a>Azure 政府版云支持

Azure Maps Web SDK 支持 Azure 政府云。 用于访问 Azure Maps Web SDK 的所有 JavaScript 和 CSS Url 保持不变。 需要完成以下任务才能连接到 Azure Maps 平台的 Azure 政府云版本。

使用交互式地图控件时，请在创建 `Map` 类的实例之前添加以下代码行。 

```javascript
atlas.setDomain('atlas.azure.us');
```

验证地图和服务时，请确保使用 Azure 政府版云平台中 Azure Maps 的身份验证详细信息。

使用 "服务" 模块时，需要在创建 API URL 端点的实例时设置服务的域。 例如，下面的代码创建 `SearchURL` 类的实例，并将该域指向 Azure 政府云。

```javascript
var searchURL = new atlas.service.SearchURL(pipeline, 'atlas.azure.us');
```

如果直接访问 Azure Maps REST 服务，请将 URL 域更改为 `atlas.azure.us`。 例如，如果使用搜索 API 服务，请将 URL 域从 `https://atlas.microsoft.com/search/` 更改为 `https://atlas.azure.us/search/`。

## <a name="next-steps"></a>后续步骤

了解如何创建地图并与之进行交互：

> [!div class="nextstepaction"]
> [创建地图](map-create.md)

了解如何设置地图样式：

> [!div class="nextstepaction"]
> [选择地图样式](choose-map-style.md)

将更多数据添加到地图：

> [!div class="nextstepaction"]
> [创建地图](map-create.md)

> [!div class="nextstepaction"]
> [示例代码](https://docs.microsoft.com/samples/browse/?products=azure-maps)

有关演示如何将 Azure Active Directory （AAD）与 Azure Maps 集成的示例列表，请参阅：

> [!div class="nextstepaction"]
> [Azure AD 身份验证示例](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)