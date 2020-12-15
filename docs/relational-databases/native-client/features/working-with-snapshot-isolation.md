---
description: 在 SQL Server Native Client 中使用快照隔离
title: 使用快照隔离 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], snapshot isolation
- SQLNCLI, snapshot isolation
- isolation levels [SQL Server], snapshot
- DBPROPSET_SESSION property set
- DBDROPSET_DATASOURCEINFO property set
- snapshot isolation [SQL Server Native Client]
- SQL Server Native Client OLE DB provider, snapshot isolation
- SQL Server Native Client ODBC driver, snapshot isolation
- SQL Server Native Client, snapshot isolation
- SQLGetInfo function
- concurrency [SQL Server Native Client]
- SQLSetConnectAttr function
ms.assetid: 39e87eb1-677e-45dd-bc61-83a4025a7756
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49dcf3c6478de561ee71a8f55f0f093ada158c7b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463348"
---
# <a name="working-with-snapshot-isolation-in-sql-server-native-client"></a>在 SQL Server Native Client 中使用快照隔离
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了旨在增强联机事务处理 (OLTP) 应用程序的并发处理能力的新“快照”隔离级别。 在早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，并发仅基于锁定，对于某些应用程序，这可能导致阻塞和死锁问题。 快照隔离依赖于行版本控制的增强功能，旨在通过避免读取器-编写器阻塞情况来提高性能。  
  
 在快照隔离下启动的事务读取该事务启动时的数据库快照。 一种结果是在快照事务上下文中打开的键集、动态和静态服务器游标的行为与在可序列化事务中打开的静态游标的行为非常相似。 但是，当在快照隔离级别下打开游标时，则不会取得锁，这会减少服务器的阻塞。  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序具有充分利用中引入的快照隔离的增强功能 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 。 这些增强功能包括对 DBPROPSET_DATASOURCEINFO 和 DBPROPSET_SESSION 属性集的更改。  
  
### <a name="dbpropset_datasourceinfo"></a>DBPROPSET_DATASOURCEINFO  
 已更改 DBPROPSET_DATASOURCEINFO 属性集，以便指示通过添加在 DBPROP_SUPPORTEDTXNISOLEVELS 中使用的 DBPROPVAL_TI_SNAPSHOT 值来支持快照隔离级别。 此新值指示支持快照隔离级别，而不管是否对数据库启用了版本控制。 下面列出了 DBPROP_SUPPORTEDTXNISOLEVELS 值：  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_SUPPORTEDTXNISOLEVELS|类型：VT_I4<br /><br /> R/W：只读<br /><br /> 说明：指定支持的事务隔离级别的位掩码。 零个或多个下列各项的组合：<br /><br /> DBPROPVAL_TI_CHAOS<br /><br /> DBPROPVAL_TI_READUNCOMMITTED<br /><br /> DBPROPVAL_TI_BROWSE<br /><br /> DBPROPVAL_TI_CURSORSTABILITY<br /><br /> DBPROPVAL_TI_READCOMMITTED<br /><br /> DBPROPVAL_TI_REPEATABLEREAD<br /><br /> DBPROPVAL_TI_SERIALIZABLE<br /><br /> DBPROPVAL_TI_ISOLATED<br /><br /> DBPROPVAL_TI_SNAPSHOT|  
  
### <a name="dbpropset_session"></a>DBPROPSET_SESSION  
 已更改 DBPROPSET_SESSION 属性集，以便指示通过添加在 DBPROP_SESS_AUTOCOMMITISOLEVELS 中使用的 DBPROPVAL_TI_SNAPSHOT 值来支持快照隔离级别。 此新值指示支持快照隔离级别，而不管是否对数据库启用了版本控制。 下面列出了 DBPROP_SESS_AUTOCOMMITISOLEVELS 值：  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|类型：VT_I4<br /><br /> R/W：只读<br /><br /> 说明：指定指示在自动提交模式下的事务隔离级别的位掩码。 可在此位掩码中设置的值与可针对 DBPROP_SUPPORTEDTXNISOLEVELS 设置的值相同。|  
  
> [!NOTE]  
>  如果在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 版本时设置 DBPROPVAL_TI_SNAPSHOT，将发生 DB_S_ERRORSOCCURRED 或 DB_E_ERRORSOCCURRED 错误。  
  
 有关如何在事务中支持快照隔离的信息，请参阅[支持本地事务](../../../relational-databases/native-client-ole-db-transactions/supporting-local-transactions.md)。  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序通过对[SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)和[SQLGetInfo](../../../relational-databases/native-client-odbc-api/sqlgetinfo.md)函数进行增强，为快照隔离提供支持。  
  
### <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 **SQLSetConnectAttr** 函数现在支持使用 SQL_COPT_SS_TXN_ISOLATION 特性。 将 SQL_COPT_SS_TXN_ISOLATION 设置为 SQL_TXN_SS_SNAPSHOT 指示事务将在快照隔离级别下发生。  
  
### <a name="sqlgetinfo"></a>SQLGetInfo  
 [SQLGetInfo](../../../relational-databases/native-client-odbc-api/sqlgetinfo.md)函数现在支持已添加到 SQL_TXN_ISOLATION_OPTION 信息类型的 SQL_TXN_SS_SNAPSHOT 值。  
  
 有关如何在事务中支持快照隔离的信息，请参阅 [游标事务隔离级别](../../../relational-databases/native-client-odbc-cursors/properties/cursor-transaction-isolation-level.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [行集属性和行为](../../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
  
