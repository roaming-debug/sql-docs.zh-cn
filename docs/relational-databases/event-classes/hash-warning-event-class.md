---
description: Hash Warning 事件类
title: Hash Warning 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Hash Warning event class
ms.assetid: cb93c620-4be9-4362-8bf0-af3f2048bdaf
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec0ced044ba6405d36f53bed4a42f17b57e7d342
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485419"
---
# <a name="hash-warning-event-class"></a>Hash Warning 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Hash Warning 事件类可用于监视在哈希操作过程中何时发生哈希递归或哈希终止（哈希释放）。  
  
 当生成输入无法装入可用内存时，会发生哈希递归，这将导致输入分割成单独处理的多个分区。 如果这些分区中任何一个仍然大于可用内存，则该分区再拆分成子分区分别进行处理。 此拆分过程将一直继续到每个分区都小于可用内存，或达到最大递归级数（显示在 IntegerData 数据列）。  
  
 当哈希操作达到其最大递归级数并转换到替换计划以处理剩余的分区数据时发生哈希释放。 哈希释放通常是由于偏斜数据造成的。  
  
 哈希递归和哈希释放会导致服务器的性能降低。 若要消除或降低哈希递归和哈希释放的频率，请执行下列操作之一：  
  
-   确保正在联接或分组的列上存在统计信息。  
  
-   如果这些列上存在统计信息，请更新它们。  
  
-   使用其他类型的联接。 例如，使用 MERGE 或 LOOP 联接（如果适合）。  
  
-   增加计算机上的可用内存。 当没有足够的内存就地处理查询，因此这些查询需要溢出到磁盘时，会发生哈希递归或哈希释放。  
  
 创建或更新联接涉及的列上的统计信息是减少发生哈希递归或哈希释放次数最有效的方法。  
  
> [!NOTE]  
>  另外，“Grace 哈希联接”和“递归哈希联接”两词也用于描述哈希释放。  
  
> [!IMPORTANT]  
>  若要确定查询优化器生成执行计划时 Hash Warning 事件发生的位置，还应在跟踪中收集一个 Showplan 事件类。 可以选择除了 Showplan Text 和 Showplan Text (Unencoded) 事件类（不会返回节点 ID）以外的任何 Showplan 事件类。 Showplan 中的节点 ID 标识查询优化器在生成查询执行计划时执行的每个运算。 这些运算称为“运算符” ，Showplan 中的每个运算符都有一个节点 ID。 Hash Warning 事件的 ObjectID 列与 Showplan 中的节点 ID 对应，因此可确定哪个运算符或运算导致错误。  
  
## <a name="hash-warning-event-class-data-columns"></a>Hash Warning 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 该列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|EventClass|**int**|事件类型 = 55。|27|否|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 0 = 递归<br /><br /> 1 = 释放|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IntegerData|**int**|递归级数（仅限于哈希递归）。|25|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginName|**nvarchar**|用户的登录名（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为 \<DOMAIN>\\<username\>）。|11|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ObjectID|**int**|重新分区所涉及的哈希组的根节点 ID。 与 Showplan 中的节点 ID 对应。|22|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26||  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|XactSequence|**bigint**|用于说明当前事务的标记。|50|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)    
 [联接](../../relational-databases/performance/joins.md)    
  
  
