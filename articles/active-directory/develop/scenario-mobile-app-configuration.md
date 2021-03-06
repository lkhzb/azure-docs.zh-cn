---
title: Configure mobile apps that call web APIs - Microsoft identity platform | Azure
description: Learn how to build a mobile app that calls web APIs (app's code configuration)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 61d3a7b7f06ffdb7f39f80f7f3bf19e9007945d2
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702107"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Mobile app that calls web APIs - code configuration

Once you've created your application, you'll learn how to configure the code using the app registration parameters. Mobile applications also have some complex specifics, which have to do with fitting into the framework used to build these apps

## <a name="msal-libraries-supporting-mobile-apps"></a>MSAL libraries supporting mobile apps

The Microsoft libraries supporting mobile apps are:

  MSAL 库 | Description
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | To develop portable applications. MSAL.NET supported platforms to build a mobile application are UWP, Xamarin.iOS, and Xamarin.Android.
  ![MSAL.iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL.iOS | To develop native iOS applications with Objective-C or Swift
  ![MSAL.Android](media/sample-v2-code/logo_android.png) <br/> MSAL.Android | To develop native Android applications in Java for Android

## <a name="instantiating-the-application"></a>Instantiating the application

### <a name="android"></a>Android

Mobile applications use the `PublicClientApplication` class. Here is how to instantiate it:

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

Mobile applications on iOS need to instantiate the `MSALPublicClientApplication` class.

Objective-C：

```objc
NSError *msalError = nil;
     
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

Swift：
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

There are [additional MSALPublicClientApplicationConfig properties](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplicationConfig.html#/Configuration%20options) which can override the default authority, specify a redirect URI or change MSAL token caching behavior. 

### <a name="xamarin-or-uwp"></a>Xamarin or UWP

The following paragraph explains how to instantiate the application for Xamarin.iOS, Xamarin.Android, and UWP apps.

#### <a name="instantiating-the-application"></a>Instantiating the application

In Xamarin, or UWP, the simplest way to instantiate the application is as follows, where the `ClientId` is the Guid of your registered app.

```csharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

There are additional With*parameter* methods which set the UI parent, override the default authority, specify a client name and version (for telemetry), specify a redirect URI, Specify the Http factory to use (for instance to handle proxies, specify telemetry and logging) . This is the topic of the following paragraphs.

##### <a name="specifying-the-parent-uiwindowactivity"></a>Specifying the parent UI/Window/Activity

On Android, you need to pass the parent activity before you do the interactive authentication. On iOS, when using a broker, you need to pass-in the ViewController. In the same way on UWP, you might want to pass-in the parent window. This is possible when you acquire the token, but it's also possible to specify a callback at app creation time a delegate returning the UIParent.

```csharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

On Android, we recommend you use the `CurrentActivityPlugin` [here](https://github.com/jamesmontemagno/CurrentActivityPlugin).  Then your `PublicClientApplication` builder code would look like this:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>More app building parameters

- For the list of all modifiers available on `PublicClientApplicationBuilder`, see the reference documentation [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)
- For the description of all the options exposed in `PublicClientApplicationOptions` see [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions), in the reference documentation

## <a name="xamarin-ios-specific-considerations"></a>Xamarin iOS specific considerations

On Xamarin iOS, there are several considerations that you must take into account when using MSAL.NET:

1. [Override and implement the `OpenUrl` function in the `AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Enable Keychain groups](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Enable token cache sharing](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Enable Keychain access](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Details are provided in [Xamarin iOS considerations](msal-net-xamarin-ios-considerations.md)

## <a name="msal-for-ios-and-macos-specific-considerations"></a>MSAL for iOS and macOS specific considerations

Similar considerations apply when using MSAL for iOS and macOS:

1. [Implement the `openURL` callback](#brokered-authentication-for-msal-for-ios-and-macos)
2. [Enable keychain access groups](howto-v2-keychain-objc.md)
3. [Customize browsers and WebViews](customize-webviews.md)

## <a name="xamarin-android-specific-considerations"></a>Xamarin Android specific considerations

Here are Xamarin Android specifics:

- [Ensuring control goes back to MSAL once the interactive portion of the authentication flow ends](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Update the Android manifest](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Use the embedded web view (optional)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [故障排除](msal-net-xamarin-android-considerations.md#troubleshooting)

Details are provided in [Xamarin Android considerations](msal-net-xamarin-android-considerations.md)

Finally, there are some specificities to know about the browsers on Android. They are explained in [Xamarin Android-specific considerations with MSAL.NET](msal-net-system-browser-android-considerations.md)

#### <a name="uwp-specific-considerations"></a>UWP specific considerations

On UWP, you can use corporate networks. For additional information about using the MSAL library with UWP, see [Universal Windows Platform-specific considerations with MSAL.NET](msal-net-uwp-considerations.md).

## <a name="configuring-the-application-to-use-the-broker"></a>Configuring the application to use the broker

### <a name="why-use-brokers-in-ios-and-android-applications"></a>Why use brokers in iOS and Android applications?

On Android and iOS, brokers enable:

- Single Sign On (SSO) when device is registered with AAD. 用户无需登录到每个应用程序。
- 设备标识。 通过访问设备上已加入工作区的设备证书，启用与 Azure AD 设备相关的条件性访问策略。
- 应用程序标识验证。 当应用程序调用代理时，它会传递其重定向 url，并且 broker 将对其进行验证。

### <a name="enable-the-broker-on-xamarin"></a>启用 Xamarin 上的代理

若要启用其中一项功能，请在调用 `PublicClientApplicationBuilder.CreateApplication` 方法时使用 `WithBroker()` 参数。 默认情况下，`.WithBroker()` 设置为 true。 对于[Xamarin](#brokered-authentication-for-xamarinios)，请遵循以下步骤。

### <a name="enable-the-broker-for-msal-for-android"></a>为适用于 Android 的 MSAL 启用代理

有关在 Android 上启用代理的信息，请参阅[android 中的中转身份验证](brokered-auth.md)。 

### <a name="enable-the-broker-for-msal-for-ios-and-macos"></a>为 iOS 和 macOS 启用 MSAL 代理

默认情况下，将为 iOS 和 macOS 的 MSAL 中的 AAD 方案启用中转身份验证。 按照以下步骤配置应用程序，以便对[MSAL For iOS 和 macOS](#brokered-authentication-for-msal-for-ios-and-macos)的中转身份验证支持。 请注意， [MSAL For Xamarin](#brokered-authentication-for-xamarinios)和[MSAL For ios 和 macOS](#brokered-authentication-for-msal-for-ios-and-macos)的步骤不同。

### <a name="brokered-authentication-for-xamarinios"></a>针对 Xamarin 的中转身份验证

请按照以下步骤操作，使你的 Xamarin iOS 应用与[Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458)应用进行交流。

#### <a name="step-1-enable-broker-support"></a>步骤1：启用代理支持

代理支持基于每个`PublicClientApplication` 启用。 此项默认禁用。 通过 `PublicClientApplicationBuilder`创建 `PublicClientApplication` 时，必须使用 `WithBroker()` 参数（默认设置为 true）。

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>步骤2：更新 AppDelegate 以处理回调

当 MSAL.NET 调用代理时，代理将转而通过 `AppDelegate.OpenUrl` 方法返回到应用程序。 由于 MSAL 将等待来自代理的响应，因此应用程序需要合作才能调用 MSAL.NET。 为此，请更新 `AppDelegate.cs` 文件以重写以下方法。

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url,
                             string sourceApplication,
                             NSObject annotation)
{
 if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
 {
  AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
  return true;
 }
 else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
 {
  return false;
 }
 return true;
}
```

每次启动应用程序时都将调用此方法，并将其用作处理来自代理的响应并完成 MSAL.NET 启动的身份验证过程的机会。

#### <a name="step-3-set-a-uiviewcontroller"></a>步骤3：设置 UIViewController （）

使用 Xamarin iOS 时，通常不需要设置对象窗口，但在这种情况下，你可以从 broker 发送和接收响应。 仍在 `AppDelegate.cs`中设置 ViewController。

执行以下操作来设置对象窗口：

1) 在 `AppDelegate.cs`中，将 `App.RootViewController` 设置为新的 `UIViewController()`。 这将确保在调用代理的 `UIViewController`。 如果未正确设置，则可能会收到此错误： `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) 在 AcquireTokenInteractive 调用中，使用 `.WithParentActivityOrWindow(App.RootViewController)`，并将引用传递给你将使用的对象窗口。

例如：

在 `App.cs` 中：
```csharp
   public static object RootViewController { get; set; }
```
在 `AppDelegate.cs` 中：
```csharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
在获取令牌调用中：
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

#### <a name="step-4-register-a-url-scheme"></a>步骤4：注册 URL 方案

MSAL.NET 使用 Url 调用 broker，然后将 broker 响应返回到应用。 若要完成往返过程，需要在 `Info.plist` 文件中注册应用程序的 URL 方案。

为包含 `msauth`的 `CFBundleURLSchemes` 加前缀。 然后，将 `CFBundleURLName` 添加到末尾。

`$"msauth.(BundleId)"`

**例如：** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> 此 URL 方案将成为 RedirectUri 的一部分，用于在接收来自代理的响应时唯一标识应用。

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

#### <a name="step-5-lsapplicationqueriesschemes"></a>步骤5： LSApplicationQueriesSchemes

MSAL 使用 `–canOpenURL:` 来检查设备上是否安装了 broker。 在 iOS 9 中，Apple 锁定了应用程序可以查询的方案。

**将** **`msauthv2`** 添加到 `Info.plist` 文件的 `LSApplicationQueriesSchemes` 部分。

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-msal-for-ios-and-macos"></a>适用于 iOS 和 macOS 的 MSAL 的中转身份验证

默认情况下，为 AAD 方案启用中转身份验证。

#### <a name="step-1-update-appdelegate-to-handle-the-callback"></a>步骤1：更新 AppDelegate 以处理回调

当 iOS 和 macOS 的 MSAL 调用代理时，代理将转而通过 `openURL` 方法回调到你的应用程序。 由于 MSAL 将等待来自代理的响应，因此应用程序需要合作才能调用 MSAL。 为此，请更新 `AppDelegate.m` 文件以重写以下方法。

Objective-C：

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

Swift：

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

请注意，如果在 iOS 13 + 上采用了 UISceneDelegate，则 MSAL 回调需改为 UISceneDelegate 的 `scene:openURLContexts:` （请参阅[Apple 文档](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)）。 对于每个 URL，只能调用一次 MSAL `handleMSALResponse:sourceApplication:`。

#### <a name="step-2-register-a-url-scheme"></a>步骤2：注册 URL 方案

适用于 iOS 和 macOS 的 MSAL 使用 Url 调用 broker，然后将 broker 响应返回到应用。 若要完成往返过程，需要在 `Info.plist` 文件中注册应用程序的 URL 方案。

将自定义 URL 方案作为前缀 `msauth`。 然后，将**捆绑标识符**添加到末尾。

`msauth.(BundleId)`

**例如：** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> 此 URL 方案将成为 RedirectUri 的一部分，用于在接收来自代理的响应时唯一标识应用。 请确保在[Azure 门户](https://portal.azure.com)中为应用程序注册 `msauth.(BundleId)://auth` 格式的 RedirectUri。

```XML
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-lsapplicationqueriesschemes"></a>步骤3： LSApplicationQueriesSchemes

**添加 `LSApplicationQueriesSchemes`** 以允许调用 Microsoft Authenticator （如果已安装）。
请注意，在用 Xcode 11 和更高版本编译应用程序时，需要 "msauthv3" 方案。 

```XML 
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>msauthv2</string>
  <string>msauthv3</string>
</array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>适用于 Xamarin 的中转身份验证

MSAL.NET 尚不支持适用于 Android 的代理。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [获取令牌](scenario-mobile-acquire-token.md)
