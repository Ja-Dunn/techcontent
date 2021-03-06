<properties
    pageTitle="导入和导出 Azure IoT 中心设备标识 | Azure"
    description="如何使用 Azure IoT 服务 SDK 针对标识注册表执行批量操作，以导入和导出设备标识。借助导入操作，可批量创建、更新和删除设备标识。"
    services="iot-hub"
    documentationcenter=".net"
    author="dominicbetts"
    manager="timlt"
    editor="" />
<tags
    ms.assetid="2ade1494-45ea-46a7-ade7-cf6e11ce62da"
    ms.service="iot-hub"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="01/04/2017"
    wacn.date="02/10/2017"
    ms.author="dobett" />  


# 批量管理 IoT 中心设备标识
每个 IoT 中心都有一个标识注册表，可用于在服务中创建每个设备的资源（例如包含即时云到设备消息的队列）。标识注册表还可用于控制对面向设备的终结点的访问。本文介绍如何从标识注册表批量导入和导出设备标识。

导入和导出操作在*作业*的上下文中进行，可让用户对 IoT 中心执行批量服务操作。

**RegistryManager** 类包含使用**作业**框架的 **ExportDevicesAsync** 和 **ImportDevicesAsync** 方法。这些方法可以导出、导入和同步整个 IoT 中心标识注册表。

## 什么是作业？
标识注册表操作使用**作业**系统的前提是操作符合以下条件：

* 相较标准运行时操作，其执行时间可能很长，或
* 向用户返回大量数据。

在这些情况下，操作不会让单个 API 调用在返回操作结果后等待或阻塞，而是以异步方式创建该 IoT 中心的**作业**，然后立即返回 **JobProperties** 对象。

以下 C# 代码片段演示如何创建导出作业：


		// Call an export job on the IoT Hub to retrieve all devices
		JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);


然后可以使用 **RegistryManager** 类来查询使用所返回 **JobProperties** 元数据的**作业**的状态。

以下 C# 代码片段演示如何每隔五秒轮询一次以查看作业是否已完成执行：


		// Wait until job is finished
		while(true)
		{
		  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
		  if (exportJob.Status == JobStatus.Completed || 
		      exportJob.Status == JobStatus.Failed ||
		      exportJob.Status == JobStatus.Cancelled)
		  {
		    // Job has finished executing
		    break;
		  }

		  await Task.Delay(TimeSpan.FromSeconds(5));
		}


## 导出设备

