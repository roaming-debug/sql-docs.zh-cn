---
description: ADO 安全设计功能
title: ADO 安全设计问题 |Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, security
ms.assetid: 86b83a38-efdf-4831-a6d5-7e470d517d1c
author: rothja
ms.author: jroth
ms.openlocfilehash: c992b58b6b84f79efa62c63353e3b8f1efcc333d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030495"
---
# <a name="ado-security-design-features"></a>ADO 安全设计功能
以下部分介绍 ActiveX 数据对象 (ADO) 2.8 及更高版本中的安全设计功能。 在 ADO 2.8 中进行了这些更改，以提高安全性。 Windows Vista 中的 Windows DAC 6.0 随附的 ADO 6.0 在功能上等同于 ADO 2.8，后者包含在 Windows XP 和 Windows Server 2003 的 MDAC 2.8 中。 本主题提供有关如何在 ADO 2.8 或更高版本中最大程度地保护应用程序的信息。

> [!IMPORTANT]
>  如果要从早期版本的 ADO 更新应用程序，建议您在将更新后的应用程序部署到客户之前在非生产计算机上测试该应用程序。 这样，你就可以在部署已更新的应用程序之前确保你已了解任何兼容性问题。

## <a name="internet-explorer-file-access-scenarios"></a>Internet Explorer 文件访问方案
 以下功能影响在 Internet Explorer 中的脚本化网页中使用 ADO 2.8 和更高版本的方式。

### <a name="revised-and-improved-security-warning-message-box-now-used-to-alert-users"></a>现已修订并改善了用于提醒用户的安全警告消息框
 对于 ADO 2.7 及更早版本，当脚本化的网页尝试从不受信任的提供程序运行 ADO 代码时，将显示以下警告消息：

```console
This page accesses data on another domain. Do you want to allow this? To
avoid this message in Internet Explorer, you can add a secure Web site to
your Trusted Sites zone on the Security tab of the Internet Options dialog
box.
```

 对于 ADO 2.8 和更高版本，将不再显示前面的消息。 相反，以下消息会出现在此上下文中：

```console
This Website uses a data provider that may be unsafe. If you trust the
Website, click OK, otherwise click Cancel.
```

 前面的消息允许用户作出明智的决定，同时知道任一选择的结果：

-   如果用户信任站点，则单击 "确定" 将允许所有 ADO 方法和属性 (所有 ADO 方法和属性，但在浏览器窗口中的 "运行" 和 "执行") 的所有 ADO 方法和属性除外。

-   如果用户不信任该站点，则单击 "取消" 将阻止用于数据访问的 ADO 代码在整个中运行和执行。

### <a name="disk-accessible-code-limited-now-to-trusted-sites"></a>磁盘可访问的代码现在限制为受信任的站点
 ADO 2.8 中进行了其他设计更改，这些更改专门限制了一组有限的 Api 的功能，这可能会导致在本地计算机上读取或写入文件。 下面是在运行 Internet Explorer 时进一步限制安全的 API 方法：

-   对于 ADO **流** 对象，如果使用 [LoadFromFile](../reference/ado-api/loadfromfile-method-ado.md) 或 [SaveToFile](../reference/ado-api/savetofile-method.md) 方法，则为。

-   对于 ADO **记录集** 对象，如果使用 [Save](../reference/ado-api/save-method.md) 方法或 [Open](../reference/ado-api/open-method-ado-recordset.md) 方法（如设置了 **AdCmdFile** 选项）或使用 [Microsoft OLE DB 永久性提供程序 (MSPersist)](./appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) ，则为。

 对于这些有限的可能磁盘可访问函数集，如果使用这些方法的任何代码都在 Internet Explorer 中运行，则 ADO 2.8 和更高版本会出现以下行为：

-   如果已将提供代码的站点添加到 "受信任的站点" 区域列表中，则会在浏览器中执行代码，并向本地文件授予访问权限。

-   如果站点未出现在 "受信任的站点" 区域列表中，则会阻止代码，并拒绝对本地文件的访问。

    > [!NOTE]
    >  在 ADO 2.8 和更高版本中，不会向用户发出警报或建议将站点添加到受信任的站点区域列表。 因此，受信任的站点列表的管理是指部署或支持需要访问本地文件系统的基于网站的应用程序的用户。

### <a name="access-blocked-to-the-activecommand-property-on-recordset-objects"></a>对记录集对象的 ActiveCommand 属性的访问被阻止
 在 Internet Explorer 中运行时，ADO 2.8 现在会阻止对活动 **记录集** 对象的 [ActiveCommand](../reference/ado-api/activecommand-property-ado.md)属性的访问，并返回一个错误。 不管页面是否来自 "受信任的站点" 列表中注册的网站，都将发生此错误。

