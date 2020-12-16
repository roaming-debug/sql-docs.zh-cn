---
title: ATOMIC 块 | Microsoft Docs
description: 了解 BEGIN ATOMIC，它属于 ANSI SQL 标准。 SQL Server 支持本机过程中的 ATOMIC 块。
ms.custom: ''
ms.date: 10/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 40e0e749-260c-4cfc-a848-444d30c09d85
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7e5de0ea599650792feef01508c1eed54aed68c6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465428"
---
# <a name="atomic-blocks-in-native-procedures"></a>本机过程中的 ATOMIC 块
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  **BEGIN ATOMIC** 属于 ANSI SQL 标准。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持顶级本机编译存储过程的 ATOMIC 块，以及本机编译的标量用户定义函数。 有关这些函数的详细信息，请参阅 [内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
  
-   每个本机编译的存储过程都恰好包含一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句块。 这个语句块是原子块。  
  
-   非本机的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程和即席批处理不支持原子块。  
  
 原子块在事务内执行（以原子方式）。 或者块中的整个语句都成功，或者整个块都将回滚到在块的开始处创建的保存点。 此外，会话设置对于原子块而言是固定的。 以不同设置在会话中执行相同的原子块将导致相同的行为，与当前会话的设置无关。  
  
## <a name="transactions-and-error-handling"></a>事务和错误处理  
 如果某个事务已在会话上存在（因为某一批处理执行了 **BEGIN TRANSACTION** 语句并且该事务保持活动状态），则开始一个原子块将在该事务中创建一个保存点。 如果该原子块退出且没有异常，将创建用于块提交的保存点，但在该事务在会话级别提交前该事务将不提交。 如果该块引发一个异常，则该块的影响将被回滚，但处于会话级别的事务将继续，除非该异常是注定事务终止的。 例如，写入冲突是注定事务终止的，但不是类型转换错误。  
  
 如果在会话上没有处于活动状态的事务，则 **BEGIN ATOMIC** 将开始一个新事务。 如果在该块的作用域外未引发异常，则在该块的末尾将提交该事务。 如果该块引发异常（也就是说，未在该块内捕获和处理异常），则该事务将被回滚。 对于跨单个原子块的事务（单个本机编译的存储过程），你无需编写显式的 **BEGIN TRANSACTION** 和 **COMMIT** 或 **ROLLBACK** 语句。  
  
 本机编译的存储过程支持使用 **TRY**、 **CATCH** 和 **THROW** 构造进行错误处理。 不支持 **RAISERROR** 。  
  
 下面的示例说明针对原子块和本机编译存储过程的错误处理行为：  
  
```sql  
-- sample table  
CREATE TABLE dbo.t1 (  
  c1 int not null primary key nonclustered  
)  
WITH (MEMORY_OPTIMIZED=ON)  
GO  
  
-- sample proc that inserts 2 rows  
CREATE PROCEDURE dbo.usp_t1 @v1 bigint not null, @v2 bigint not null  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS  
BEGIN ATOMIC  
WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english', DELAYED_DURABILITY = ON)  
  
  INSERT dbo.t1 VALUES (@v1)  
  INSERT dbo.t1 VALUES (@v2)  
  
END  
GO  
  
-- insert two rows  
EXEC dbo.usp_t1 1, 2  
GO  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify the rows 1 and 2 were committed  
SELECT c1 FROM dbo.t1  
GO  
  
-- execute proc with arithmetic overflow  
EXEC dbo.usp_t1 3, 4444444444444  
GO  
-- expected error message:  
-- Msg 8115, Level 16, State 0, Procedure usp_t1  
-- Arithmetic overflow error converting bigint to data type int.  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify rows 3 was not committed; usp_t1 has been rolled back  
SELECT c1 FROM dbo.t1  
GO  
  
-- start a new transaction  
BEGIN TRANSACTION  
  -- insert rows 3 and 4  
  EXEC dbo.usp_t1 3, 4  
  
  -- verify there is still an active transaction  
  SELECT @@TRANCOUNT  
  
  -- verify the rows 3 and 4 were inserted  
  SELECT c1 FROM dbo.t1 WITH (SNAPSHOT)   
  ORDER BY c1  
  
  -- catch the arithmetic overflow error  
  BEGIN TRY  
    EXEC dbo.usp_t1 5, 4444444444444  
  END TRY  
  BEGIN CATCH  
    PRINT N'Error occurred: ' + error_message()  
  END CATCH  
  
  -- verify there is still an active transaction  
  SELECT @@TRANCOUNT  
  
  -- verify rows 3 and 4 are still in the table, and row 5 has not been inserted  
  SELECT c1 FROM dbo.t1 WITH (SNAPSHOT)   
  ORDER BY c1  
  
COMMIT  
GO  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify rows 3 and 4 has been committed  
SELECT c1 FROM dbo.t1  
ORDER BY c1  
GO  
```  
  
 以下特定于内存优化表的错误消息是注定事务终止的。 如果它们在 ATOMIC 块的作用域中发生，将导致中止事务：10772、41301、41302、41305、41325、41332、41333 和 41839。  
  
## <a name="session-settings"></a>会话设置  
 在编译存储过程时原子块中的会话设置是固定的。 某些设置可使用 **BEGIN ATOMIC** 指定，而其他设置则始终固定为相同值。  
  
 以下选项对于 **BEGIN ATOMIC** 而言是必需的：  
  
|必需设置|说明|  
|----------------------|-----------------|  
|**TRANSACTION ISOLATION LEVEL**|支持的值为 **SNAPSHOT**、 **REPEATABLEREAD** 和 **SERIALIZABLE**。|  
|**LANGUAGE**|确定日期和时间格式以及系统消息。 支持 [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) 中的所有语言和别名。|  
  
 以下设置是可选的：  
  
|可选设置|说明|  
|----------------------|-----------------|  
|**DATEFORMAT**|支持所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期格式。 指定后， **DATEFORMAT** 将取代与 **LANGUAGE** 相关联的默认日期格式。|  
|**DATEFIRST**|指定后， **DATEFIRST** 将取代与 **LANGUAGE** 相关联的默认设置。|  
|**DELAYED_DURABILITY**|支持的值为 **OFF** 和 **ON**。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务提交可以是完全持久、默认或延迟的持久。有关详细信息，请参阅[控制事务持久性](../../relational-databases/logs/control-transaction-durability.md)。|  
  
 下面的 SET 选项对于所有本机编译存储过程中的所有原子块具有相同的系统默认值：  
  
|Set 选项|原子块的系统默认值|  
|----------------|--------------------------------------|  
|ANSI_NULLS|ON|  
|ANSI_PADDING|ON|  
|ANSI_WARNING|ON|  
|ARITHABORT|ON|  
|ARITHIGNORE|OFF|  
|CONCAT_NULL_YIELDS_NULL|ON|  
|IDENTITY_INSERT|OFF|  
|NOCOUNT|ON|  
|NUMERIC_ROUNDABORT|OFF|  
|QUOTED_IDENTIFIER|ON|  
|ROWCOUNT|0|  
|TEXTSIZE|0|  
|XACT_ABORT|OFF<br /><br /> 未捕获的异常导致原子块回滚，但不会导致事务中止，除非错误是注定事务终止的。|  
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
