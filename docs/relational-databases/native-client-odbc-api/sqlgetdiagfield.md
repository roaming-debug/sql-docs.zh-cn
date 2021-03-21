---
description: SQLGetDiagField
title: SQLGetDiagField |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetDiagField function
ms.assetid: 395245ba-0372-43ec-b9a4-a29410d85a6d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2aa29dda546e709b06b7a87300ffe66e5505ffd5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754147"
---
# <a name="sqlgetdiagfield"></a>SQLGetDiagField
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序为 **SQLGetDiagField** 指定以下附加诊断字段。 这些字段支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序的大量错误报告功能，并且可以在连接的 ODBC 连接句柄和 ODBC 语句句柄上生成的所有诊断记录中使用。 这些字段在 sqlncli.h 中定义。  
  
|诊断记录字段|说明|  
|------------------------------|-----------------|  
|SQL_DIAG_SS_LINE|报告生成错误的存储过程的行号。 只有 SQL_DIAG_SS_PROCNAME 返回值时，SQL_DIAG_SS_LINE 的值才有意义。 该值作为无符号 16 位整数返回。|  
|SQL_DIAG_SS_MSGSTATE|错误消息的状态。 有关错误消息状态的信息，请参阅 [RAISERROR](../../t-sql/language-elements/raiserror-transact-sql.md)。 该值作为有符号 32 位整数返回。|  
|SQL_DIAG_SS_PROCNAME|根据需要生成错误的存储过程的名称。 该值作为字符串返回。 字符串的长度（以字符为单位）取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本。 可以通过调用 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) 请求 SQL_MAX_PROCEDURE_NAME_LEN 的值来确定此值。|  
|SQL_DIAG_SS_SEVERITY|关联错误消息的严重级别。 该值作为有符号 32 位整数返回。|  
|SQL_DIAG_SS_SRVNAME|发生错误的服务器的名称。 该值作为字符串返回。 字符串的长度（以字符为单位）由 sqlncli.h 中的 SQL_MAX_SQLSERVERNAME 宏定义。|  
  
 包含字符数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特定诊断字段（即 SQL_DIAG_SS_PROCNAME 和 SQL_DIAG_SS_SRVNAME）将该数据作为以 Null 值结束的 ANSI 或 Unicode 字符串返回给客户端。 如有必要，应根据字符宽度调整字符计数。 或者，还可以使用可移植 C 数据类型（如 TCHAR 或 SQLTCHAR）确保正确的程序可变长度。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序报告标识最后尝试的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句的以下额外动态函数代码。 动态函数代码在诊断记录集的标头（记录 0）中返回，因此可在每次执行时（成功或失败）使用。  
  
