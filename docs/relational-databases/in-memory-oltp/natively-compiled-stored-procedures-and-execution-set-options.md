---
title: 本机编译存储过程和设置选项
description: 会话的 SET 选项不影响存储过程的执行，除非某些 SET 选项导致存储过程无法运行。
ms.custom: seo-dt-2019
ms.date: 10/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c1869cf7-9030-4d18-85d6-0e419a4e9af7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e86ab52348e4e51a2b060bab50aecf2d3c453fc3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485279"
---
# <a name="natively-compiled-stored-procedures-and-execution-set-options"></a>本机编译存储过程和执行的 Set 选项
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如 [ATOMIC 块](atomic-blocks-in-native-procedures.md)中所述，ATOMIC 块中修复了“会话”选项。 由于 ATOMIC 块为必需，存储过程的执行不会受到会话的 SET 选项的影响。 但是，某些 SET 选项（例如 SET NOEXEC 和 SET SHOWPLAN_XML）会导致存储过程（包括本机编译存储过程）不执行。   
  
 在任意 STATISTICS 选项开启的情况下执行本机编译的存储过程时，系统将该过程作为整体（而非针对每条语句）收集统计数据。 有关详细信息，请参阅 [SET STATISTICS IO (Transact-SQL)](../../t-sql/statements/set-statistics-io-transact-sql.md)、[SET STATISTICS PROFILE (Transact-SQL)](../../t-sql/statements/set-statistics-profile-transact-sql.md)、[SET STATISTICS TIME (Transact-SQL)](../../t-sql/statements/set-statistics-time-transact-sql.md) 和 [SET STATISTICS XML (Transact-SQL)](../../t-sql/statements/set-statistics-xml-transact-sql.md)。 若要在本机编译的存储过程中获取针对每条语句的执行统计信息，请使用 sp_statement_completed 事件上的扩展事件会话，该会话将会在某一存储过程执行中的每个单独查询完成时启动。 有关创建扩展事件会话的详细信息，请参阅 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)。  
  
 本机编译的存储过程支持 **SHOWPLAN_XML** 。 本机编译的存储过程不支持 **SHOWPLAN_ALL** 和 **SHOWPLAN_TEXT** 。  
  
 本机编译的存储过程不支持 **SET FMTONLY** 。 请改为使用 [sp_describe_first_result_set (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
