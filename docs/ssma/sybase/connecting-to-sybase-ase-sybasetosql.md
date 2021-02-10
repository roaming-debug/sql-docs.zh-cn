---
title: 连接到 SAP ASE (SybaseToSQL) |Microsoft Docs
description: 了解如何连接到自适应服务器，以将 SAP 自适应服务器企业 (ASE) 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Sybase
ms.assetid: a45a2330-9175-4c9e-af38-ef920e350614
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7afb893d194b874da4cbb6482df4ba8a16e8c0a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056852"
---
# <a name="connecting-to-sap-ase-sybasetosql"></a>连接到 SAP ASE (SybaseToSQL) 

若要将 SAP 自适应服务器企业 (ASE) 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，必须连接到包含要迁移的数据库的自适应服务器。 当你连接时，SSMA 将获取有关自适应服务器上的所有数据库的元数据，并在 Sybase 元数据资源管理器窗格中显示数据库元数据。 SSMA 存储有关数据库服务器的信息，但不存储密码。  
  
到 ASE 的连接会一直保持活动状态，直到关闭项目。 重新打开项目时，如果需要与服务器建立活动连接，则必须重新连接到 ASE。  
  
自适应服务器的元数据不会自动更新。 相反，如果要更新 Sybase 元数据资源管理器中的元数据，则必须手动更新元数据，如本主题后面的 "刷新 Sybase ASE 元数据" 一节中所述。  
  
## <a name="required-ase-permissions"></a>需要 ASE 权限

用于连接到 ASE 的帐户必须至少具有对 master 数据库以及要迁移到或 SQL Azure 的任何源数据库的 **公共** 访问权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 此外，若要选择要迁移的表的权限，用户必须对以下系统表具有 SELECT 权限：  
  
- [source_db] .dbo.sys对象  
- [source_db] .dbo.sys列  
- [source_db] .dbo.sys用户  
- [source_db] .dbo.sys类型  
- [source_db] .dbo.sys约束  
- [source_db] .dbo.sys注释  
- [source_db] .dbo.sys索引  
- [source_db] .dbo.sys引用  
- master.dbo.sys数据库  
  
## <a name="establishing-a-connection-to-ase"></a>建立与 ASE 的连接

当你连接到自适应服务器时，SSMA 将读取数据库服务器上的数据库元数据，然后将此元数据添加到项目文件。 当 SSMA 将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 语法，并在将数据迁移到或 SQL Azure 时，此元数据由使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可以在 "Sybase 元数据资源管理器" 窗格中浏览此元数据，查看各个数据库对象的属性。  
  
> [!IMPORTANT]  
> 尝试连接到数据库服务器之前，请确保数据库服务器正在运行，并且可以接受连接。  
  
**连接到 Sybase ASE**
  
1. 在 " **文件** " 菜单上，选择 " **连接到 Sybase**"。  
  
   如果以前连接到了 Sybase，则命令名称将 **重新连接到 sybase**。  
  
2. 在 " **提供程序** " 框中，选择要连接到 Sybase 服务器的计算机上的任何已安装的提供程序。  
  
3. 在 " **模式** " 框中，选择 " **标准模式** " 或 " **高级模式**"。  
  
   使用 "标准" 模式指定服务器名称、端口、用户名和密码。 使用高级模式提供连接字符串。 此模式通常仅用于故障排除或使用技术支持。  
  
4. 如果选择 " **标准" 模式**，请提供以下值：  
  
    1. 在 " **服务器名称** " 框中，输入或选择数据库服务器的名称或 IP 地址。  
    2. 如果数据库服务器未配置为接受默认端口 (5000) 上的连接，请在 " **服务器端口** " 框中输入用于 Sybase 连接的端口号。  
    3. 在 " **用户名** " 框中，输入具有所需权限的 Sybase 帐户。  
    4. 在 " **密码** " 框中，输入指定用户名的密码。  
  
5. 如果选择 " **高级模式**"，请在 " **连接字符串** " 框中提供连接字符串。  
  
    不同连接字符串的示例如下所示：  
  
    1. **Sybase OLE DB 提供程序的连接字符串：**  
  
        对于 Sybase ASE OLE DB 12.5，连接字符串的示例如下所示：  
  
        `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`  
  
        对于 Sybase ASE OLE DB 15，连接字符串的示例如下所示：  
  
        `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider= ASEOLEDB;Port=5000;`  
  
    2. **Sybase ODBC 提供程序的连接字符串：**  
  
       `Driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`  
  
    3. **Sybase ADO.NET 提供程序的连接字符串：**  
  
       `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`  
  
    有关详细信息，请参阅 [连接到 Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md)。  
  
## <a name="reconnecting-to-sybase-ase"></a>重新连接到 Sybase ASE

与数据库服务器的连接会一直保持活动状态，直到关闭该项目。 重新打开项目时，如果需要与自适应服务器建立活动连接，则必须重新连接。 你可以脱机工作，直至你要更新元数据、将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，以及迁移数据。  
  
## <a name="refreshing-sybase-ase-metadata"></a>刷新 Sybase ASE 元数据

不会自动刷新有关 ASE 数据库的元数据。 Sybase 元数据资源管理器中的元数据是指首次连接到自适应服务器或上次手动刷新元数据时的元数据的快照。 您可以手动更新单个数据库、单个数据库架构或所有数据库的元数据。  
  
**刷新元数据**
  
1. 请确保已连接到自适应服务器。  
  
2. 在 "Sybase 元数据资源管理器" 中，选中要更新的数据库或数据库架构旁边的复选框。  
  
3. 右键单击 "数据库" 或单独的数据库或数据库架构，然后选择 " **从数据库刷新**"。  
  
4. 如果要求您检查当前对象，则单击 **"是"**。  
  
## <a name="next-step"></a>下一步  
  
- 迁移过程的下一步是连接到实例， [SQL Server](connecting-to-sql-server-sybasetosql.md)  /  [连接到实例 SQL Azure](connecting-to-azure-sql-db-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅

[将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
