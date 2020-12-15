---
title: 功能
description: 除了公开 Windows 数据访问组件的功能外，SQL Server Native Client 还实现了用于公开 SQL Server 功能的其他功能。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- SQLNCLI, about SQL Server Native Client
- data access [SQL Server Native Client], features
ms.assetid: 7bb32865-5afb-41ab-98b4-3fa545ee8953
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d192f469759ae57d01f4789aef887f4cbb60adca
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462098"
---
# <a name="sql-server-native-client-features"></a>SQL Server Native Client 功能
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  除公开 Windows（以前为 Microsoft）数据访问组件 (WDAC) 的功能以外，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 还实现诸多其他功能以公开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  
  
## <a name="in-this-section"></a>本节内容  
 [处理字符转换时 ODBC 驱动程序行为的变化](../../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  
 介绍从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2012 Native Client 开始的行为变化。  
  
 [使用数据库镜像](../../../relational-databases/native-client/features/using-database-mirroring.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持使用镜像数据库，这是在备用服务器上保留数据库的副本或镜像功能 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 [执行异步操作](../../../relational-databases/native-client/features/performing-asynchronous-operations.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持异步操作，即在不阻塞调用线程的情况下立即返回的功能。  
  
 [使用多个活动的结果集 (MARS)](../../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持多个活动结果集 (MARS)。 MARS 支持使用单一数据库连接执行和接收多个结果集。  
  
 [使用 XML 数据类型](../../../relational-databases/native-client/features/using-xml-data-types.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持 XML 数据类型，此基于 XML 的数据类型可用作列类型、变量类型、参数类型或函数返回类型。  
  
 [使用用户定义类型](../../../relational-databases/native-client/features/using-user-defined-types.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持 User-Defined 类型 (UDT) ，它通过允许您在数据库中存储对象和自定义数据结构，从而扩展了 SQL 类型系统 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 [使用大值类型](../../../relational-databases/native-client/features/using-large-value-types.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持大值数据类型，该类型是大型对象数据类型 (LOB)。  
  
 [以编程方式更改密码](../../../relational-databases/native-client/features/changing-passwords-programmatically.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持处理过期密码，现在可以在不需要管理员介入的情况下在客户端更改密码。  
  
 [使用快照隔离](../../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持对行版本控制的增强功能，该功能通过避免读取器-编写器阻塞情况来提高数据库性能。  
  
 [使用查询通知](../../../relational-databases/native-client/features/working-with-query-notifications.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持基于行集修改的使用者通知。  
  
 [执行大容量复制操作](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 如何支持大容量复制操作，这些操作允许将大量数据传入或传出 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表或视图。  
  
 [使用不带验证的加密](../../../relational-databases/native-client/features/using-encryption-without-validation.md)  
 讨论如何使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对发送到服务器的数据加密，而无需验证证书。  
  
 [表值参数 &#40;SQL Server Native Client&#41;](../../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对表值参数的支持。  
  
 [大型 CLR 用户定义类型](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
 讨论对大型公共语言运行时 (CLR) 用户定义类型 (UDT) 的支持。  
  
 [FILESTREAM 支持](../../../relational-databases/native-client/features/filestream-support.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对增强型 FILESTREAM 功能的支持。  
  
 [客户端连接中的服务主体名称 (SPN) 支持](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
 讨论如何扩展对服务主体名称 (SPN) 的支持，以便能够跨所有协议进行相互身份验证。  
  
 [SQL Server Native Client 中的稀疏列支持](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对稀疏列的支持。  
  
 [日期和时间改进](../../../relational-databases/native-client/features/date-and-time-improvements.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中添加的对日期和时间数据类型的支持。  
  
 [元数据发现](../../../relational-databases/native-client/features/metadata-discovery.md)  
 讨论对 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中的元数据发现功能进行的改进。  
  
 [SQL Server Native Client 11.0 中的 UTF-16 支持](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  
 讨论 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中引入的行为更改。 如果在绑定列结果或输出参数时提供固定长度的缓冲区，并且在终止字符之前写入缓冲区中的 **wchar** 字符是代理项对的高代理项码位，并且下一个 **wchar** 字符是低代理项码位，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将不会向缓冲区添加高代理项码位。  
  
 [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
 讨论如何配置应用程序以利用 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中添加的高可用性和灾难恢复功能。  
  
 [访问扩展事件日志中的诊断信息](../../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 讨论对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 和跟踪数据的增强功能，它们使您可以访问环形缓冲区和 XEvents 日志中的诊断信息。  
  
 [SQL Server Native Client 对 LocalDB 的支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  
 讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对 LocalDB 功能的支持。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 编程](../../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [ODBC 操作指南主题](../../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [OLE DB 操作指南主题](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)   
 [安装 SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
