---
description: 保证 RDS 应用程序的安全
title: 保护 RDS 应用程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS security [ADO]
ms.assetid: 82fb1330-d6c6-4c17-ad3e-d417ff822b25
author: rothja
ms.author: jroth
ms.openlocfilehash: d91b2ef0d4efba7453e3acedb12c8f55d528b1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031799"
---
# <a name="securing-rds-applications"></a>保证 RDS 应用程序的安全
本主题提供 RDS 的安全信息。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="microsoft-internet-explorer-security-issues"></a>Microsoft Internet Explorer 安全问题  
 在 Microsoft Internet Explorer 中添加新的安全增强功能后，某些 ADO 和 RDS 对象只能在 "安全" 模式环境下运行。 这要求你知道这些问题，包括不同的区域、安全级别、限制行为、不安全操作和自定义安全设置。  
  
## <a name="security-and-your-web-server"></a>安全性和 Web 服务器  
 如果在 Internet Web 服务器上使用 [RDSServer DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象，请记住，这样做会带来潜在的安全风险。 获取有效数据源名称 (DSN) 、用户 ID 和密码信息的外部用户可以写入页面，以将任何查询发送到该数据源。 如果需要对数据源进行更多限制的访问，一种选择是取消注册并删除 **RDSServer**) 对象 ( # A0，并改为将自定义业务对象用于硬编码查询。  
  
 若要详细了解如何使用 RDSServer DataFactory 对象，请参阅 Microsoft Security 网站上的 Microsoft 安全公告 MS99-025。  
  
## <a name="client-impersonation-and-security"></a>客户端模拟和安全  
 如果你的 IIS Web 服务器的 " **密码身份验证** " 属性设置为 "windows nt 4.0) 的 Windows nt 质询/响应身份验证 (或 windows 2000) 的集成 windows 身份验证 (，则将在客户端的安全上下文中调用业务对象。 这是 RDS 1.5 中的一项新功能，允许通过 HTTP 进行客户端模拟。 在此模式下工作时，登录到 Web 服务器 (IIS) 不是匿名的，但使用客户端计算机在其下运行的用户 ID 和密码。 如果 ODBC Dsn 设置为使用可信连接，则在客户端的安全上下文下也会发生对数据库（如 SQL Server）的访问。 但这仅适用于数据库与 IIS 在同一台计算机上的情况;不能将客户端凭据传递到另一台计算机。  
  
 例如，用户 id 为 "JohnD" 且密码为 "secret" 的客户端（John Doe）登录到客户端计算机。 他运行基于浏览器的应用程序，该应用程序需要访问 **RDSServer** 对象，以便通过在运行 IIS 的 "MyServer" 计算机上执行 SQL 查询来创建 [记录集](../../reference/ado-api/recordset-object-ado.md) 。 MyServer，运行 Windows NT Server 4.0 的系统设置为使用 Windows NT 质询/响应身份验证，其 ODBC DSN 已选中 "使用可信连接"，并且该服务器还包含 SQL Server 数据源。 当 Web 服务器收到请求时，它会要求客户端提供用户 ID 和密码。 因此，该请求将作为来自 "JohnD"/"Secret" 的 ""/"Secret" 而不是 IUSER_MyServer (，这是) 上的匿名密码身份验证时的默认设置。 同样，在登录到 SQL Server 时，使用 "JohnD"/"Secret"。  
  
 因此，IIS Windows NT 质询/响应身份验证模式允许创建 HTML 页面，而不会明确提示用户输入登录数据库所需的用户 ID 和密码信息。 如果正在使用 IIS 基本身份验证，则这也是必需的。  
  
## <a name="password-authentication"></a>密码身份验证  
 RDS 可以与在以下三种密码身份验证模式之一中运行的 IIS Web 服务器进行通信：匿名、基本或 NT 质询/响应身份验证 (称为 Windows 2000) 中的集成 Windows 身份验证。 这些设置定义 Web 服务器如何控制对其进行访问，例如要求客户端计算机在 NT Web 服务器上具有显式访问权限。