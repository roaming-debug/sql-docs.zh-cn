---
description: UPDATETEXT (Transact-SQL)
title: UPDATETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATETEXT
- UPDATETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text updates [SQL Server]
- updating data [SQL Server]
- data updates [SQL Server], UPDATETEXT statement
- UPDATETEXT statement
ms.assetid: d73c28ee-3972-4afd-af8d-ebbbd9e50793
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5304394a75662d5cdc37196aab70444ebfbcbe41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186912"
---
# <a name="updatetext-transact-sql"></a>UPDATETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新现有 text、ntext 或 image 字段。 使用 UPDATETEXT 可以只更改 text、ntext 或 image 列的一部分。 使用 WRITETEXT 可以更新和替换整个 text、ntext 或 image 字段。  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]请改用大值数据类型和 [UPDATE](../../t-sql/queries/update-transact-sql.md) 语句的 .WRITE 子句。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
UPDATETEXT [BULK] { table_name.dest_column_name dest_text_ptr }  
  { NULL | insert_offset }  
     { NULL | delete_length }  
     [ WITH LOG ]  
     [ inserted_data  
    | { table_name.src_column_name src_text_ptr } ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 BULK  
 启用上载工具来上载二进制数据流。 该数据流必须由该工具在 TDS 协议级别提供。 在数据流不存在时，查询处理器将忽略 BULK 选项。  
  
> [!IMPORTANT]  
>  我们建议不要在基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的应用程序中使用 BULK 选项。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中可能会更改或删除该选项。  
  
 table_name。 dest_column_name  
 要更新的表以及 text、ntext 或 image 列的名称。 表名和列名必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 可以选择是否指定数据库名和所有者名。  
  
 dest_text_ptr  
 指向要更新的 text、ntext 或 image 数据的文本指针的值（由 TEXTPTR 函数返回）。 dest_text_ptr 必须是二进制 (16)。  
  
 insert_offset  
 以零为基的更新起始位置。 对于 text 或 image 列，insert_offset 是在插入新数据前要从现有列的起点跳过的字节数。 对于 ntext 列，insert_offset 是字符数（每个 ntext 字符占用 2 个字节）。 从此基数为零的起始点开始的现有 text、ntext 或 image 数据向右移，为新数据留出空间。 值为 0 表示将新数据插入现有数据的开始处。 值为 NULL 则将新数据追加到现有数据值后。  
  
 delete_length  
 从 insert_offset 位置开始的、要从现有 text、ntext 或 image 列中删除的数据长度。 delete_length 值为 text 和 image 列指定时以字节为单位，为 ntext 列指定时以字符为单位。 每个 ntext 字符占用 2 个字节。 值为 0 表示不删除数据。 值为 NULL 则删除现有 text 或 image 列中从 insert_offset 位置开始到末尾的所有数据。  
  
 WITH LOG  
 日志记录由数据库的当前恢复模式决定。  
  
 inserted_data  
 要插入到 insert_offset 位置现有 text、ntext 或 image 列中的数据。 这是单个 char、nchar、varchar、nvarchar、binary、varbinary、text、ntext 或 image 值。 inserted_data 可以是文字或变量。  
  
 table_name.src_column_name  
 用作插入数据源的表和 text、ntext 或 image 列的名称。 表名和列名必须符合标识符规则。  
  
 src_text_ptr  
 指向用作插入数据源的 text、ntext 或 image 列的文本指针值（由 TEXTPTR 函数返回）。  
  
> [!NOTE]  
>  scr_text_ptr 值不能与 dest_text_ptr 值相同。  
  
## <a name="remarks"></a>注解  
 新插入的数据可以是单个 inserted_data 常量、表名、列名或文本指针。  
  
|Update 操作|UPDATETEXT 参数|  
|-------------------|---------------------------|  
|替换现有数据|指定一个非空 insert_offset 值、非零 delete_length 值和要插入的新数据。|  
|删除现有数据|指定非空 insert_offset 值和非零 delete_length。 不指定要插入的新数据。|  
|插入新数据|指定 insert_offset 值、为 0 的 delete_length 和要插入的新数据。|  
  
 为获得最佳性能，建议按照块区大小为 8,040 字节倍数的方式插入或更新数据。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，可能存在指向 text、ntext 或 image 数据的行内文本指针，但可能无效。 有关 text in row 选项的信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。 有关使文本指针无效的信息，请参阅 [sp_invalidate_textptr (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md)。  
  
 若要将 text 列初始化为 NULL，请使用 WRITETEXT；UPDATETEXT 将 text 列初始化为空字符串。  
  
## <a name="permissions"></a>权限  
 需要对指定表的 UPDATE 权限。  
  
## <a name="examples"></a>示例  
 以下示例将文本指针放入局部变量 `@ptrval` 中，然后使用 `UPDATETEXT` 更新拼写错误。  
  
> [!NOTE]  
>  若要运行此示例，必须安装 pubs 数据库。  
  
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
UPDATETEXT pub_info.pr_info @ptrval 88 1 'b';  
GO  
ALTER DATABASE pubs SET RECOVERY FULL;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [READTEXT (Transact-SQL)](../../t-sql/queries/readtext-transact-sql.md)   
 [TEXTPTR (Transact-SQL)](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)  
  
  
