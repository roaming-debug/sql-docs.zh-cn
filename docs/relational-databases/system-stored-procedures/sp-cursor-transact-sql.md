---
description: sp_cursor (Transact-SQL)
title: sp_cursor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530093d48d1fa811231a5883b1b2b7e0f3922bf8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205181"
---
# <a name="sp_cursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  请求定位更新。 此过程对游标的提取缓冲区内的一行或多行执行操作。 sp_cursor 通过在表格格式数据流 (TDS) 数据包中指定 ID = 1 来调用。  
  
||  
|-|  
|**适用** 于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 通过 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)) SQL Server (。|  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>参数  
 *cursor*  
 游标句柄。 *cursor* 是为 **int** 输入值调用的必需参数。 *cursor* 是由 SQL Server 生成并由 sp_cursoropen 过程返回的 *句柄* 值。  
  
 *optype*  
 一个必需参数，它指定游标将执行的操作。 *optype* 需要以下 **int** 输入值之一。  
  
|值|名称|说明|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|用于更新提取缓冲区中的一行或多行。  重新访问和更新 *rownum* 中指定的行。|  
|0x0002|删除|用于删除提取缓冲区中的一行或多个行。 重新访问和删除 *rownum* 中指定的行。|  
|0X0004|INSERT|插入数据而不生成 SQL **INSERT** 语句。|  
|0X0008|REFRESH|用于从基础表中重新填充缓冲区，并且可用于刷新行（如果由于乐观并发控制导致更新或删除失败或在某个 UPDATE 之后）。|  
|0X10|LOCK|导致在包含指定行的页上获取 SQL Server U-锁。 此锁与 S 锁兼容，但不与 X 锁或其他 U 锁兼容。 可用于实现短期锁定。|  
|0X20|SETPOSITION|仅当程序要发出后续 SQL Server 定位的 DELETE 或 UPDATE 语句时才使用。|  
|0X40|ABSOLUTE|仅能与 UPDATE 或 DELETE 结合使用。  ABSOLUTE 仅与 KEYSET 游标结合使用（对于 DYNAMIC 游标将忽略，并且无法更新 STATIC 游标）。<br /><br /> 注意：如果在尚未提取的键集中的行上指定了绝对值，则此操作可能会导致并发检查失败，并且无法保证返回结果。|  
  
 *rownum*  
 指定提取缓冲区中游标将对其执行操作、更新或删除的行。  
  
> [!NOTE]  
>  这不会影响任何 RELATIVE、NEXT 或 PREVIOUS 提取操作的起点，也不影响任何使用 sp_cursor 执行的更新或删除。  
  
 *rownum* 是一个必需参数，用于调用 **int** 输入值。  
  
 1  
 指示提取缓冲区中的第一行。  
  
 2  
 指示提取缓冲区中的第二行。  
  
 3、4、5  
 指示第三行，依此类推。  
  
 n  
 指示提取缓冲区中的第 n 行。  
  
 0  
 指示提取缓冲区中的所有行。  
  
> [!NOTE]  
>  仅适用于更新、删除、刷新或锁定 *optype* 值。  
  
 *table*  
 当游标定义涉及联接或 *值* 参数返回不明确的列名时，用于标识 *optype* 适用的表的表名。 如果未指定特定的表，则默认为 FROM 子句中的第一个表。 *table* 是一个可选参数，它需要字符串输入值。 可以将此字符串指定为任何字符或 UNICODE 数据类型。 *表* 可以是由多个部分组成的表名。  
  
 *value*  
 用于插入或更新值。 *值* string 参数仅与 UPDATE 和 INSERT *optype* 值一起使用。 可以将此字符串指定为任何字符或 UNICODE 数据类型。  
  
> [!NOTE]  
>  *值* 的参数名可以由用户分配。  
  
## <a name="return-code-values"></a>返回代码值  
 当使用 RPC 时，如果已定位的删除或更新操作的缓冲区数为0，则将返回 (失败的 *行计数* 为0的已完成消息) 或 1 (获取缓冲区中每一行的成功) 。  
  
## <a name="remarks"></a>备注  
  
## <a name="optype-parameter"></a>optype 参数  
 除了 SETPOSITION 和 UPDATE、DELETE、REFRESH 或 LOCK 的组合以外，对于 UPDATE 或 DELETE， *optype* 值是相互排斥的。  
  
 更新值的 SET 子句是从 *value* 参数构造的。  
  
 使用 INSERT *optype* 值的一个优点是，您可以避免将非字符数据转换为字符格式以进行插入。 指定值的方式与 UPDATE 相同。 如果不包含任何所需的列，则 INSERT 失败。  
  
-   SETPOSITION 值不影响任何 RELATIVE、NEXT 或 PREVIOUS 提取操作的起点，也不会使用 sp_cursor 接口执行任何更新或删除。 任何未在提取缓冲区中指定行的编号都将导致位置设置为 1，且不返回错误。 执行 SETPOSITION 后，位置将保持有效，直到下一个 sp_cursorfetch 操作、T-sql **FETCH** 操作或 sp_cursor SETPOSITION 操作通过同一个游标。 在其他游标调用将不会影响此位置的值时，后续 sp_cursorfetch 操作将游标的位置设置为新提取缓冲区中的第一行。 SETPOSITION 可由 OR 子句链接到 REFRESH、UPDATE、DELETE 或 LOCK，以便将位置的值设置为上次修改的行。  
  
 如果未通过 *rownum* 参数指定提取缓冲区中的行，则该位置将设置为1，并且不会返回任何错误。 一旦设置了此位置，它将保持有效，直至对同一游标执行下一个 sp_cursorfetch 操作、T-SQL FETCH 操作或 sp_cursor SETPOSITION 操作。  
  
 SETPOSITION 可由 OR 子句与 REFRESH、UPDATE、DELETE 或 LOCK 进行链接，以便将游标位置设置为上一次修改的行。  
  
## <a name="rownum-parameter"></a>rownum 参数  
 如果指定此参数，则可以将 *rownum* 参数解释为键集中的行号，而不是提取缓冲区中的行号。 用户负责确保维护并发控制。 这意味着，对于 SCROLL_LOCKS 游标，您必须独立维护给定行的锁（此操作可通过一个事务完成）。 对于 OPTIMISTIC 游标，您必须事先提取行以执行此操作。  
  
## <a name="table-parameter"></a>table 参数  
 如果 *optype* 值为 UPDATE 或 insert 并且完整的 UPDATE 或 insert 语句作为 *值* 参数提交，则忽略为 *table* 指定的值。  
  
> [!NOTE]  
>  与视图相关，仅可修改一个参与视图的表。 *值* 参数列名称必须反映视图中的列名，但表名称可以是基础基表的名称 (在这种情况下，sp_cursor 会将视图名称替换) 。  
  
## <a name="value-parameter"></a>value 参数  
 如前面在 "参数" 部分中所述，使用 *值* 的规则有两种替代方法：  
  
1.  \@对于任何命名 *值* 参数，可以在选择列表中对列的名称使用预先挂起的名称。 此替代方法的一个优点是可能不需要进行数据转换。  
  
2.  使用参数提交完整的 UPDATE 语句或 INSERT 语句，或使用多个参数提交 UPDATE 语句或 INSERT 语句的部分，SQL Server 稍后将其生成为完整的语句。 此类示例可在本主题后面的“示例”部分中找到。  
  
## <a name="examples"></a>示例  
  
### <a name="alternative-value-parameter-uses"></a>备选 value 参数用法  
 对于 UPDATE：  
  
 当使用单个参数时，可以使用以下语法提交 UPDATE 语句：  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,...n]`  
  
> [!NOTE]  
>  如果 \<table name> 指定 UPDATE，将忽略为 *表* 参数指定的任何值。  
  
 当使用多个参数时，第一个参数必须为采用以下形式的字符串：  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 并且后续参数必须为以下形式：  
  
 `<column name> = expression  [,...n]`  
  
 在这种情况下， \<table name> 构造的 update 语句中的是由 *table* 参数指定或默认为的。  
  
 对于 INSERT：  
  
 当使用单个参数时，可以使用以下语法提交 INSERT 语句：  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  如果 *\<table name>* 指定 INSERT，则将忽略为 *表* 参数指定的任何值。  
  
 当使用多个参数时，第一个参数必须为采用以下形式的字符串：  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 并且后续参数必须为以下形式：  
  
 `expression [,...n]`  
  
 除了指定 VALUES 之外，在此情况下，在最后一个表达式后面必须带一个尾随“)”。 在这种情况下， *\<table name>* 构造的 UPDATE 语句中的是由 *table* 参数指定或默认为的。  
  
> [!NOTE]  
>  可以提交一个参数作为命名参数，也即“`@VALUES`”。 在此情况下，不能使用其他命名参数。  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
