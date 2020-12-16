---
description: 确定性函数和不确定性函数
title: 确定性函数和不确定性函数 | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- built-in functions [SQL Server]
- nondeterministic functions
- extended stored procedures [SQL Server], functions that call
- deterministic functions
- user-defined functions [SQL Server], deterministic
ms.assetid: 2f3ce5f5-c81c-4470-8141-8144d4f218dd
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eebd2896dc1931e03dd121867ee09c1940d02d36
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466708"
---
# <a name="deterministic-and-nondeterministic-functions"></a>确定性函数和不确定性函数
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  只要使用特定的输入值集并且数据库具有相同的状态，那么不管何时调用，确定性函数始终都会返回相同的结果。 即使访问的数据库的状态不变，每次使用特定的输入值集调用非确定性函数都可能会返回不同的结果。 例如，函数 AVG 对上述给定的限定条件始终返回相同的值，但返回当前 datetime 值的 GETDATE 函数始终会返回不同的结果。  
  
 有多个用户定义函数的属性通过对调用函数的计算列进行索引或通过引用函数的索引视图确定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 为函数的结果建立索引的功能。 函数的确定性是一个属性。 例如，如果一个视图引用了任何不确定性函数，则无法对该视图创建聚集索引。 有关函数属性（包括函数确定性）的详细信息，请参阅 [用户定义函数](../../relational-databases/user-defined-functions/user-defined-functions.md)。  
  
 本主题介绍了内置系统函数的确定性，以及在包含对扩展存储过程的调用时对用户定义函数的确定性的影响。  
  
## <a name="built-in-function-determinism"></a>内置函数的确定性  
 用户无法影响任何内置函数的确定性。 每个内置函数都根据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实现该函数的方式而分为确定性函数或非确定性函数。 例如，在查询中指定 ORDER BY 子句不会更改查询中使用的函数的决定机制。  
  
 除 [FORMAT](../../t-sql/functions/format-transact-sql.md) 外，所有字符串内置函数都是确定性的。 有关这些函数的列表，请参阅[字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)。  
  
 内置函数类别中除字符串函数以外的下列内置函数始终具有确定性。  
  
:::row:::
    :::column:::
        ABS
    :::column-end:::
    :::column:::
        DATEDIFF
    :::column-end:::
    :::column:::
        POWER
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ACOS
    :::column-end:::
    :::column:::
        DAY
    :::column-end:::
    :::column:::
        RADIANS
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ASIN
    :::column-end:::
    :::column:::
        DEGREES
    :::column-end:::
    :::column:::
        ROUND
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ATAN
    :::column-end:::
    :::column:::
        EXP
    :::column-end:::
    :::column:::
        SIGN
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ATN2
    :::column-end:::
    :::column:::
        FLOOR
    :::column-end:::
    :::column:::
        SIN
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CEILING
    :::column-end:::
    :::column:::
        ISNULL
    :::column-end:::
    :::column:::
        SQUARE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        COALESCE
    :::column-end:::
    :::column:::
        ISNUMERIC
    :::column-end:::
    :::column:::
        SQRT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        COS
    :::column-end:::
    :::column:::
        LOG
    :::column-end:::
    :::column:::
        TAN
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        COT
    :::column-end:::
    :::column:::
        LOG10
    :::column-end:::
    :::column:::
        YEAR
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DATALENGTH
    :::column-end:::
    :::column:::
        MONTH
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DATEADD
    :::column-end:::
    :::column:::
        NULLIF
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 
 下列函数并非始终是确定性函数，但是在以确定性方式指定后，可用于索引视图或计算列的索引。  
  
