<properties
	pageTitle="适用于 Xamarin.Android 应用的 Azure 移动应用入门"
	description="按照本教程进行操作，开始使用 Azure 移动应用进行 Xamarin Android 开发"
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="ggailey777"
	manager="erikre"
	editor="" />

<tags
	ms.service="app-service-mobile"
	ms.date="08/11/2016"
	wacn.date="09/26/2016"/>

#创建 Xamarin.Android 应用

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##概述

本教程说明如何向 Xamarin.Android 应用添加基于云的后端服务。有关详细信息，请参阅[什么是移动应用](/documentation/articles/app-service-mobile-value-prop/)。

以下是完成的应用程序的屏幕快照：

![][0]

只有在完成本教程后，才可以学习有关 Xamarin Android 应用的所有其他移动应用教程。

##先决条件

若要完成本教程，您需要以下各项：

* 有效的 Azure 帐户。如果没有帐户，可以注册 Azure 试用版并获取多达 10 个免费的移动应用，即使在试用期结束之后仍可继续使用这些应用。有关详细信息，请参阅 [Azure 试用](/pricing/1rmb-trial/)。

* Visual Studio with Xamarin。有关说明，请参阅[设置和安装 Visual Studio 和 Xamarin](https://msdn.microsoft.com/zh-cn/library/mt613162.aspx)。

>[AZURE.NOTE] 如果要在注册 Azure 帐户之前就开始使用 Azure 应用服务，请转到[试用应用服务](https://tryappservice.azure.com/?appServiceName=mobile)，即可在应用服务中立即创建一个生存期较短的入门级移动应用。你不需要使用信用卡，也不需要做出承诺。

## 创建新的 Azure 移动应用后端

按照下列步骤创建新的移动应用后端。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

现已预配可供移动客户端应用程序使用的 Azure 移动应用后端。接下来，将为简单的“待办事项列表”后端下载服务器项目并将其发布到 Azure。

## 配置服务器项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## 下载并运行 Xamarin.Android 应用

1. 在“下载并运行 Xamarin.Android 项目”下，单击“下载”按钮。

  	此时会下载包含已连接到移动应用的客户端应用程序的项目。将压缩的项目文件保存到本地计算机，并记下保存位置。

2. 按 **F5** 键生成项目并启动该应用。

3. 在应用中键入有意义的文本（例如 _Complete the tutorial_），然后单击“添加”图标。

	![][10]

	这样可向在 Azure 中托管的新移动应用后端发送 POST 请求。来自请求的数据被插入到 TodoItem 表。移动应用后端返回存储在表中的项，数据显示在列表中。

	> [AZURE.NOTE] 可以查看访问移动应用后端以查询和插入数据的代码，这些代码在 ToDoActivity.cs C# 文件中。

##后续步骤

* [向应用添加身份验证](/documentation/articles/app-service-mobile-xamarin-android-get-started-users/) 了解如何使用标识提供者对应用的用户进行身份验证。
* [向 Xamarin.Android 应用添加推送通知](/documentation/articles/app-service-mobile-xamarin-android-get-started-push/) 了解如何向应用添加推送通知。
* [如何使用 Azure 移动应用的托管客户端](/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) 了解如何在 Xamarin 应用中使用托管客户端 SDK。


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://portal.azure.cn/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203

<!---HONumber=Mooncake_0919_2016-->