---
title: 性能改进 - 内存中 OLTP
description: 此代码示例演示了使用解释型 Transact-SQL 和本机编译的存储过程实现内存优化表的快速性能。
ms.custom: seo-dt-2019
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c6def45d-d2d4-4d24-8068-fab4cd94d8cc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 22a4e3b5b1ee18fd70ba62c813aa6412bdbd9383
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481268"
---
# <a name="demonstration-performance-improvement-of-in-memory-oltp"></a>演示：内存 OLTP 的性能改进
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题中的代码示例演示了内存优化表的快速性能。 从传统、已解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)]访问内存优化表中的数据时，此性能改进非常明显。 从本机编译存储过程 (NCSProc) 访问内存优化表中的数据时，此性能改进甚至更明显。  
 
若要查看有关内存中 OLTP 潜在性能改进的更全面的演示，请参阅 [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)（内存中 OLTP 性能演示 v1.0）。 
  
 本文中的代码示例是单线程的，未能利用内存中 OLTP 的并发利益。 使用并发的工作负荷将带来更高的性能提升。 此代码示例仅显示性能改进的一个方面，即提高了针对插入的数据访问效率。  
  
 从 NCSProc 访问内存优化表中的数据时，将完全实现内存优化表提供的性能改进。  
  
## <a name="code-example"></a>代码示例  
 以下各小节介绍了每个步骤。  
  
### <a name="step-1a-prerequisite-if-using-ssnoversion"></a>步骤 1a：使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时的先决条件  
 此第一个小节中的步骤仅适用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中运行的情况，而不适用于在 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中运行的情况。 请执行以下操作：  
  
1.  使用 SQL Server Management Studio (SSMS.exe) 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 或也可使用任何类似于 SSMS.exe 的工具。  
  
2.  手动创建一个名为 **C:\data\\** 的目录。 示例 Transact-SQL 代码预期目录已预先存在。  
  
3.  运行短 T-SQL 以创建数据库及其内存优化的文件组。  
  
```sql  
go  
CREATE DATABASE imoltp;    --  Transact-SQL  
go  
  
ALTER DATABASE imoltp ADD FILEGROUP [imoltp_mod]  
    CONTAINS MEMORY_OPTIMIZED_DATA;  
  
ALTER DATABASE imoltp ADD FILE  
    (name = [imoltp_dir], filename= 'c:\data\imoltp_dir')  
    TO FILEGROUP imoltp_mod;  
go  
  
USE imoltp;  
go  
```  
  
### <a name="step-1b-prerequisite-if-using-sssdsfull"></a>步骤 1b：使用 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 时的先决条件  
 本小节仅适用于使用 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]的情况。 请执行以下操作：  
  
1.  决定将用于代码示例的现有测试数据库。  
  
2.  如果你决定创建一个新的测试数据库，请使用 [Azure 门户](https://portal.azure.com) 创建一个名为 **imoltp** 的数据库。  
  
 如果需要有关使用 Azure 门户来实现这一操作的说明，请参阅 [Azure SQL Database 入门](/azure/azure-sql/database/single-database-create-quickstart)。  
  
### <a name="step-2-create-memory-optimized-tables-and-ncsproc"></a>步骤 2：创建内存优化表和 NCSProc  
 此步骤创建内存优化表和本机编译的存储过程 (NCSProc)。 请执行以下操作：  
  
1.  使用 SSMS.exe 连接到新的数据库。  
  
2.  在数据库中运行以下 T-SQL。  
  
```sql  
go  
DROP PROCEDURE IF EXISTS ncsp;  
DROP TABLE IF EXISTS sql;  
DROP TABLE IF EXISTS hash_i;  
DROP TABLE IF EXISTS hash_c;  
go  
  
CREATE TABLE [dbo].[sql] (  
  c1 INT NOT NULL PRIMARY KEY,  
  c2 NCHAR(48) NOT NULL  
);  
go  
  
CREATE TABLE [dbo].[hash_i] (  
  c1 INT NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000),  
  c2 NCHAR(48) NOT NULL  
) WITH (MEMORY_OPTIMIZED=ON, DURABILITY = SCHEMA_AND_DATA);  
go  
  
CREATE TABLE [dbo].[hash_c] (  
  c1 INT NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000),  
  c2 NCHAR(48) NOT NULL  
) WITH (MEMORY_OPTIMIZED=ON, DURABILITY = SCHEMA_AND_DATA);  
go  
  
CREATE PROCEDURE ncsp  
    @rowcount INT,  
    @c NCHAR(48)  
  WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
  AS   
  BEGIN ATOMIC   
  WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
  DECLARE @i INT = 1;  
  WHILE @i <= @rowcount  
  BEGIN;  
    INSERT INTO [dbo].[hash_c] VALUES (@i, @c);  
    SET @i += 1;  
  END;  
END;  
go  
```  
  
### <a name="step-3-run-the-code"></a>步骤 3：运行代码  
 现在，你可以执行将演示内存优化表性能的查询。 请执行以下操作：  
  
1.  使用 SSMS.exe 在数据库中运行以下 T-SQL。  
  
     忽略此首次运行生成的任何速度数据或其他性能数据。 第一次运行确保执行几个仅完成一次的操作，如内存的初始分配。  
  
2.  同样，使用 SSMS.exe 在数据库中重新运行以下 T-SQL。  
  
```sql  
go  
SET STATISTICS TIME OFF;  
SET NOCOUNT ON;  
  
-- Inserts, one at a time.  
  
DECLARE @starttime DATETIME2 = sysdatetime();  
DECLARE @timems INT;  
DECLARE @i INT = 1;  
DECLARE @rowcount INT = 100000;  
DECLARE @c NCHAR(48) = N'12345678901234567890123456789012345678';  
  
-- Harddrive-based table and interpreted Transact-SQL.  
  
BEGIN TRAN;  
  WHILE @i <= @rowcount  
  BEGIN;  
    INSERT INTO [dbo].[sql] VALUES (@i, @c);  
    SET @i += 1;  
  END;  
COMMIT;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'A: Disk-based table and interpreted Transact-SQL: '  
    + cast(@timems AS VARCHAR(10)) + ' ms';  
  
-- Interop Hash.  
  
SET @i = 1;  
SET @starttime = sysdatetime();  
  
BEGIN TRAN;  
  WHILE @i <= @rowcount  
    BEGIN;  
      INSERT INTO [dbo].[hash_i] VALUES (@i, @c);  
      SET @i += 1;  
    END;  
COMMIT;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'B: memory-optimized table with hash index and interpreted Transact-SQL: '  
    + cast(@timems as VARCHAR(10)) + ' ms';  
  
-- Compiled Hash.  
  
SET @starttime = sysdatetime();  
  
EXECUTE ncsp @rowcount, @c;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'C: memory-optimized table with hash index and native SP:'  
    + cast(@timems as varchar(10)) + ' ms';  
go  
  
DELETE sql;  
DELETE hash_i;  
DELETE hash_c;  
go  
```  
  
 接下来是我们的第二次测试运行所生成的输出时间统计信息。  
  
```sql  
10453 ms , A: Disk-based table and interpreted Transact-SQL.  
5626 ms , B: memory-optimized table with hash index and interpreted Transact-SQL.  
3937 ms , C: memory-optimized table with hash index and native SP.  
```  
  
## <a name="see-also"></a>另请参阅  
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
