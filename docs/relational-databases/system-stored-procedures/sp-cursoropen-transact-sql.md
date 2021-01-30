---
description: sp_cursoropen (Transact-SQL)
title: sp_cursoropen (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoropen
- sp_cursoropen_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoropen
ms.assetid: 16462ede-4393-4293-a598-ca88c48ca70b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1e8e4025a31c92ec001bcb45bee4f8d26f3dc24e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206281"
---
# <a name="sp_cursoropen-transact-sql"></a>sp_cursoropen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  打开游标。 sp_cursoropen 定义与游标和游标选项相关联的 SQL 语句，然后填充游标。 sp_cursoropen 等效于 [!INCLUDE[tsql](../../includes/tsql-md.md)] DECLARE_CURSOR 和 OPEN 语句的组合。 此过程通过在表格格式数据流 (TDS) 数据包中指定 ID = 2 来调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursoropen cursor OUTPUT, stmt  
    [, scrollopt[ OUTPUT ] [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,boundparam][,...n]]] ]]  
```  
  
## <a name="arguments"></a>参数  
 *cursor*  
 SQL Server 生成的游标标识符。 *cursor* 是一个 *句柄* 值，必须对涉及游标的所有后续过程（如 sp_cursorfetch）提供此值。 *cursor* 是带有 **int** 返回值的必需参数。  
  
 *游标* 允许单个数据库连接上的多个游标处于活动状态。  
  
 *stmt*  
 定义游标结果集的必需参数。 任何有效的查询字符串 (语法和绑定) 任何字符串类型 (无论 Unicode、大小等，) 都可以充当有效的 *stmt* 值类型。  
  
 *scrollopt*  
 滚动选项。 *scrollopt* 是一个可选参数，它需要以下 **整数** 输入值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 因为请求的值可能不适合于 *stmt* 定义的游标，所以，此参数可同时用作输入和输出。 在此类情况下，SQL Server 分配一个适当的值。  
  
 *ccopt*  
 并发控制选项。 *ccopt* 是一个可选参数，它需要以下 **整数** 输入值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS（以前称为 LOCKCC）|  
|0x0004|**乐观** (以前称为 OPTCC) |  
|0x0008|OPTIMISTIC（以前称为 OPTCCVAL）|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 与 *scrollopt* 一样， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以覆盖请求的 *ccopt* 值。  
  
 *数*  
 要用于 AUTO_FETCH 的提取缓冲区行数。 默认值为 20 行。 指定为输入值与返回值时，*行计数* 的行为不同。  
  
|作为输入值|作为返回值|  
|--------------------|---------------------|  
|当指定的 AUTO_FETCH *scrollopt* 值为 *rowcount* 时，表示要放入提取缓冲区中的行数。<br /><br /> 注意：如果指定 AUTO_FETCH，则 >0 是有效的值，否则将被忽略。|表示结果集中的行数，除非指定了 *scrollopt* AUTO_FETCH 值。|  
  
-  
  
 *boundparam*  
 指示使用其他参数。 *boundparam* 是一个可选参数，如果 *scrollopt* PARAMETERIZED_STMT 值设置为 ON，则应指定此参数。  
  
## <a name="return-code-values"></a>返回代码值  
 如果未引发错误，则 sp_cursoropen 返回以下值之一。  
  
 0  
 已成功执行该过程。  
  
 0x0001  
 在执行过程中发生了错误（次要错误，严重程度不足以在操作中引发错误）。  
  
 0x0002  
 正在执行异步操作。  
  
 0x0002  
 正在执行 FETCH 操作。  
  
 A  
 此游标已由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 取消分配而不可用。  
  
 当引发错误时，返回值可能不一致，无法确保准确性。  
  
 当 *rowcount* 参数指定为返回值时，将出现以下结果集。  
  
 -1  
 如果行数未知或不适用，则返回此值。  
  
 -n  
 当异步填充生效时，返回此值。 表示在指定 *scrollopt* AUTO_FETCH 值时放置在提取缓冲区中的行数。  
  
 如果使用 RPC，则返回值如下所示。  
  
 0  
 过程成功。  
  
 1  
 过程失败。  
  
 2  
 正在异步生成键集游标。  
  
 16  
 快进游标已自动关闭。  
  
> [!NOTE]  
>  如果 sp_cursoropen 过程成功执行，则会发送 RPC 返回参数和具有 TDS 列格式信息的结果集 (0xa0 & 0xa1 消息) 。 如果不成功，则发送一条或多条 TDS 错误消息。 在任一情况下，都不会返回行数据，已 *完成* 的消息计数将为零。 如果您使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本低于 7.0，将返回 0xa0、0xa1（SELECT 语句的标准）以及 0xa5 和 0xa4 标记流。 如果您使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0，将返回 0x81（SELECT 语句的标准）以及 0xa5 和 0xa4 标记流。  
  
## <a name="remarks"></a>备注  
  
## <a name="stmt-parameter"></a>stmt 参数  
 如果 *stmt* 指定了存储过程的执行，则输入参数可能会定义为 *常量字符串的* 一部分，或指定为 *boundparam* 参数。 通过此方法，可以将声明的变量作为绑定参数传递。  
  
 *Stmt* 参数允许的内容取决于 *ccopt* ALLOW_DIRECT 返回值是由链接还是链接到 *ccopt* 值的其余部分，例如：  
  
-   如果未指定 ALLOW_DIRECT，则 [!INCLUDE[tsql](../../includes/tsql-md.md)] 必须使用为包含单个 SELECT 语句的存储过程调用的 SELECT 语句或执行语句。 此外，SELECT 语句必须限定为一个游标；也即，它不能包含关键字 SELECT INTO 或 FOR BROWSE。  
  
-   如果指定了 ALLOW_DIRECT，则这可能导致一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，包括那些依次执行其他存储过程以及多个语句的语句。 将只执行非 SELECT 语句或包含关键字 SELECT INTO 或 FOR BROWSE 的任何 SELECT 语句，而不会导致创建游标。 这同样适用于在一批多个语句中包含的任何 SELECT 语句。 在 SELECT 语句包含只与游标相关的子句的情况下，将忽略这些子句。 例如，当 *ccopt* 的值为0x2002 时，这是对的请求：  
  
    -   具有滚动锁的游标（如果只有一个 SELECT 语句限定为游标），或者  
  
    -   一个直接的语句执行（如果有多个语句、一个非 SELECT 语句或不限定为游标的 SELECT 语句）。  
  
## <a name="scrollopt-parameter"></a>scrollopt 参数  
 前五个 *scrollopt* 值 (KEYSEY、DYNAMIC、FORWARD_ONLY、STATIC 和 FAST_FORWARD) 互相排斥。  
  
 PARAMETERIZED_STMT 和 CHECK_ACCEPTED_TYPES 可以由 OR 链接到前五个值中的任何一个。  
  
 AUTO_FETCH 和 AUTO_CLOSE 可以由 OR 链接到 FAST_FORWARD。  
  
 如果 CHECK_ACCEPTED_TYPES 为 ON，则 (KEYSET_ACCEPTABLE DYNAMIC_ACCEPTABLE、FORWARD_ONLY_ACCEPTABLE、STATIC_ACCEPTABLE 或 FAST_FORWARD_ACCEPTABLE 的最后五个 *scrollopt* 值中至少有一个值 `,` 也必须为 ON。  
  
 STATIC 游标始终打开为 READ_ONLY。 这意味着无法通过此游标更新基础表。  
  
## <a name="ccopt-parameter"></a>ccopt 参数  
 前四个 *ccopt* 值 (READ_ONLY，SCROLL_LOCKS，并且两个乐观值) 都是互斥的。  
  
> [!NOTE]  
>  选择前四个 *ccopt* 值中的一个值，指示游标是只读的，还是使用锁定或乐观方法来防止丢失更新。 如果未指定 *ccopt* 值，则默认值为乐观。  
  
 ALLOW_DIRECT 和 CHECK_ACCEPTED_TYPES 可以由 OR 链接到前四个值中的任何一个。  
  
 UPDT_IN_PLACE 可以由 OR 链接到 READ_ONLY、SCROLL_LOCKS 或任一 OPTIMISTIC 值。  
  
 如果 CHECK_ACCEPTED_TYPES 为 ON，则 (READ_ONLY_ACCEPTABLE，SCROLL_LOCKS_ACCEPTABLE，最后四个 *ccopt* 值中至少有一个，，并且 OPTIMISTIC_ACCEPTABLE 值都必须为 ON。  
  
 定位更新和删除函数只能在提取缓冲区内执行，并且仅当 *ccopt* 值等于 SCROLL_LOCKS 或乐观时才可执行。 如果 SCROLL_LOCKS 是指定的值，则此操作可确保成功。 如果 OPTIMISTIC 是指定的值，则当自上次提取该行后行已发生变化时，操作将失败。  
  
 失败原因在于：当 OPTIMISTIC 为指定的值时，将通过比较时间戳或校验和值执行乐观并发控制函数（由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 确定）。 如果任何行不匹配，则操作失败。  
  
 将 UPDT_IN_PLACE 指定为返回值可控制以下结果：  
  
-   如果未设置，则当对某个表使用唯一索引执行定位更新时，游标将从其工作表中删除该行，并将其插入到游标使用的任何键列的末尾，从而更改这些列。  
  
-   如果设置为 ON，游标将只更新工作表的原始行中的键列。  
  
## <a name="bound_param-parameter"></a>bound_param 参数  
 根据代码中的错误消息指定 PARAMETERIZED_STMT 时，参数名称应为 *paramdef* 。 当未指定 PARAMETERIZED_STMT 时，在错误消息将不指定任何名称。  
  
## <a name="rpc-considerations"></a>RPC 注意事项  
 RPC RETURN_METADATA 输入标志可设置为 0x0001，以请求在 TDS 流中返回游标选择列表元数据。  
  
## <a name="examples"></a>示例  
  
### <a name="bound_param-parameter"></a>bound_param 参数  
 第五个参数之后的任何参数都将作为输入参数传递到语句计划。 第一个此类参数必须为以下格式的字符串：  
  
 *{局部变量名称数据类型}[,...北*  
  
 后续参数用于传递要替换为语句中的 *局部变量名称* 的值。  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursorfetch &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
