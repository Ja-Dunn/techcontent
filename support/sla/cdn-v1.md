﻿<properties
	pageTitle=""
    description=""
    services=""
    documentationCenter=""
    authors=""
    manager=""
    editor=""
    tags=""/>

<tags ms.service="legal" ms.date="03/2016" wacn.date="03/2016" wacn.lang="cn"/>

> [AZURE.LANGUAGE]
- [中文](/support/sla/cdn-v1/)
- [English](/support/sla/cdn-en-v1/)
#CDN 的服务级别协议

我们保证至少在 99.9% 的时间 CDN 将响应客户端请求并交付请求的内容，而不会出现错误。我们将检查并接受来自您选择用来监控内容的任何商业上合理的独立度量系统的数据。您必须从测量系统中的标准代理列表中选择一组普遍可用的代理，这些代理通常可用且代表中华人民共和国的主要大城市区域中的至少五个不同的地理位置。


##引言
 

本 Azure 服务级别协议（以下简称“服务级别协议”）由世纪互联制定，与客户从世纪互联处购买 Azure 服务所依据的协议（以下简称“协议”）相关并构成该协议的一部分。

为了保证达到并保持所提供服务的服务级别，我们会提供财务方面的支持。如果我们未能达到和保持本服务级别协议中说明的每种服务的服务级别，则您有资格获得月度服务费用的部分服务费抵扣。在您的协议期间，这些条款都不会做任何变动。如果续展订购，则在续订期限开始时实行的本服务级别协议的版本将适用于整个续订期限。如果对本服务级别协议有任何重大不利变更，我们应至少提前九十 (90) 天进行通知。您可以随时访问 [http://www.azure.cn/support/legal/sla/](/support/legal/sla/) 查看本服务级别协议的最新版本。


##一般条款
 

###一、定义
 
1. “索赔” 指客户根据本服务级别协议向世纪互联提交的、有关尚未达到某个服务级别以及客户可获得的服务费抵扣的索赔。

2. “客户”指签订本协议的机构。

3. “客户支持”指世纪互联可由此为客户提供帮助以解决服务问题的服务。

4. “错误代码”用于指示某项操作出现了问题，例如，5xx 范围内的 HTTP 状态代码。

5. “外部连接”是指可通过受支持的协议（例如 HTTP 和 HTTPS）从公共 IP 地址发送和接收的双向网络流量。

6. “事件”表示导致无法达到服务级别的任何情况。

7. “管理门户”指由世纪互联提供的 web 界面，客户可以通过该界面来管理服务。

8. “世纪互联”指客户协议上载明的世纪互联实体。

9. “预览版”指提供用来获得客户反馈的服务或软件的预览版、测试版或其他预发行版。

10. “服务”表示根据协议为客户提供的并在以下指定服务级别协议的 Azure 服务。

11. “服务费抵扣”表示针对受影响的服务或服务资源的已经证实的服务索赔，返还给客户的月度服务费用的百分比。

12. “服务级别”指定世纪互联选择遵守并据此衡量其所提供的每种服务的服务级别的标准，具体如下所述。

13. “服务资源”指某个服务内可供使用的单独资源。

14. “成功代码”用于指示某项操作已经成功，例如，2xx 范围内的 HTTP 状态代码。

15. “支持时段”指支持某个服务功能或者支持与某个单独产品或服务兼容的时间范围。

16. “虚拟网络”指虚拟专用网络，包括用户定义的 IP 地址和子网的集合，形成了 Azure 内的网络边界。

17. “虚拟网络网关”是指促成虚拟网络和客户本地网络之间跨区域连接的网关。

###二、服务费抵扣索赔

1. 客户必须在事件（索赔主题）发生的帐单月份结束之后的两 (2) 个月内向客户支持提交索赔，世纪互联才会受理该索赔。客户必须向客户支持提供世纪互联证实该索赔所需的所有信息，包括但不限于：事件的详细说明、事件的发生时间和持续时间、受影响的资源或操作以及客户尝试解决该事件所做出的任何努力。

2. 世纪互联将利用所有可合理获得的信息来验证索赔，并判定是否应兑现服务费抵扣。

3. 如果由于同一事件导致未能达到某个特定服务的多个服务级别，客户必须仅选择一个服务级别并基于该服务级别针对此次事件提出索赔。

4. 服务费抵扣仅适用于为未达到服务级别的特定服务、服务资源或服务层级所支付的费用。如果服务级别适用于单独的服务资源或单独的服务层级，则服务费抵扣仅适用于为受影响的服务资源或服务层级（如果有）所支付的费用。

###三、服务级别协议排除项


本服务级别协议和任何适用的服务级别不适用于由以下情况造成的任何性能或可用性问题：

1. 由于超出世纪互联合理控制范围的因素而导致的问题，例如，在世纪互联数据中心以外（包括在客户的站点或者客户的站点和世纪互联数据中心之间）的网络或设备故障；

2. 由于客户使用非世纪互联提供的硬件、软件或服务（例如，从 Azure 商店购买的第三方软件或服务或者世纪互联提供的其他非 Azure 服务）作为服务的一部分而导致的问题；

3. 由于客户未按与服务功能一致的方式（例如，尝试执行不受支持的操作）或者未按与发布的文档或指南一致的方式来使用服务而导致的问题；

4. 由于错误的输入、指令或参数（例如，请求访问不存在的文件）而导致的问题；

5. 世纪互联建议客户改变服务的使用方式之后，如果客户未按建议改变其使用方式而继续使用服务所导致的问题；

6. 使用预览版期间出现问题或者与预览版有关的问题，或者使用世纪互联订购信用进行购买而导致的问题；

7. 由于客户试图执行的操作超出了规定的服务配额而导致的问题，或者由于限制可疑的滥用行为而导致的问题；

8. 由于客户使用的服务功能超出了相关支持时段而导致的问题；或

9. 由于他人利用客户的密码或设备未经授权地访问世纪互联服务的行为，或由于客户未能遵循适当的安全惯例而导致的问题。

###四、服务费抵扣

1. 针对所描述的每一种服务，下文介绍了服务费抵扣的金额和计算方法。

2. 服务费抵扣是客户针对未能达到任何服务级别的唯一且排他性的救济。

3. 在任何情况下，任何帐单月份内提供的与特定服务或服务资源相关的服务费抵扣都不得超过客户在该帐单月份内用于该服务或服务资源（如果有）的月度服务费用。

4. 对于作为套件一部分购买的服务，服务费抵扣将基于服务所占的成本比例进行计算，这将由世纪互联通过合理的判断确定。如果客户已从转售商处购买服务，服务费抵扣将基于适用服务的估计零售价进行计算，这将由世纪互联通过合理的判断确定。


##SLA 详细信息
 

###CDN 服务的每月正常服务时间计算和服务级别
1. 世纪互联不负责全面监控客户的内容；这是客户的责任。世纪互联将检查并接受来自客户采用的任何商业上合理的独立测量系统的数据。

2. 客户必须从测量系统中的标准代理列表中选择一组代理，这些代理通常可用且代表中华人民共和国的主要大城市区域中的至少五个不同的地理位置。

	1) 配置测量系统测试（频率为每代理每小时至少一次测试），以便根据以下模型执行一个 HTTP(S) GET 操作：

	2) 测试文件将放置在客户的原始位置上（如 Azure 存储帐户）。

	3) GET 操作将从相应 Azure 域名主机名中请求对象，以此通过 CDN 服务来检索文件。

	4) 测试文件应符合以下条件：

	- 测试对象应符合 CDN 规范。例如，响应头不应包括“set-cookie”；缓存控制属性不能设置为“私人”或“无缓存”;“ 过期”值不能设置为一个过去的时间值。

	- 测试对象应当是一个为 50KB 以上但不超过 1MB 的文件。

	- 应当精简原始数据，以清除任何来自测量期间遇到技术问题的代理的测量结果。

	备注：

	- 如果测试结果显示有问题，务必尽快联系我们，以便我们能在第一时间对测试结果进行核对，找到问题的原因；

	- 如果所选测试源自身链路等出现问题，也会导致测试结果异常；对此，最终计算可用性的时候需要排除上述异常的部分；
	
3. “每月正常服务时间百分比”是指 CDN 响应客户请求并且准确无误地提交请求的内容的 HTTP 事务的百分比。CDN 服务的每月正常服务时间百分比按以下方式计算：成功提交对象的次数除以总请求数（在删除错误数据后）。

4. 以下服务级别和服务费抵扣适用于客户对 CDN 服务的使用：

每月正常服务时间百分比 | 服务费抵扣 
---|---  
<99.5% | 5%  
<99% | 20% 
