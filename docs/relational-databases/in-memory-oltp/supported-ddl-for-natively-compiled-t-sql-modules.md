---
title: 本机编译的 T-SQL 模块支持的 DDL | Microsoft Docs
description: 了解本机编译的 T-SQL 模块支持的 DDL，例如存储过程、标量 UDF、内联 TVF 和触发器。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6b21f47e-bceb-4054-8b3c-9d39bb9583c0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dbce86c0b46e1d256934b79765372978221dd41
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485139"
---
# <a name="supported-ddl-for-natively-compiled-t-sql-modules"></a>对于本机编译的 T-SQL 模块支持的 DDL
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题列出了对于本机编译的 T-SQL 模块支持的 DDL，例如存储过程、标量 UDF、内联 TVF 和触发器。  
  
 有关功能和可用作本机编译的 T-SQL 模块一部分的 T-SQL 外围应用的信息，请参阅 [本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。  
  
 有关不支持的构造的信息，请参阅 [内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)。  
  
 支持以下各项：  
  
-   [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
-   [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
-   [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md) 和 INSERT SELECT 语句  
  
-   SCHEMABINDING 和 BEGIN ATOMIC（对于本机编译存储过程是必需的）  
  
     有关详细信息，请参阅 [Creating Natively Compiled Stored Procedures](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md)。  
  
-   NATIVE_COMPILATION  
  
     有关详细信息，请参阅 [Native Compilation of Tables and Stored Procedures](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md)。  
  
-   可以将参数和变量声明为 NOT NULL（仅适用于本机编译模块：本机编译存储过程和本机编译标量用户定义函数）。  
  
-   表值参数。  
  
     有关详细信息，请参阅[使用表值参数（数据引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)。  
  
-   EXECUTE AS OWNER、SELF、CALLER 和用户。  
  
-   表和过程的 GRANT 和 DENY 权限。  
  
     有关详细信息，请参阅 [GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md) 和 [DENY 对象权限 (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
