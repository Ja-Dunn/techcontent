可将标记应用于 Azure 资源，以便按类别在逻辑上对它们进行组织。每个标记由键和值组成。例如，可以将键“Environment”和值“Production”应用于生产中的所有资源。没有此标记，可能很难识别资源是用于开发、测试还是生产。不过，“Environment”和“Production”只是示例。可定义最适合组织你的订阅的键和值。

应用标记后，可以检索订阅中具有该标记键和值的所有资源。使用标记可以检索驻留在不同资源组中的相关资源。需要为计费或管理目的组织资源时，此方法可能很有用。

以下限制适用于标记：

* 每个资源或资源组最多可以有 15 个标记。
* 标记名称限制为 512 个字符。
* 标记值限制为 256 个字符。
* 应用于资源组的标记不会被该资源组中的资源继承。

<!---HONumber=Mooncake_0227_2017-->
<!--Update_Description:new article about introducing the resource manager tag -->