---
description: 运行通讯簿 SQL 脚本
title: 运行通讯簿 SQL 脚本 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 409b3f8b-0ced-4867-acbe-b245dcdf6702
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e36c60e0c3e56ab0c159c362e98c2280e6c5426
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036387"
---
# <a name="running-the-address-book-sql-script"></a>运行通讯簿 SQL 脚本
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 必须使用 ISQL/查询分析器命令行实用工具或 SQL Server Enterprise 管理器来运行包含的 SQL 脚本 (Sampleemp) ：  
  
-   在默认设备上创建新的数据库 AddrBookDB。  
  
-   连接到数据库 AddrBookDB。  
  
-   创建 Employee 表。  
  
-   用示例数据填充该表。  
  
-   运行简单的 SELECT 语句来验证数据库表的填充。  
  
-   设置名为 "adcdemo"、密码为 "adcdemo" 的用户帐户。  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-65"></a>在 Microsoft SQL Server 6.5 中运行 Sampleemp 脚本  
  
1.  单击 " **开始**"，指向 " **程序**"，然后指向 **Microsoft SQL Server 6.5**"。 单击 " **SQL 企业管理器**"。  
  
2.  从 " **工具** " 菜单中，单击 " **SQL 查询工具**"。  
  
3.  单击 " **加载 SQL 脚本** " 并浏览到 c:\Platform SDK\Samples\DataAccess\RDS\AddressBook。  
  
4.  选择 Sampleemp 文件。 单击“打开”。  
  
5.  单击 " **执行查询** " 按钮， (工具栏上的绿色箭头) 。  
  
6.  执行后，关闭 **查询** 和 **企业管理器** 窗口。  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-70"></a>在 Microsoft SQL Server 7.0 中运行 Sampleemp 脚本  
  
1.  单击 " **开始**"，指向 " **程序**"，然后指向 **Microsoft SQL Server 7.0**"。 单击 " **企业管理器**"。  
  
2.  确保在企业管理器中从已注册的服务器列表中选择要使用的 SQL Server。  
  
3.  从 " **工具** " 菜单中，单击 " **SQL Server 查询分析器**"。  
  
4.  单击工具栏上的 " **加载 SQL 脚本** " 按钮 (打开的文件夹) 并浏览到 c:\Platform SDK\Samples\DataAccess\RDS\AddressBook。  
  
5.  选择 Sampleemp 文件。 单击“打开”。  
  
6.  单击工具栏上的绿色箭头 (" **执行查询** " 按钮) 或按 **F5**。  
  
7.  执行后，关闭 **查询**、 **查询分析器** 和 **企业管理器** 窗口。  
  
## <a name="see-also"></a>另请参阅  
 [运行通讯簿示例应用程序](./running-the-address-book-sample-application.md)