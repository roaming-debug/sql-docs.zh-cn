---
description: SQL Server Compact Edition 连接管理器
title: SQL Server Compact Edition 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlmobileconnection.connection.f1
- sql13.dts.designer.sqlmobileconnection.all.f1
helpviewer_keywords:
- SQL Server Compact, connection manager
- connections [Integration Services], SQL Server Compact
- connection managers [Integration Services], SQL Server Compact
ms.assetid: ba627d4d-41f4-49fc-a921-f534cde67770
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae61fcfcec740ee26a7fc2a57c0e03b0141e6e10
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425939"
---
# <a name="sql-server-compact-edition-connection-manager"></a>SQL Server Compact Edition 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 连接管理器使包能够连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 目标使用该连接管理器将数据加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库中的表。  
  
> [!NOTE]  
>  在 64 位计算机上，必须以 32 位模式运行连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据源的包。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Compact 数据源的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Provider 只在 32 位版本中提供。  
  
## <a name="configuration-the-sql-server-compact-edition-connection-manager"></a>SQL Server Compact Edition 连接管理器的配置  
 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 连接管理器添加到包时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建一个在运行时解析为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 连接的连接管理器，设置该连接管理器的属性，并将该连接管理器添加到包的 **Connections** 集合。  
  
 该连接管理器的 **ConnectionManagerType** 属性设置为 **SQLMOBILE**。  
  
 可以通过以下方式配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 连接管理器：  
  
-   提供指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库位置的连接字符串。  
  
-   为受密码保护的数据库提供密码。  
  
-   指定存储数据库的服务器。  
  
-   指示是否在运行时保留从连接管理器中创建的连接。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
## <a name="sql-server-compact-edition-connection-manager-editor-connection-page"></a>SQL Server Compact Edition 连接管理器编辑器（“连接”页）
  可以使用 **“SQL Server Compact Edition 连接管理器”** 对话框，指定连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库时使用的属性。  
  
 若要了解有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition 连接管理器的详细信息，请参阅 [SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **输入数据库文件名和路径**  
 输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的路径和文件名。  
  
 **“浏览”**  
 通过使用“选择 SQL Server Compact Edition 数据库”对话框找到所需的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库文件。  
  
 **输入数据库密码**  
 输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的密码。  
  
## <a name="sql-server-compact-edition-connection-manager-editor-all-page"></a>SQL Server Compact Edition 连接管理器编辑器（“全部”页）
  可以使用 **“SQL Server Compact Edition 连接管理器”** 对话框，指定连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库时使用的属性。  
  
 若要了解有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition 连接管理器的详细信息，请参阅 [SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **自动收缩阈值**  
 指定运行自动收缩进程前， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库中允许使用的可用空间大小（以百分比表示）。  
  
 **默认锁升级**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库在尝试升级锁之前获取的数据库锁的数量。  
  
 **默认锁超时值**  
 指定事务等待锁的默认时间间隔（毫秒）。  
  
 **刷新间隔**  
 指定提交的事务将数据刷新到磁盘的时间间隔（秒）。  
  
 **区域设置标识符**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的区域设置 ID (LCID)。  
  
 **最大缓冲区大小**  
 指定在将数据刷新到磁盘之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 使用的最大内存量 (KB)。  
  
 **最大数据库大小**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的最大大小 (MB)。  
  
 **模式**  
 指定打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的文件模式。 此属性的默认值为 **“读写”**。  
  
 “模式”选项包含四个值，如下表所述：  
  
|值|说明|  
|-----------|-----------------|  
|**只读**|指定对数据库的只读访问。|  
|**“读写”**|指定对数据库的读/写权限。|  
|**独占**|指定对数据库的排他访问。|  
|**共享读取**|指定其他用户可以同时读取该数据库。|  
  
 **Persist Security Info**  
 指定安全信息是否作为连接字符串的一部分返回。 此选项的默认值为 **False**。  
  
 **临时文件目录**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 临时数据库文件的位置。  
  
 **数据源**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的名称。  
  
 **密码**  
 输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库的密码。  
  
