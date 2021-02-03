---
description: WRITETEXT (Transact-SQL)
title: WRITETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WRITETEXT_TSQL
- WRITETEXT
dev_langs:
- TSQL
helpviewer_keywords:
- replacing data
- WRITETEXT statement
- updating data [SQL Server]
- nonlogged updating
- minimally logged updating [SQL Server]
- overwriting data
- data updates [SQL Server], WRITETEXT statement
ms.assetid: 80c252fd-a8b8-4a2e-888a-059081ed4109
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1793b813e763df5cb8078a73fb1cd78c7fe7a6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193371"
---
# <a name="writetext-transact-sql"></a>WRITETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  允许对现有的 text、ntext 或 image 列执行最小日志记录的交互式更新。 WRITETEXT 将覆盖受其影响的列中的所有现有数据。 WRITETEXT 语句不能用于视图中的 text、ntext 或 image 列。  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]请改用大值数据类型和 [UPDATE](../../t-sql/queries/update-transact-sql.md) 语句的 .WRITE 子句。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
WRITETEXT [BULK]  
  { table.column text_ptr }  
  [ WITH LOG ] { data }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 BULK  
 启用上载工具来上载二进制数据流。 该数据流必须由该工具在 TDS 协议级别提供。 在数据流不存在时，查询处理器将忽略 BULK 选项。  
  
> [!IMPORTANT]  
>  我们建议不要在基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的应用程序中使用 BULK 选项。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中可能会更改或删除该选项。  
  
 *table* **.column**  
 要更新的表以及 text、ntext 或 image 列的名称。 表名和列名必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 可以选择是否指定数据库名和所有者名。  
  
 text_ptr  
 存储指向 text、ntext 或 image 数据的指针的值。 text_ptr 必须是 binary(16)。若要创建文本指针，请对非 NULL 值的 text、ntext 或 image 列数据执行 [INSERT](../../t-sql/statements/insert-transact-sql.md) 或 [UPDATE](../../t-sql/queries/update-transact-sql.md) 语句。  
  
 WITH LOG  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 忽略此参数。 日志记录由数据库的当前恢复模式决定。  
  
 *data*  
 要存储的实际 text、ntext 或 image 数据。 data 可以是文字或参数。 对于 text、ntext 或 image 数据，可以使用 WRITETEXT 以交互方式插入的最大文本长度约为 120 KB。  
  
## <a name="remarks"></a>备注  
 使用 WRITETEXT 替换 text、ntext 或 image 数据，使用 UPDATETEXT 修改 text、ntext 或 image 数据。 UPDATETEXT 更加灵活，因为它只更改 text、ntext 或 image 列的一部分而非整个列。  
  
 为获得最佳性能，建议按照块区大小为 8040 字节倍数的方式插入或更新 text、ntext 或 image 数据。  
  
 如果数据库为简单恢复模式或大容量日志恢复模式，则在插入或追加新数据时，使用 WRITETEXT 语句的 text、ntext 或 image 操作将成为最小日志记录操作。  
  
> [!NOTE]  
>  在更新现有值时，不使用最小日志记录。  
  
 为了正确使用 WRITETEXT，列必须已经包含有效的文本指针。  
  
 如果该表没有行内文本，则在通过 INSERT 向 text 列中添加显式或隐式 NULL 值时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不初始化 text 列以节省空间，并且不能获取这类 NULL 值的文本指针。 若要将 text 列初始化为 NULL，请使用 UPDATE 语句。 如果表有行内文本，则不必为空值初始化文本列，而且始终可以获取文本指针。  
  
 ODBC SQLPutData 函数更快，且使用的动态内存比 WRITETEXT 少。 该函数最多可插入 2 GB 的 text、ntext 或 image 数据。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，可能存在指向 text、ntext 或 image 数据的行内文本指针，但可能无效。 有关 text in row 选项的信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。 有关使文本指针无效的信息，请参阅 [sp_invalidate_textptr (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 需要对指定表的 UPDATE 权限。 可在转移 UPDATE 权限时转移这些权限。  
  
## <a name="examples"></a>示例  
 以下示例将文本指针放到局部变量 `@ptrval` 中，然后 `WRITETEXT` 将新的文本字符串放到 `@ptrval` 指向的行中。  
  
> [!NOTE]  
>  若要运行此示例，必须安装 pubs 示例数据库。  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books'  
WRITETEXT pub_info.pr_info @ptrval 'New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!';  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
