---
description: 使用 RDS 与 ODBC 连接池
title: 将 RDS 与 ODBC 连接池一起使用 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling in RDS [ADO]
ms.assetid: e8b912c1-da5b-4e85-a000-1e6648a94237
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c353d7e0038f40eea574b895f2cd55ea3f47585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032589"
---
# <a name="using-rds-with-odbc-connection-pooling"></a>使用 RDS 与 ODBC 连接池
如果正在使用 ODBC 数据源，可以使用 Internet Information Services (IIS) 中的连接池选项来实现客户端负载的高性能处理。 连接池是用于连接的资源管理器，用于维护经常使用的连接的打开状态。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 若要启用连接池，请参阅 Internet Information Services 文档。  
  
 请注意，启用连接池可能会使 Web 服务器遭受其他限制，如 Microsoft Internet Information Services 文档中所述。  
  
 若要确保连接池稳定并提供更高的性能提升，你必须将 Microsoft SQL Server 配置为使用 TCP/IP 套接字网络库。  
  
 若要实现此目的，需要：  
  
-   将 SQL Server 计算机配置为使用 TCP/IP 套接字。  
  
-   将 Web 服务器配置为使用 TCP/IP 套接字。  
  
## <a name="configuring-the-sql-server-computer-to-use-tcpip-sockets"></a>将 SQL Server 计算机配置为使用 TCP/IP 套接字  
 在 SQL Server 计算机上，运行 SQL Server 安装程序，以便与数据源进行交互使用 TCP/IP 套接字网络库。  
  
### <a name="to-specify-the-tcpip-socket-network-library-on-the-sql-server-computer"></a>在 SQL Server 计算机上指定 TCP/IP 套接字网络库  
  
### <a name="in-microsoft-sql-server-65"></a>在 Microsoft SQL Server 6.5：  
  
1.  从 "开始" 菜单中，依次指向 "程序"、Microsoft SQL Server 6.5 "，然后单击" SQL 安装程序 "。  
  
2.  单击 "继续" 两次。  
  
3.  在 "Microsoft SQL Server 选项" 对话框中，选择 "更改网络支持"，然后单击 "继续"。  
  
4.  请确保已选中 "TCP/IP 套接字" 复选框，然后单击 "确定"。  
  
5.  单击 "继续" 以完成，然后退出安装程序。  
  
### <a name="in-microsoft-sql-server-70"></a>在 Microsoft SQL Server 7.0：  
  
1.  从 "开始" 菜单，依次指向 "程序"、Microsoft SQL Server 7.0 "，然后单击" 服务器网络实用工具 "。  
  
2.  在对话框的 "常规" 选项卡上，单击 "添加"。  
  
3.  在 "添加网络库配置" 对话框中，单击 "TCP/IP"。  
  
4.  在 "端口号" 和 "代理地址" 框中，输入网络管理员提供的端口号和代理地址。  
  
5.  单击 "确定" 完成，并退出安装程序。  
  
## <a name="configuring-the-web-server-to-use-tcpip-sockets"></a>将 Web 服务器配置为使用 TCP/IP 套接字  
 有两个选项可用于将 Web 服务器配置为使用 TCP/IP 套接字。 具体操作取决于是否从 Web 服务器访问所有 SQL 服务器，或者是否仅从 Web 服务器访问特定 SQL Server。  
  
 如果所有 SQL 服务器都是从 Web 服务器访问的，则需要在 Web 服务器计算机上运行 SQL Server 的客户端配置实用程序。 以下步骤将更改从此 IIS Web 服务器建立的所有 SQL Server 连接的默认网络库，以使用 TCP/IP 套接字网络库。  
  
### <a name="to-configure-the-web-server-all-sql-servers"></a>若要配置 Web 服务器 (所有 SQL server)   
  
### <a name="for-microsoft-sql-server-65"></a>对于 Microsoft SQL Server 6.5：  
  
1.  从 "开始" 菜单中，依次指向 "程序"、Microsoft SQL Server 6.5 "，然后单击" SQL 客户端配置实用工具 "。  
  
2.  单击 "网络库" 选项卡。  
  
3.  在 "默认网络" 框中，选择 "TCP/IP 套接字"。  
  
4.  单击 "完成" 保存更改并退出实用程序。  
  
### <a name="for-microsoft-sql-server-70"></a>对于 Microsoft SQL Server 7.0：  
  
1.  从 "开始" 菜单，依次指向 "程序"、Microsoft SQL Server 7.0 "，然后单击" 客户端网络实用工具 "。  
  
2.  单击“常规”选项卡。  
  
3.  在 "默认网络库" 框中，单击 "TCP/IP"。  
  
4.  单击 "确定" 保存更改并退出实用程序。  
  
 如果从 Web 服务器访问特定的 SQL Server，则需要在 Web 服务器计算机上运行 SQL Server 客户端配置实用程序。 若要更改特定 SQL Server 连接的网络库，请按如下所示在 Web 服务器计算机上配置 SQL Server 客户端软件。  
  
### <a name="to-configure-the-web-server-a-specific-sql-server"></a>若要配置 Web 服务器 (特定的 SQL Server)   
  
### <a name="for-microsoft-sql-server-65"></a>对于 Microsoft SQL Server 6.5：  
  
1.  从 "开始" 菜单中，依次指向 "程序"、Microsoft SQL Server 6.5 "，然后单击" SQL 客户端配置实用工具 "。  
  
2.  单击“高级”选项卡。  
  
3.  在 "服务器" 框中，键入要使用 TCP/IP 套接字连接到的服务器的名称。  
  
4.  在 "DLL 名称" 框中，选择 "TCP/IP 套接字"。  
  
5.  单击 "添加/修改"。 指向此服务器的所有数据源现在将使用 TCP/IP 套接字。  
  
6.  单击“完成”。  
  
### <a name="for-microsoft-sql-server-70"></a>对于 Microsoft SQL Server 7.0：  
  
1.  从 "开始" 菜单中，依次指向 "程序"、Microsoft SQL Server 7.0 "，然后单击" 客户端配置实用工具 "。  
  
2.  单击“常规”选项卡。  
  
3.  单击“添加”。  
  
4.  在 "服务器别名" 框中输入服务器的别名。 在 "网络库" 框中，单击 "TCP/IP"。 在 "计算机名称" 框中，输入侦听 TCP/IP 套接字客户端的计算机的计算机名称。 在 "端口号" 框中，输入 SQL Server 侦听的端口。  
  
5.  单击 "确定"，然后再次单击 "确定" 退出实用程序。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)