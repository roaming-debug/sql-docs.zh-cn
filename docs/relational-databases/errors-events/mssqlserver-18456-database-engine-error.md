---
title: MSSQLSERVER_18456 | Microsoft Docs
description: 由于密码或用户名错误，SQL Server 中的连接尝试被拒。 请查看错误的说明和可能的解决方法。
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18456 (Database Engine error)
ms.assetid: c417631d-be1f-42e0-8844-9f92c77e11f7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c1e2f5aaf5198530b5d7c93ecffb088e7145b9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196519"
---
# <a name="mssqlserver_18456"></a>MSSQLSERVER_18456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|18456|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LOGON_FAILED|  
|消息正文|用户 '%.*ls'.%.\*ls 登录失败|  
  
## <a name="explanation"></a>说明  
因密码或用户名错误而使身份验证失败并导致连接尝试被拒时，类似以下内容的消息将返回到客户端：“用户‘<user_name>’登录失败。 （Microsoft SQL Server，错误：18456）”。  
  
返回到客户端的其他信息有：  
  
“用户‘<user_name>’登录失败。 （.Net SqlClient 数据访问接口）”  
  
-----------------------------\-  
  
“服务器名称：<computer_name>”  
  
“错误编号：18456”  
  
“严重级别：14”  
  
“状态：1”  
  
“行号：65536”  
  
也可能返回以下消息：  
  
“消息 18456、级别 14、状态 1、服务器 <computer_name>、行 1”  
  
“用户‘<user_name>’登录失败。”  
  
## <a name="additional-error-information"></a>其他错误信息  
为了增强安全性，返回到客户端的错误消息有意隐藏身份验证错误的本质。 但是，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中，对应的错误包含映射到身份验证失败条件的错误状态。 将错误状态与以下列表进行比较以确定登录失败的原因。  
  
|状态|说明|  
|---------|---------------|  
|1|无法获得错误信息。 此状态通常意味着您不拥有接收错误详细信息的权限。 请联系 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员以获得详细信息。|  
|2|用户 ID 无效。|  
|5|用户 ID 无效。|  
|6|尝试同时使用 SQL Server 身份验证与 Windows 登录名。|  
|7|登录已禁用，密码不正确。|  
|8|密码不正确。|  
|9|密码无效。|  
|11|登录有效，但服务器访问失败。 导致此错误的一个可能原因是：Windows 用户作为本地管理员组的成员有权访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，但 Windows 没有提供管理员凭据。 若要连接，请使用“以管理员身份运行”选项启动连接程序，然后将 Windows 用户作为特定的登录名添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。|  
|12|登录是有效的登录，但服务器访问失败。|  
|18|必须更改密码。|  
|38、46|找不到用户请求的数据库。|
|58| 将 SQL Server 设置为仅使用 Windows 身份验证，而客户端尝试使用 SQL 身份验证登录时。 另一个原因是 SID 不匹配。|
|102 - 111|AAD 失败。|
|122 - 124|由于用户名或密码为空导致的失败。|
|126|用户请求的数据库不存在。|
|132 - 133|AAD 失败。|
  
存在其他错误状态，并表示一个意外的内部处理错误。  
  
**其他不常见的可能原因**  
  
在以下情况下可能会返回错误原因 **“尝试使用 SQL Server 身份验证登录失败。服务器配置为仅使用 Windows 身份验证。** 可能会在下列情况下返回。  
  
-   当服务器配置为混合模式身份验证并且某个 ODBC 连接使用 TCP 协议，且该连接未显式指定该连接应使用某一可信连接时。  
  
-   当服务器配置为混合模式身份验证并且某个 ODBC 连接使用命名管道，且客户端用来打开命名管道的凭据用于自动模拟该用户并且该连接未显式指定该连接应使用某一可信连接时。  
  
若要解决此问题，应在连接字符串中包含 **TRUSTED_CONNECTION = TRUE**。  
  
## <a name="examples"></a>示例  
在此示例中，身份验证错误状态为 8。 这表示密码不正确。  
  
|Date|源|消息|  
|--------|----------|-----------|  
|2007-12-05 20:12:56.34|登录|错误：18456，严重级别：14，状态：8.|  
|2007-12-05 20:12:56.34|登录|用户‘<user_name>’登录失败。 [客户端: <ip address>]|  
  
> [!NOTE]  
> 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是使用 Windows 身份验证模式安装的，并随后更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 身份验证模式，则最初会禁用 **sa** 登录名。 这将导致状态 7 错误：“用户‘sa’登录失败。”若要启用 **sa** 登录名，请参阅 [更改服务器身份验证模式](~/database-engine/configure-windows/change-server-authentication-mode.md)。  
  
## <a name="user-action"></a>用户操作  
如果您尝试使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接，请验证是否将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为使用混合身份验证模式。 有关更改 SQL Server 身份验证模式的详细信息，请查看[更改服务器身份验证模式](../../database-engine/configure-windows/change-server-authentication-mode.md)
  
如果尝试使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接，请验证 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名是否存在以及拼写是否正确。  
  
如果尝试使用 Windows 身份验证进行连接，请验证您是否正确地登录到相应的域。  
  
如果错误指示状态 1，请与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员联系。  
  
如果您尝试使用您的管理员凭据进行连接，则通过使用“以管理员身份运行”选项启动您的应用程序。 在连接后，将您的 Windows 用户作为单独的登录名添加。  
  
如果[!INCLUDE[ssDE](../../includes/ssde-md.md)]支持包含的数据库，请确认在迁移到包含的数据库用户后未删除登录名。  
  
在本地连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时，在 **NT AUTHORITY\NETWORK SERVICE** 下运行的服务的连接必须使用计算机完全限定域名进行身份验证。 有关详细信息，请参阅本主题中的[如何在 ASP.NET 中使用网络服务帐户来访问资源](/previous-versions/msp-n-p/ff647402(v=pandp.10))