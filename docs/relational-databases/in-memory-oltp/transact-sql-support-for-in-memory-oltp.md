---
title: Transact-SQL 对内存中 OLTP 的支持 | Microsoft Docs
description: 了解包含用于支持内存中 OLTP 的语法选项的 Transact-SQL 语句。 使用支持的功能的其他参考链接。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: b1cc7c30-1747-4c21-88ac-e95a5e58baac
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dc86572c8a1ee4b8d63e9705a9a5295565c3808
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438716"
---
# <a name="transact-sql-support-for-in-memory-oltp"></a>对内存中 OLTP 的 Transact-SQL 支持
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句包含用于支持内存中 OLTP 的语法选项：  
  
-   [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)（已添加 **MEMORY_OPTIMIZED_DATA**）  
  
-   [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)（已添加 **MEMORY_OPTIMIZED_DATA**）  
  
-   [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
  
-   [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
  
-   [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)  
  
-   [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
    在本机编译的存储过程中，你可以将变量声明为 **NOT NULL**。 但在常规的存储过程中，你不能执行此操作。  
  
 从 SQL Server 2016 开始，内存优化表的 **AUTO_UPDATE_STATISTICS** 可为 **ON**。 有关详细信息，请参阅 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)。  
  
 本机编译的存储过程不支持 [SET STATISTICS XML (Transact-SQL)](../../t-sql/statements/set-statistics-xml-transact-sql.md) ON。  
  
 有关不支持的功能的信息，请参阅[内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)。  
  
 有关本机编译存储过程中支持的构造的信息，请参阅 [本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md) 和 [本机编译的 T-SQL 模块支持的 DDL](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md)。  
  
## <a name="see-also"></a>另请参阅  
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)   
 [本机编译的存储过程的迁移问题](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [内存中 OLTP 不支持的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)   
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