### <a name="changes-in-handling-for-ole-db-providers-and-integrated-security"></a>OLE DB 提供程序和集成安全性的处理更改
 在查看 ADO 2.7 及更早版本时，可能会出现以下情况：

 在某些情况下，支持集成安全性 [DBPROP_AUTH_INTEGRATED](/previous-versions/windows/desktop/ms712973(v=vs.85)) 属性的 OLE DB 提供程序可能会允许脚本化的网页重复使用 ADO 连接对象，以使用用户当前的登录凭据无意连接到其他服务器。 若要防止出现这种情况，ADO 2.8 和更高版本将根据其选择提供（或不提供）以实现集成安全性的方式来处理 OLE DB 提供程序。

 对于从 "受信任的站点区域列表" 中列出的站点加载的网页，下表提供了有关 ADO 2.8 和更高版本在每种情况下如何管理 ADO 连接的细分。

|用于用户身份验证、登录的 IE 设置|提供程序支持 "集成安全性"，UID 和 PWD (SQLOLEDB 指定) |提供程序不支持 "集成安全性" (JOLT，MSDASQL，MSPersist) |提供程序支持 "集成安全性"，并将其设置为 SSPI (未指定 UID/PWD) |
|------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
|用当前用户名和密码自动登录|允许连接|允许连接|允许连接|
|提示输入用户名和密码|允许连接|连接失败|连接失败|
|仅在 Intranet 区域中自动登录|允许连接|提示用户提供安全警告|提示用户提供安全警告|
|匿名登录|允许连接|连接失败|连接失败|

 如果现在出现安全警告，则消息框会通知用户：

```console
This Website is using your identity to access a data source. If you trust this Website, click OK, otherwise click Cancel.
```

 前面的消息使用户能够做出更明智的决策并进行相应的处理。

> [!NOTE]
>  对于不受信任的站点 (即 "受信任的站点区域列表" 中未列出的站点) ，如果提供程序也不受信任 (如本部分前面) 部分所述），则用户可能会在一行中看到两个安全警告，有关安全提供程序的警告以及有关尝试使用其标识的第二条警告。 如果用户在第一个警告中单击 "确定"，则会执行上表中所述的 Internet Explorer 设置和响应行为代码。

## <a name="controlling-whether-password-text-is-returned-in-ado-connection-strings"></a>控制是否在 ADO 连接字符串中返回密码文本
 尝试获取 ADO **连接** 对象的 [ConnectionString](../reference/ado-api/connectionstring-property-ado.md)属性值时，将发生以下事件：

1.  如果连接处于打开状态，则对基础 OLE DB 提供程序进行初始化调用以获取连接字符串。

2.  根据 [DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO](/previous-versions/windows/desktop/ms714905(v=vs.85)) 属性的 OLE DB 提供程序中的设置，密码连同返回的其他连接字符串信息一起提供。

 例如，如果 ADO 连接动态属性的 " **持久安全信息** " 设置为 " **True**"，则返回的连接字符串中包含密码信息。 否则，如果基础提供程序已将属性设置为 **False** (例如，对于 SQLOLEDB 提供程序) ，则在返回的连接字符串中省略密码信息。

 如果你使用的是具有 ADO 应用程序代码的非 Microsoft) OLE DB 提供程序的第三方 (，你可能会检查如何实现 **DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO** 属性，以确定是否允许包含带有 ado 连接字符串的密码信息。

## <a name="checking-for-non-file-devices-when-loading-and-saving-recordsets-or-streams"></a>在加载和保存记录集或流时检查非文件设备
 对于 ADO 2.7 及更早版本，在某些情况下，用于读取和写入基于文件的数据的文件输入/输出操作（如 [打开](../reference/ado-api/open-method-ado-recordset.md) 和 [保存](../reference/ado-api/save-method.md) ）可能会允许使用 URL 或文件名指定非基于磁盘的文件类型。 例如，LPT1、COM2、PRN.TXT、AUX 可以用作系统上的打印机和辅助设备之间的输入/输出别名，

 对于 ADO 2.8 及更高版本，此功能已更新。 对于打开并保存 **记录集** 和 **流** 对象，ADO 现在会执行文件类型检查，以确保在 URL 或文件名中指定的输入或输出设备是实际文件。

> [!NOTE]
>  此部分中所述的文件类型检查仅适用于 Windows 2000 和更高版本。 这不适用于 ADO 2.8 或更高版本在早期 Windows 版本（如 Windows 98）下运行的情况。