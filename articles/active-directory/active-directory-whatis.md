<properties
    pageTitle="什么是 Azure Active Directory？"
    description="使用 Azure Active Directory 将现有的本地标识扩展到云中，或开发 Azure AD 集成的应用程序。"
    services="active-directory"
    documentationcenter=""
    author="curtand"
    manager="femila"
    editor="" />
<tags
    ms.assetid="498820c4-9ebe-42be-bda2-ecf38cc514ca"
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/14/2017"
    wacn.date="03/07/2017"
    ms.author="curtand" />  


# 什么是 Azure Active Directory？
Azure Active Directory (Azure AD) 是 Microsoft 提供的基于多租户云的目录和标识管理服务。

对于 IT 管理员而言，Azure AD 提供经济实惠、易于使用的解决方案，使员工和业务合作伙伴能够使用单一登录 (SSO) 功能来访问[数千种云 SaaS 应用程序](http://blogs.technet.com/b/ad/archive/2014/09/03/50-saas-apps-now-support-federation-with-azure-ad.aspx)，例如 Office365、Salesforce.com、DropBox 和 Concur。

对于应用程序开发人员而言，通过 Azure AD 可专注于构建应用程序，快速方便地集成全球各地数百万个组织所使用的一流标识管理解决方案。

Azure AD 还包含整套标识管理功能，例如多重身份验证、设备注册、自助密码管理、自助组管理、特权帐户管理、基于角色的访问控制、应用程序使用情况监视、多样化审核以及安全监视和警报。这些功能可以帮助保护基于云的应用程序的安全，简化 IT 流程，削减成本，以及确保实现公司的合规目标。

此外，只需[单击四下](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx)，Azure AD 就能与现有的 Windows Server Active Directory 集成，使组织能够运用现有的本地标识管理系统投资来管理对基于云的 SaaS 应用程序的访问。

如果是 Office365、Azure 或 Dynamics CRM Online 的客户，那么可能不知道自己已在使用 Azure AD。每个 Office365、Azure 和 Dynamics CRM 租户实际上都已经是 Azure AD 租户。每当有需要时，就可以开始使用该租户来为 Azure AD 集成的其他数千种云应用程序管理访问权限！

![Azure AD Connect 堆栈](./media/active-directory-whatis/Azure_Active_Directory.png)

## Azure AD 的可靠性如何？
Azure AD 的多租户、地理分布、高可用性设计意味着可以依赖它来解决最关键的业务需求。在全球运转有 28 个可自动故障转移的数据中心，这让人能够体会到 Azure AD 的高度可靠，即使数据中心发生服务中断，目录数据也至少会在两个以上的地域分散的数据中心内保留副本，并且提供立即访问权限。

有关详细信息，请参阅[服务级别协议](/support/legal/sla/)。

## Azure AD 的优势是什么？
组织可以使用 Azure AD 在许多方面提高员工生产力、简化 IT 流程、提高安全性并削减成本：

- 快速采用云服务，为员工与合作伙伴提供简单的单一登录体验，这是由 Azure AD 提供的、完全自动化的 SaaS 应用程序访问管理和预配服务功能。
- 使员工能够在任何位置，使用他们偏好的设备来工作，访问一流的云应用，以及各种自助功能。
- 轻松安全地管理员工和供应商对公司社交媒体帐户的访问。
- 使用 Azure AD 多重身份验证和条件性访问提高应用程序安全性。
- 实施一致性的自助应用程序访问管理，使企业主能够快速采取行动，同时降低 IT 成本和管理开销。
- 利用安全报告和监视来监视应用程序使用情况并防止企业受到严重威胁。
- 对本地应用程序进行安全的移动（远程）访问。

## Azure AD 与本地 Active Directory 域服务 (AD DS) 的比较

Azure Active Directory (Azure AD) 和本地 Active Directory（Active Directory 域服务，简称 AD DS）都是可以存储目录数据以及管理用户和资源之间通信的系统，涉及用户登录过程、身份验证和目录搜索。

AD DS 是 Windows Server 上的服务器角色，这意味着它可以部署在物理机或虚拟机上。它的层次结构基于 X.500。它使用 DNS 来查找对象，可以使用 LDAP 进行交互，且主要使用 Kerberos 进行身份验证。除了将计算机加入域，Active Directory 还可以启用组织单位 (OU) 和组策略对象 (GPO)，因此可以在域之间建立信任。

Azure AD 是多客户公共目录服务，这意味着用户可以在 Azure AD 中为云服务器和 Office 365 之类的应用程序创建租户。用户和组在平面结构中创建，没有 OU 或 GPO。身份验证通过 SAML、WS-Federation、OAuth 等协议执行。可以查询 Azure AD，但必须使用名为“AD Graph API”的 REST API，而不能使用 LDAP。这些都是基于 HTTP 和 HTTPS 运行的。

可以使用 Azure AD Connect 通过 Azure AD 同步本地标识。

---

### 身份验证和授权详细信息

#### Azure AD
`SAML`、`WS-Federation`、交互式（使用受支持的凭据）、OAuth 2.0、OpenID Connect

#### 本地 AD DS
`SAML`、`WS-Federation`、NTLM、Kerberos、MD5、基本

---

### 对象存储库详细信息

#### Azure AD
通过 Azure AD Graph 和 Microsoft Graph 进行访问

#### 本地 AD DS
X.500 LDAP

---


### 编程访问详细信息

#### Azure AD
MS/Azure AD Graph REST API

#### 本地 AD DS
LDAP

---

### 应用程序 SSO 详细信息

#### Azure AD
`OpenID Connect`、`SAML`

#### 本地 AD DS
`Open-ID Connect`、`SAML`、WS-Fed

---

### 访问管理详细信息

#### Azure AD
资源定义的作用域和基于角色的访问控制、客户端定义的委派权限和应用程序权限、许可框架（强制实施适当的用户/管理员许可，由资源/客户端定义/请求）

通过应用角色进行，可以单独应用，也可以通过组来应用；支持：管理员管理、自助式应用程序访问、用户许可

#### 本地 AD DS
通过 ACL 进行，可以单独应用，也可以通过组来应用；支持：管理员管理

---

### 受支持的凭据详细信息

#### Azure AD
`Username + password`、`Smartcard`

#### 本地 AD DS
`Username + password`、`Smartcard`
 
---







## 如何开始？

**如果是 IT 管理员：**

- [立即试用！](/documentation/services/identity/)- 现在就可以使用此链接注册试用版并部署第一个云解决方案

- 阅读“Azure AD 入门”，获取有关如何设置和快速运行 Azure AD 租户的提示与技巧

**如果你是开发人员：**
 
- 查看 Azure Active Directory 的[开发人员指南](/documentation/articles/active-directory-developers-guide/)

- [开始试用](/documentation/services/identity/) - 立即注册试用版，开始将应用集成到 Azure AD

## 可以从何处了解详细信息？
我们提供了大量的优秀在线资源，以帮助用户了解 Azure AD 的方方面面。下面是一些有助于快速入门的优秀文章：

- [使用 Azure AD Connect 启用目录的混合管理](/documentation/articles/active-directory-aadconnect/)
- [在互联世界中提高安全性](/documentation/articles/multi-factor-authentication/)


- [从任意位置管理密码](/documentation/articles/active-directory-passwords/)
- [Azure Active Directory 的应用程序访问与单一登录是什么？](/documentation/articles/active-directory-appssoaccess-whatis/)
- [什么是 Azure Active Directory 许可？](/documentation/articles/active-directory-licensing-what-is/)

<!---HONumber=Mooncake_0227_2017-->
<!---Update_Description: wording update -->