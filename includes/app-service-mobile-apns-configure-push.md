

1.  在 Mac 上启动“Keychain Access”。依次打开“类别”>“我的证书”。找到要导出的 SSL 证书（之前已下载），并公开其内容。仅选择证书而不选择私钥，并[将其导出](https://support.apple.com/kb/PH20122?locale=en_US)。

2. 在 Azure 门户中，依次单击“浏览全部”>“应用程序服务”>你的移动应用后端 >“设置”>“移动”>“推送”>“配置所需设置”>“+ 通知中心”，提供通知中心的名称和命名空间，单击“确定”按钮。

  	![][1]

3. 在“创建通知中心”边栏选项卡中，单击“创建”按钮。
     
    在继续执行下一步之前，请单击“通知”，确保通知中心安装已完成。

4. 在 Azure 门户中，依次单击“浏览全部”>“应用程序服务”> 移动应用程序后端 >“设置”>“移动”>“推送”>“Apple Push Notification 服务”>“上传证书”。上传 .p12 文件，选择正确的“模式”（具体取决于之前生成的客户端 SSL 证书是“开发”还是“分发”。） 现在，服务已配置为在 iOS 上使用通知中心！

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png

<!---HONumber=Mooncake_0919_2016-->