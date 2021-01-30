---
description: sp_cursorprepare (Transact-SQL)
title: sp_cursorprepare (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_prepare
ms.assetid: 6207e110-f4bf-4139-b3ec-b799c9cb3ad7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df4b0ff31a5b8a1723b6c8f130aeab9b32b03166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209752"
---
# <a name="sp_cursorprepare-transact-sql"></a>sp_cursorprepare (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将游标语句或批处理编译成执行计划，但并不创建游标。 编译的语句以后可供 sp_cursorexecute 使用。 此过程与 sp_cursorexecute 结合在一起具有与 sp_cursoropen 相同的功能，但拆分为两个阶段。 通过在表格格式数据流 (TDS) 数据包中指定 ID = 3 来调用 sp_cursorprepare。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursorprepare prepared_handle OUTPUT, params , stmt , options  
    [ , scrollopt[ , ccopt]]  
```  
  
## <a name="arguments"></a>参数  
 *prepared_handle*  
 SQL Server 生成的已准备好的 *句柄* 标识符，它返回一个整数值。  
  
> [!NOTE]  
>  *prepared_handle* 随后提供给 sp_cursorexecute 过程，以打开游标。 一旦创建了句柄，它就一直存在，直到您注销或通过 sp_cursorunprepare 过程显式删除它。  
  
 *params*  
 标识参数化语句。 变量的参数定义将替换为语句 *中的参数* 标记。 *params* 是调用 **ntext**、 **nchar** 或 **nvarchar** 输入值的必需参数。 如果语句未参数化，则输入一个 NULL 值。  
  
> [!NOTE]  
>  当 *stmt* 已参数化且 *scrollopt* PARAMETERIZED_STMT 值为 ON 时，使用 **ntext** 字符串作为输入值。  
  
 *stmt*  
 定义游标结果集。 *Stmt* 参数是必需的，并且调用了 **ntext**、 **nchar** 或 **nvarchar** 输入值。  
  
> [!NOTE]  
>  用于指定 *stmt* 值的规则与 sp_cursoropen 的规则相同，但 *stmt* 字符串数据类型必须为 **ntext**。  
  
 *options*  
 一个可选参数，它返回游标结果集列的说明。 *选项* 需要以下 **int** 输入值。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
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
  
 因为请求的值可能不适合于 *stmt* 定义的游标，所以，此参数可同时用作输入和输出。 在此类情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配一个适当的值。  
  
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
  
 与 *scrollpt* 一样， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以从所请求的值分配不同的值。  
  
## <a name="remarks"></a>备注  
 RPC 状态参数为以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|0|成功|  
|0x0001|失败|  
|1FF6|无法返回元数据。<br /><br /> 注意：此操作的原因是语句不生成结果集;例如，它是 INSERT 或 DDL 语句。|  
  
## <a name="examples"></a>示例  
  下面是使用 sp_cursorprepare 和的示例 sp_cursorexecute

```sql
declare @handle int , @p5 int, @p6 int
exec sp_cursorprepare @handle OUTPUT, 
    N'@dbid int', 
    N'select * from sys.databases where database_id < @dbid',
    1,
    @p5 output,
    @p6 output


declare @p1 int  
set @P1 = @handle 
declare @p2 int   
declare @p3 int  
declare @p4 int  
set @P6 = 4 
exec sp_cursorexecute @p1, @p2 OUTPUT, @p3 output , @p4 output, @p5 OUTPUT, @p6

exec sp_cursorfetch @P2

exec sp_cursorunprepare @handle
exec sp_cursorclose @p2
```
 
 如果对 *stmt* 进行参数化，并且 *scrollopt* PARAMETERIZED_STMT 值为 ON，则字符串的格式如下所示：  
  
 { *\<local variable name>**\<data type>* } [ ,...*n* ]  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursorexecute &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorunprepare &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorunprepare-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
