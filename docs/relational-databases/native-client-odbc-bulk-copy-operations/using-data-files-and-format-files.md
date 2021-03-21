---
description: 使用数据文件和格式化文件
title: 使用数据文件和格式化文件 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- ODBC, functions
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [SQL Server Native Client]
- ODBC, bulk copy operations
- bulk copy [ODBC], data files
ms.assetid: c01b7155-3f0a-473d-90b7-87a97bc56ca5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9ca6f51e9d81f3b2f40b2d4c049ba100667dfd4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746107"
---
# <a name="using-data-files-and-format-files"></a>使用数据文件和格式化文件
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  最简单的大容量复制程序执行以下操作：  
  
1.  调用 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) 指定大容量复制 (将 BCP_OUT 从表或视图) 设置为数据文件。  
  
2.  调用 [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) 执行大容量复制操作。  
  
 数据文件以本机模式创建；因此，来自表或视图中所有列的数据都采用与数据库中相同的格式存储于数据文件中。 然后，通过使用与上述相同的步骤并设置 DB_IN（而非 DB_OUT），将该文件大容量复制到某一服务器中。 只有在源表和目标表都具有完全相同的结构时，这一功能才适用。 生成的数据文件也可以通过使用 **/n** (本机模式) 开关输入到 **bcp** 实用工具中。  
  
 若要大容量复制出 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的结果集，而非直接从表或视图复制：  
  
1.  调用 **bcp_init** 以指定大容量复制，但为表名称指定 NULL。  
  
2.  调用 [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) 将 *EOPTION* 设置为 BCPHINTS，并将 *IValue* 设置为指向包含 transact-sql 语句的 SQLTCHAR 字符串的指针。  
  
3.  调用 **bcp_exec** 执行大容量复制操作。  

 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可以是生成某一结果集的任何语句。 将创建包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的第一个结果集的数据文件。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句生成多个结果集，则大容量复制将忽略第一个结果集之后的所有结果集。  
  
 若要创建一个数据文件，其中的列数据与表中的格式不同，请调用 [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) 以指定将更改的列数，然后为要更改其格式的每个列调用 [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) 。 这是在调用 **bcp_init** 之后、调用 **bcp_exec** 之前完成的。 **bcp_colfmt** 指定列数据在数据文件中的存储格式。 当大容量复制时，可以使用此方法。你还可以使用 **bcp_colfmt** 来设置行终止符和列终止符。 例如，如果您的数据中不包含制表符，则可以通过使用 **bcp_colfmt** 将制表符设置为每列的终止符来创建制表符分隔文件。  
  
 大容量复制和使用 **bcp_colfmt** 时，可以轻松地创建一个格式文件，用于描述你通过在上一次调用 **bcp_colfmt** 后调用 [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md)创建的数据文件。  
  
 从格式化文件描述的数据文件中大容量复制时，请通过在 **bcp_init** 之后调用 [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) ，然后在 **bcp_exec** 之前读取格式化文件。  
  
 **Bcp_control** 函数在从数据文件大容量复制到时控制多个选项 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 **bcp_control** 设置选项，如终止前的最大错误数、要开始大容量复制的文件中的行、要停止的行以及批大小。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