使用 **ExportDevicesAsync** 方法将整个 IoT 中心标识注册表导出到使用[共享访问签名](https://msdn.microsoft.com/zh-cn/library/ee395415.aspx)的 [Azure 存储](/documentation/services/storage/) Blob 容器。

此方法可让你在所控制的 Blob 容器中创建可靠的设备信息备份。

**ExportDevicesAsync** 方法需要两个参数：

*  包含 Blob 容器 URI 的*字符串*。此 URI 必须包含可授予容器写入权限的 SAS 令牌。作业在此容器中创建用于存储序列化导出设备数据的块 Blob。SAS 令牌必须包含这些权限：
    
    
	    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    

*  指示你是否要在导出数据中排除身份验证密钥的*布尔值*。如果为 **false**，则身份验证密钥将包含在导出输出中；否则像为 **null** 时一样导出密钥。

下面的 C# 代码片段演示了如何启动在导出数据中包含设备身份验证密钥的导出作业，然后对完成情况进行轮询：


		// Call an export job on the IoT Hub to retrieve all devices
		JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

		// Wait until job is finished
		while(true)
		{
		    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
		    if (exportJob.Status == JobStatus.Completed || 
		        exportJob.Status == JobStatus.Failed ||
		        exportJob.Status == JobStatus.Cancelled)
		    {
		    // Job has finished executing
		    break;
		    }

		    await Task.Delay(TimeSpan.FromSeconds(5));
		}


作业在提供的 Blob 容器中将其输出存储为名为 **devices.txt** 的块 Blob。输出数据包含 JSON 序列化设备数据，每行代表一个设备。

以下示例显示输出数据：


		{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
		{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
		{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
		{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
		{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}


如果你需要访问代码中的此数据，可以使用 **ExportImportDevice** 类轻松将此数据反序列化。以下 C# 代码片段演示如何读取前面导出到块 Blob 的设备信息：


		var exportedDevices = new List<ExportImportDevice>();

		using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
		{
		  while (streamReader.Peek() != -1)
		  {
		    string line = await streamReader.ReadLineAsync();
		    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
		    exportedDevices.Add(device);
		  }
		}


> [AZURE.NOTE]  也可以使用 **RegistryManager** 类的 **GetDevicesAsync** 方法检索设备列表。但是，此方法有一个硬性限制，那就是返回的设备对象数最多只能有 1000 个。**GetDevicesAsync** 方法的预期用例适用于开发方案，其目的是要帮助调试，因此不建议用于生产工作负荷。

## 导入设备

借助 **RegistryManager** 类中的 **ImportDevicesAsync** 方法，可在 IoT 中心标识注册表中执行批量导入和同步操作。与 **ExportDevicesAsync** 方法一样，**ImportDevicesAsync** 方法也使用**作业**框架。

请谨慎使用 **ImportDevicesAsync** 方法，因为除了在标识注册表中预配新设备外，此方法还会更新和删除现有设备。

> [AZURE.WARNING]
导入操作不可撤消。请始终先使用 **ExportDevicesAsync** 方法将现有数据备份到其他 Blob 容器，然后再对标识注册表进行批量更改。
> 
> 

**ImportDevicesAsync** 方法有两个参数：

*  一个*字符串*，其中包含作为作业的*输入*的 [Azure 存储](/documentation/services/storage/) Blob 容器的 URI。此 URI 必须包含可授予容器读取权限的 SAS 令牌。此容器必须包含名为 **devices.txt** 的 Blob，而此 Blob 中包含要导入到设备标识注册表的序列化设备数据。导入数据必须包含使用 **ExportImportDevice** 作业所创建的相同 JSON 格式的设备信息。SAS 令牌必须包含这些权限：

    
	    SharedAccessBlobPermissions.Read
    

*  一个*字符串*，其中包含作为作业的*输出*的 [Azure 存储](/documentation/services/storage/) Blob 容器的 URI。作业在此容器中创建块 Blob，用于存储已完成的导入**作业**中的任何错误信息。SAS 令牌必须包含这些权限：
    
    
	    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    

> [AZURE.NOTE]  这两个参数可以指向同一 Blob 容器。参数不同只会让你更容易掌控数据，因为输出容器需要其他权限。

以下 C# 代码段演示如何启动导入作业：


		JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);


## 导入行为

可以使用 **ImportDevicesAsync** 方法，在设备标识注册表中执行以下批量操作：

-   批量注册新设备
-   批量删除现有设备
-   批量更改状态（启用或禁用设备）
-   批量分配新设备身份验证密钥
-   批量自动重新生成设备身份验证密钥

可以在单个 **ImportDevicesAsync** 调用中执行上述操作的任意组合。例如，可以同时注册新设备并删除或更新现有设备。配合 **ExportDevicesAsync** 方法一起使用时，可以将某个 IoT 中心内的所有设备迁移到另一个 IoT 中心。

可以在每个设备的导入序列化数据中使用可选 **importMode** 属性来控制每个设备的导入过程。**importMode** 属性具有以下选项：

| importMode | 说明 |
| -------- | ----------- |
| **createOrUpdate** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则以所提供的输入数据覆盖现有信息，而不管 **ETag** 值为何。 |
| **create** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则在日志文件中写入错误。 |
| **update** | 如果已存在具有指定 **ID** 的设备，则以提供的输入数据覆盖现有信息，而不管 **ETag** 值为何。<br/>如果设备不存在，则在日志文件中写入错误。 |
| **updateIfMatchETag** | 如果已存在具有指定 **ID** 的设备，则仅当 **ETag** 匹配时，才以提供的输入数据覆盖现有信息。<br/>如果设备不存在，则在日志文件中写入错误。<br/>如果 **ETag** 不匹配，则在日志文件中写入错误。 |
| **createOrUpdateIfMatchETag** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则仅当 **ETag** 匹配时，才以提供的输入数据覆盖现有信息。<br/>如果 **ETag** 不匹配，则在日志文件中写入错误。 |
| **delete** | 如果已存在具有指定 **ID** 的设备，则将它删除，而不管 **ETag** 值为何。<br/>如果设备不存在，则在日志文件中写入错误。 |
| **deleteIfMatchETag** | 如果已存在具有指定 **ID** 的设备，则仅当 **ETag** 匹配时才将它删除。如果设备不存在，则在日志文件中写入错误。<br/>如果 ETag 不匹配，则在日志文件中写入错误。 |

> [AZURE.NOTE] 如果序列化数据未显式定义设备的 **importMode** 标志，则在导入操作期间将默认使用 **createOrUpdate**。

## 导入设备示例 – 批量预配设备 

以下 C# 代码示例说明了如何生成多个具有下述功能的设备标识：

- 包括身份验证密钥。
- 将该设备信息写入块 blob。
- 将设备导入标识注册表。


		// Provision 1,000 more devices
		var serializedDevices = new List<string>();

		for (var i = 0; i < 1000; i++)
		{
		// Create a new ExportImportDevice
		  var deviceToAdd = new ExportImportDevice()
		  {
		    Id = Guid.NewGuid().ToString(),
		    Status = DeviceStatus.Enabled,
		    Authentication = new AuthenticationMechanism()
		    {
		      SymmetricKey = new SymmetricKey()
		      {
		        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
		        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
		      }
		    },
		    ImportMode = ImportMode.Create
		  };

		  // Add device to existing list
		  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
		}

		// Write this list to the blob
		var sb = new StringBuilder();
		serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
		await blob.DeleteIfExistsAsync();

		using (CloudBlobStream stream = await blob.OpenWriteAsync())
		{
		  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
		  for (var i = 0; i < bytes.Length; i += 500)
		  {
		    int length = Math.Min(bytes.Length - i, 500);
		    await stream.WriteAsync(bytes, i, length);
		  }
		}

		// Call import using the same blob to add new devices!
		// This normally takes 1 minute per 100 devices the normal way
		JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

		// Wait until job is finished
		while(true)
		{
		  importJob = await registryManager.GetJobAsync(importJob.JobId);
		  if (importJob.Status == JobStatus.Completed || 
		      importJob.Status == JobStatus.Failed ||
		      importJob.Status == JobStatus.Cancelled)
		  {
		    // Job has finished executing
		    break;
		  }

		  await Task.Delay(TimeSpan.FromSeconds(5));
		}


## 导入设备示例 – 批量删除

以下代码示例演示如何删除使用前面代码示例添加的设备：


		// Step 1: Update each device's ImportMode to be Delete
		sb = new StringBuilder();
		serializedDevices.ForEach(serializedDevice =>
		{
		  // Deserialize back to an ExportImportDevice
		  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

		  // Update property
		  device.ImportMode = ImportMode.Delete;

		  // Re-serialize
		  sb.AppendLine(JsonConvert.SerializeObject(device));
		});

		// Step 2: Write the new import data back to the block blob
		await blob.DeleteIfExistsAsync();
		using (CloudBlobStream stream = await blob.OpenWriteAsync())
		{
		  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
		  for (var i = 0; i < bytes.Length; i += 500)
		  {
		    int length = Math.Min(bytes.Length - i, 500);
		    await stream.WriteAsync(bytes, i, length);
		  }
		}

		// Step 3: Call import using the same blob to delete all devices!
		importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

		// Wait until job is finished
		while(true)
		{
		  importJob = await registryManager.GetJobAsync(importJob.JobId);
		  if (importJob.Status == JobStatus.Completed || 
		      importJob.Status == JobStatus.Failed ||
		      importJob.Status == JobStatus.Cancelled)
		  {
		    // Job has finished executing
		    break;
		  }

		  await Task.Delay(TimeSpan.FromSeconds(5));
		}



## 获取容器 SAS URI


以下代码示例演示如何生成具有 Blob 容器读取、写入和删除权限的 [SAS URI](/documentation/articles/storage-dotnet-shared-access-signature-part-2/)：


		static string GetContainerSasUri(CloudBlobContainer container)
		{
		  // Set the expiry time and permissions for the container.
		  // In this case no start time is specified, so the
		  // shared access signature becomes valid immediately.
		  var sasConstraints = new SharedAccessBlobPolicy();
		  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
		  sasConstraints.Permissions = 
		    SharedAccessBlobPermissions.Write | 
		    SharedAccessBlobPermissions.Read | 
		    SharedAccessBlobPermissions.Delete;

		  // Generate the shared access signature on the container,
		  // setting the constraints directly on the signature.
		  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

		  // Return the URI string for the container,
		  // including the SAS token.
		  return container.Uri + sasContainerToken;
		}



## 后续步骤
在本文中，已学习如何对 IoT 中心内的标识注册表执行批量操作。若要了解有关如何管理 Azure IoT 中心的详细信息，请参阅以下链接：

* [IoT 中心指标][lnk-metrics]
* [操作监视][lnk-monitor]

若要进一步探索 IoT 中心的功能，请参阅：

* [IoT 中心开发人员指南][lnk-devguide]
* [使用 IoT 网关 SDK 模拟设备][lnk-gateway]

[lnk-metrics]: /documentation/articles/iot-hub-metrics/
[lnk-monitor]: /documentation/articles/iot-hub-operations-monitoring/

[lnk-devguide]: /documentation/articles/iot-hub-devguide/
[lnk-gateway]: /documentation/articles/iot-hub-linux-gateway-sdk-simulated-device/

<!---HONumber=Mooncake_0109_2017-->
<!--Update_Description:update meta properties-->