|函数|注释|  
|--------------|--------------|  
|所有聚合函数|除非用 OVER 和 ORDER BY 子句指定聚合函数，否则所有聚合函数都具有确定性。 有关这些函数的列表，请参阅[聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)。|  
|CAST|除非与 **datetime**、 **smalldatetime** 或 **sql_variant** 一起使用，否则其他时候都是确定性的。|  
|CONVERT|除非存在下列条件，否则为确定性函数：<br /><br /> <br /><br /> 源类型是 **sql_variant**。<br /><br /> 目标类型是 **sql_variant** ，其源类型不是确定性的。<br /><br /> 源类型或目标类型是 **datetime** 或 **smalldatetime**，其他源类型或目标类型是字符串，并且指定了不确定的样式。 若要为确定样式，则样式参数必须是常量。 此外，除了样式 20 和 21，小于或等于 100 的样式都具有不确定性。 大于 100 的样式具有确定性，但样式 106、107、109 和 113 除外。|  
|CHECKSUM|确定性函数，CHECKSUM(*) 除外。|  
|ISDATE|只有与 CONVERT 函数一起使用、指定 CONVERT 样式参数并且样式不等于 0、100、9 或 109 时，才是确定性函数。|  
|RAND|仅当指定 *seed* 参数时，RAND 才是确定性函数。|  
  
 所有配置、游标、元数据、安全和系统统计函数都是非确定性函数。 有关这些函数的列表，请参阅[配置函数 (Transact-SQL)](../../t-sql/functions/configuration-functions-transact-sql.md)、[游标函数 (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)、[元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)、[安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md) 和[系统统计函数 (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)。  
  
 其他类别中的下列内置函数始终为非确定性函数。  
  
:::row:::
    :::column:::
        @@CONNECTIONS
    :::column-end:::
    :::column:::
        GETDATE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@CPU_BUSY
    :::column-end:::
    :::column:::
        GETUTCDATE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@DBTS
    :::column-end:::
    :::column:::
        GET_TRANSMISSION_STATUS
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@IDLE
    :::column-end:::
    :::column:::
        LAG
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@IO_BUSY
    :::column-end:::
    :::column:::
        LAST_VALUE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@MAX_CONNECTIONS
    :::column-end:::
    :::column:::
        LEAD
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@PACK_RECEIVED
    :::column-end:::
    :::column:::
        MIN_ACTIVE_ROWVERSION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@PACK_SENT
    :::column-end:::
    :::column:::
        NEWID
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@PACKET_ERRORS
    :::column-end:::
    :::column:::
        NEWSEQUENTIALID
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@TIMETICKS
    :::column-end:::
    :::column:::
        NEXT VALUE FOR
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@TOTAL_ERRORS
    :::column-end:::
    :::column:::
        NTILE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@TOTAL_READ
    :::column-end:::
    :::column:::
        PARSENAME
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        @@TOTAL_WRITE
    :::column-end:::
    :::column:::
        PERCENTILE_CONT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        AT TIME ZONE
    :::column-end:::
    :::column:::
        PERCENTILE_DISC
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        CUME_DIST
    :::column-end:::
    :::column:::
        PERCENT_RANK
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CURRENT_TIMESTAMP
    :::column-end:::
    :::column:::
        RAND
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DENSE_RANK
    :::column-end:::
    :::column:::
        RANK
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        FIRST_VALUE
    :::column-end:::
    :::column:::
        ROW_NUMBER
    :::column-end:::
:::row-end:::   
:::row:::
    :::column:::
        FORMAT
    :::column-end:::
    :::column:::
        TEXTPTR
    :::column-end:::
:::row-end:::
 
## <a name="calling-extended-stored-procedures-from-functions"></a>从函数中调用扩展存储过程  
 由于扩展存储过程会对数据库产生副面影响，因此调用扩展存储过程的函数为不确定性函数。 负面影响为对数据库全局状态的更改，如更新表、更新文件或网络等外部资源；例如，修改文件或发送电子邮件。 从用户定义函数中执行扩展存储过程时，不要依赖于返回一致的结果集。 建议不要使用对数据库产生负面影响的用户定义函数。  
  
 从函数内部调用扩展存储过程时，该扩展存储过程不能向客户端返回结果集。 任何向客户端返回结果集的开放式数据服务 API 都具有返回代码 FAIL。  
  
 扩展存储过程可以连接回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 但是，该过程不能与调用扩展存储过程的原始函数联接同一事务。  
  
 类似于从批处理或存储过程中调用，扩展存储过程在运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 安全帐户的上下文中执行。 扩展存储过程的所有者在授予其他用户执行该过程的权限时，应该考虑此安全性上下文的权限。  
  
  