|动态函数代码|源|  
|---------------------------|------------|  
|SQL_DIAG_DFC_SS_ALTER_DATABASE|ALTER DATABASE 语句|  
|SQL_DIAG_DFC_SS_CHECKPOINT|CHECKPOINT 语句|  
|SQL_DIAG_DFC_SS_CONDITION|在语句的 WHERE 或 HAVING 子句中引发的错误。|  
|SQL_DIAG_DFC_SS_CREATE_DATABASE|CREATE DATABASE 语句|  
|SQL_DIAG_DFC_SS_CREATE_DEFAULT|CREATE DEFAULT 语句|  
|SQL_DIAG_DFC_SS_CREATE_PROCEDURE|CREATE PROCEDURE 语句|  
|SQL_DIAG_DFC_SS_CREATE_RULE|CREATE RULE 语句|  
|SQL_DIAG_DFC_SS_CREATE_TRIGGER|CREATE TRIGGER 语句|  
|SQL_DIAG_DFC_SS_CURSOR_DECLARE|DECLARE CURSOR 语句|  
|SQL_DIAG_DFC_SS_CURSOR_OPEN|OPEN 语句|  
|SQL_DIAG_DFC_SS_CURSOR_FETCH|FETCH 语句|  
|SQL_DIAG_DFC_SS_CURSOR_CLOSE|CLOSE 语句|  
|SQL_DIAG_DFC_SS_DEALLOCATE_CURSOR|DEALLOCATE 语句|  
|SQL_DIAG_DFC_SS_DBCC|DBCC 语句|  
|SQL_DIAG_DFC_SS_DENY|DENY 语句|  
|SQL_DIAG_DFC_SS_DROP_DATABASE|DROP DATABASE 语句|  
|SQL_DIAG_DFC_SS_DROP_DEFAULT|DROP DEFAULT 语句|  
|SQL_DIAG_DFC_SS_DROP_PROCEDURE|DROP PROCEDURE 语句|  
|SQL_DIAG_DFC_SS_DROP_RULE|DROP RULE 语句|  
|SQL_DIAG_DFC_SS_DROP_TRIGGER|DROP TRIGGER 语句|  
|SQL_DIAG_DFC_SS_DUMP_DATABASE|BACKUP 或 DUMP DATABASE 语句|  
|SQL_DIAG_DFC_SS_DUMP_TABLE|DUMP TABLE 语句|  
|SQL_DIAG_DFC_SS_DUMP_TRANSACTION|BACKUP 或 DUMP TRANSACTION 语句。 如果 **chkpt. 上的 trunc** ，则还会返回 CHECKPOINT 语句的。 数据库选项为 on。|  
|SQL_DIAG_DFC_SS_GOTO|GOTO 控制流语句|  
|SQL_DIAG_DFC_SS_INSERT_BULK|INSERT BULK 语句|  
|SQL_DIAG_DFC_SS_KILL|KILL 语句|  
|SQL_DIAG_DFC_SS_LOAD_DATABASE|LOAD 或 RESTORE DATABASE 语句|  
|SQL_DIAG_DFC_SS_LOAD_HEADERONLY|LOAD 或 RESTORE HEADERONLY 语句|  
|SQL_DIAG_DFC_SS_LOAD_TABLE|LOAD TABLE 语句|  
|SQL_DIAG_DFC_SS_LOAD_TRANSACTION|LOAD 或 RESTORE TRANSACTION 语句|  
|SQL_DIAG_DFC_SS_PRINT|PRINT 语句|  
|SQL_DIAG_DFC_SS_RAISERROR|RAISERROR 语句|  
|SQL_DIAG_DFC_SS_READTEXT|READTEXT 语句|  
|SQL_DIAG_DFC_SS_RECONFIGURE|RECONFIGURE 语句|  
|SQL_DIAG_DFC_SS_RETURN|RETURN 控制流语句|  
|SQL_DIAG_DFC_SS_SELECT_INTO|SELECT INTO 语句|  
|SQL_DIAG_DFC_SS_SET|SET 语句（常规，所有选项）|  
|SQL_DIAG_DFC_SS_SET_IDENTITY_INSERT|SET IDENTITY_INSERT 语句|  
|SQL_DIAG_DFC_SS_SET_ROW_COUNT|SET ROWCOUNT 语句|  
|SQL_DIAG_DFC_SS_SET_STATISTICS|SET STATISTICS IO 或 SET STATISTICS TIME 语句|  
|SQL_DIAG_DFC_SS_SET_TEXTSIZE|SET TEXTSIZE 语句|  
|SQL_DIAG_DFC_SS_SETUSER|SETUSER 语句|  
|SQL_DIAG_DFC_SS_SET_XCTLVL|SET TRANSACTION ISOLATION LEVEL 语句|  
|SQL_DIAG_DFC_SS_SHUTDOWN|SHUTDOWN 语句|  
|SQL_DIAG_DFC_SS_TRANS_BEGIN|BEGIN TRAN 语句|  
|SQL_DIAG_DFC_SS_TRANS_COMMIT|COMMIT TRAN 语句|  
|SQL_DIAG_DFC_SS_TRANS_PREPARE|准备提交分布式事务|  
|SQL_DIAG_DFC_SS_TRANS_ROLLBACK|ROLLBACK TRAN 语句|  
|SQL_DIAG_DFC_SS_TRANS_SAVE|SAVE TRAN 语句|  
|SQL_DIAG_DFC_SS_TRUNCATE_TABLE|TRUNCATE TABLE 语句|  
|SQL_DIAG_DFC_SS_UPDATE_STATISTICS|UPDATE STATISTICS 语句|  
|SQL_DIAG_DFC_SS_UPDATETEXT|UPDATETEXT 语句|  
|SQL_DIAG_DFC_SS_USE|USE 语句|  
|SQL_DIAG_DFC_SS_WAITFOR|WAITFOR 控制流语句|  
|SQL_DIAG_DFC_SS_WRITETEXT|WRITETEXT 语句|  
  
## <a name="sqlgetdiagfield-and-table-valued-parameters"></a>SQLGetDiagField 和表值参数  
 SQLGetDiagField 可用于检索两个诊断字段： SQL_DIAG_SS_TABLE_COLUMN_NUMBER 和 SQL_DIAG_SS_TABLE_ROW_NUMBER。 这些字段可帮助您确定哪个值导致了与诊断记录关联的错误或警告。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLGetDiagField 函数](../../odbc/reference/syntax/sqlgetdiagfield-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
