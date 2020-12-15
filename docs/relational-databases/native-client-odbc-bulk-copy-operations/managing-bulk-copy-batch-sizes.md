---
title: 管理大容量复制批大小 |Microsoft Docs
description: 了解大容量复制的批大小如何定义事务的作用域，这将影响 SQL Server Native Client ODBC 中的错误行为和锁定开销。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- ODBC, bulk copy operations
- batches [ODBC]
- bulk copy [ODBC], batch sizes
ms.assetid: 4b24139f-788b-45a6-86dc-ae835435d737
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9be8ff6adc10b74ae176011dd1ae0f3a2b9cae1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465018"
---
# <a name="managing-bulk-copy-batch-sizes"></a>管理大容量复制的批大小
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在大容量复制操作中，批的主要目的在于定义事务的范围。 如果没有设置批大小，则大容量复制函数会将整个大容量复制视为一个事务。 如果设置了批大小，则每个批构成一个事务，当批运行完成时，该事务也就被提交。  
  
 如果在没有指定批大小的情况下执行大容量复制并且遇到错误，则将回滚整个大容量复制。 恢复长时间运行的大容量复制可能需要花费很长的时间。 如果设置了批大小，则大容量复制将每个批视为一个事务并分别提交每个批。 如果遇到错误，只需回滚最后一个未完成的批。  
  
 此外，批大小还会影响锁定开销。 对执行大容量复制时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以使用 [BCP_CONTROL](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) 指定 TABLOCK 提示，以获取表锁而不是行锁。 对于整个大容量复制操作而言，持有单个表锁的开销最小。 如果未指定 TABLOCK，则对各个行持有锁，并且在大容量复制操作期间维护所有锁的开销会导致性能降低。 由于仅在事务执行期间才持有锁，因而可以通过指定批大小来解决此问题，因为这样可以定期生成能够释放当前持有的锁的提交。  
  
 如果要大容量复制大量的行，构成批的行的数目会对性能产生显著影响。 建议采用的批大小取决于要执行的大容量复制的类型。  
  
-   在对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行大容量复制时，请指定 TABLOCK 大容量复制提示并设置一个较大的批大小。  
  
-   如果不指定 TABLOCK，请将批大小限制为小于 1,000 行。  
  
 从数据文件大容量复制时，在调用 [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md)之前，通过使用 BCPBATCH 选项调用 **bcp_control** 来指定批大小。 当使用 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)和 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)从程序变量大容量复制时，将在调用 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) *x* 次后调用 [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md)来控制批大小，其中 *x* 是批处理中的行数。  
  
 除了指定事务大小之外，批还会影响将行通过网络发送到服务器的时间。 大容量复制函数通常会缓存 **bcp_sendrow** 中的行，直到填充网络数据包，然后将完整数据包发送到服务器。 但是，当应用程序调用 **bcp_batch** 时，当前数据包将发送到服务器，而不考虑它是否已填充。 使用很小的批大小可能导致向服务器发送许多部分填满的数据包，从而降低性能。 例如，在每个 **bcp_sendrow** 之后调用 **bcp_batch** 会导致在单独的数据包中发送每个行，除非行很大，否则会浪费每个数据包中的空间。 SQL Server 的网络数据包的默认大小为 4 KB，但应用程序可以通过调用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 来更改大小，该大小指定 SQL_ATTR_PACKET_SIZE 属性。  
  
 批处理的另一个副作用是将每个批处理视为未完成的结果集，直到它完成 **bcp_batch**。 如果在批处理未完成的情况下尝试在连接句柄上执行任何其他操作， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序将发出错误，其中包含 SQLState = "HY000" 和错误消息字符串：  
  
```  
"[Microsoft][SQL Server Native Client] Connection is busy with  
results for another hstmt."  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
