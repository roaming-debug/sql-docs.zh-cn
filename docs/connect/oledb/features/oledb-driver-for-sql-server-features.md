---
title: 适用于 SQL Server 的 OLE DB 驱动程序功能
description: 了解 OLE DB Driver for SQL Server 支持的功能，例如数据库镜像、异步操作、Azure Active Directory 等。
ms.custom: ''
ms.date: 02/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], features
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07d1c6f77cd9df46269a6d36261369ec057fd778
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751177"
---
# <a name="ole-db-driver-for-sql-server-features"></a>适用于 SQL Server 的 OLE DB 驱动程序功能
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  除了公开 Windows（以前为 Microsoft）数据访问组件 (WDAC) 的功能，适用于 SQL Server 的 OLE DB 驱动程序还实现了许多其他功能来公开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  
  
## <a name="in-this-section"></a>本节内容    
 [使用数据库镜像](../../oledb/features/using-database-mirroring.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持镜像数据库的使用，即能够在备用服务器上保留 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的副本或镜像。  
  
 [执行异步操作](../../oledb/features/performing-asynchronous-operations.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持异步操作，即能够在不阻止调用线程的情况下立即返回。  

[使用 Azure Active Directory](using-azure-active-directory.md)  
讨论 OLE DB Driver 18.2.1 中引入的新的身份验证方法，这些方法具有更安全的默认设置，并允许使用联合标识连接到 Azure SQL 数据库的实例。

 [使用多个活动的结果集 (MARS)](../../oledb/features/using-multiple-active-result-sets-mars.md)  
 讨论 OLE DB Driver for SQL Server 如何支持多个活动结果集 (MARS)。 MARS 支持使用单一数据库连接执行和接收多个结果集。  
  
 [使用 XML 数据类型](../../oledb/features/using-xml-data-types.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持 XML 数据类型，这是一种基于 XML 的数据类型，可用作列类型、变量类型、参数类型或函数返回类型。  
  
 [使用用户定义类型](../../oledb/features/using-user-defined-types.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持用户定义类型 (UDT)，该类型可用于在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中存储对象和自定义数据结构，从而扩展了 SQL 类型系统。  
  
 [使用大值类型](../../oledb/features/using-large-value-types.md)  
 讨论 OLE DB Driver for SQL Server 如何支持大值数据类型，该类型是大型对象数据类型 (LOB)。  
  
 [以编程方式更改密码](../../oledb/features/changing-passwords-programmatically.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持处理过期密码，让你现在无需管理员介入即可在客户端更改密码。  
  
 [使用快照隔离](../../oledb/features/working-with-snapshot-isolation.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持提升行版本控制功能，该功能通过避免读取器-编写器情况提高数据库性能。  
  
 [使用查询通知](../../oledb/features/working-with-query-notifications.md)  
 讨论 OLE DB Driver for SQL Server 如何支持行集修改时的使用者通知。  
  
 [执行大容量复制操作](../../oledb/features/performing-bulk-copy-operations.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序如何支持大容量复制操作，该操作可在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表或视图中传入或传出大量数据。  
  
 [使用不带验证的加密](../../oledb/features/using-encryption-without-validation.md)  
 讨论如何使用 OLE DB Driver for SQL Server 对发送到服务器的数据加密，而无需验证证书。  
  
 [表值参数（适用于 SQL Server 的 OLE DB 驱动程序）](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md)  
 讨论 OLE DB Driver for SQL Server 对表值参数的支持。  
  
 [大型 CLR 用户定义类型](../../oledb/features/large-clr-user-defined-types.md)  
 讨论对大型公共语言运行时 (CLR) 用户定义类型 (UDT) 的支持。  
  
 [FILESTREAM 支持](../../oledb/features/filestream-support.md)  
 讨论 OLE DB Driver for SQL Server 对增强的 FILESTREAM 功能的支持。  
  
 [客户端连接中的服务主体名称 (SPN) 支持](../../oledb/features/service-principal-name-spn-support-in-client-connections.md)  
 讨论如何扩展对服务主体名称 (SPN) 的支持，以便能够跨所有协议进行相互身份验证。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序的稀疏列支持](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md)  
 讨论 OLE DB Driver for SQL Server 对稀疏列的支持。  
  
 [日期和时间改进](../../oledb/features/date-and-time-improvements.md)  
 讨论 OLE DB Driver for SQL Server 中添加的对日期和时间数据类型的支持。  
  
 [元数据发现](../../oledb/features/metadata-discovery.md)  
 讨论对 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中的元数据发现功能进行的改进。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序的 UTF-16 支持](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)  
 讨论 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中引入的行为更改。 在下列情况下，适用于 SQL Server 的 OLE DB 驱动程序不向缓冲区添加高代理项码位：如果在绑定列结果或输出参数时提供了长度固定的缓冲区，如果在缓冲区中终止符前面写入的 wchar 字符是代理项对的高代理项码位，以及如果下一个 wchar 字符是一个低代理项码位   。  
 
 [适用于 SQL Server 的 OLE DB 驱动程序中的 UTF-8 支持](../../oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
 讨论对 UTF-8 服务器编码的支持以及用户在使用 UTF-8 编码数据时应采取的配置防范措施。
  
 [适用于 SQL Server 的 OLE DB 驱动程序对高可用性和灾难恢复的支持](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)  
 讨论如何配置应用程序以利用 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中添加的高可用性和灾难恢复功能。  
  
 [访问扩展事件日志中的诊断信息](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 讨论针对适用于 SQL Server 的 OLE DB 驱动程序和数据跟踪功能的增强，数据跟踪可让你访问环形缓冲区和 XEvents 日志中的诊断信息。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序对 LocalDB 的支持](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md)  
 讨论 OLE DB Driver for SQL Server 对 LocalDB 功能的支持。  
  
 [使用透明网络 IP 解析](../../oledb/features/using-transparent-network-ip-resolution.md)  
 讨论 OLE DB Driver for SQL Server 如何在故障转移群集中支持透明网络 IP 解析。  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序](../../oledb/oledb-driver-for-sql-server.md)      
 [OLE DB 操作指南主题](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)   
 [安装适用于 SQL Server 的 OLE DB 驱动程序](../../oledb/applications/installing-oledb-driver-for-sql-server.md)  
  
  
