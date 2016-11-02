<properties
	pageTitle="使用移动应用在 Android 中添加身份验证 | Azure 应用服务"
	description="了解如何使用 Azure 应用服务中的移动应用，通过各种标识提供者对 Android 应用的用户进行身份验证。"
	services="app-service\mobile"
	documentationCenter="android"
	authors="RickSaling"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.date="07/18/2016"
	wacn.date="09/26/2016"/>

# 将身份验证添加到 Android 应用

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## 摘要

本教程介绍如何使用支持的标识提供者将身份验证添加到 Android 上的待办事项列表快速入门项目。本教程基于 [Get started with Mobile Apps]（移动应用入门）教程，必须先完成该教程。

##<a name="register"></a>注册应用以进行身份验证并配置应用服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>将权限限制给已经过身份验证的用户

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ 在 Android Studio 中，打开完成 [Get started with Mobile Apps]（移动应用入门）教程时创建的项目，在“运行”菜单中单击“运行应用”，然后验证在应用程序启动后是否引发状态代码为 401（“未授权”）的未经处理的异常。

	 发生此异常的原因是应用尝试以未经身份验证的用户身份访问后端，但 _TodoItem_ 表现在要求身份验证。

接下来，需要更新应用程序，以便在从移动应用后端请求资源之前对用户进行身份验证。

## 向应用程序添加身份验证

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>在客户端上缓存身份验证令牌

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##后续步骤

完成此基本身份验证教程后，请考虑继续学习以下教程之一：

+ [Add push notifications to your Android app](/documentation/articles/app-service-mobile-android-get-started-push/)（将推送通知添加到 Android 应用）：
  了解如何为应用添加推送通知支持，以及如何将移动应用后端配置为使用 Azure 通知中心发送推送通知。

+ [Enable offline sync for your Android app](/documentation/articles/app-service-mobile-android-get-started-offline-data/)（为 Android 应用启用脱机同步）：
  了解如何使用移动应用后端向应用添加脱机支持。脱机同步允许最终用户与移动应用交互（查看、添加或修改数据），即使在没有网络连接时也是如此。



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]: #next-steps


<!-- URLs. -->
[Get started with Mobile Apps]: /documentation/articles/app-service-mobile-android-get-started/

<!---HONumber=Mooncake_0919_2016-->