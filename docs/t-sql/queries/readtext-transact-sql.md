---
description: READTEXT (Transact-SQL)
title: READTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- READTEXT_TSQL
- READTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- column reading [SQL Server]
- READTEXT statement
- reading columns
ms.assetid: 91b69853-1381-4306-8343-afdb73105738
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e312bff216746345fc028b1ff581bdee56a12eca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207621"
---
# <a name="readtext-transact-sql"></a>READTEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从 **text**、**ntext** 或 **image** 列读取 **text**、**ntext** 或 **image** 值。 从指定的偏移量开始读取，并读取指定的字节数。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]请改用 [SUBSTRING](../../t-sql/functions/substring-transact-sql.md) 函数。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
READTEXT { table.column text_ptr offset size } [ HOLDLOCK ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_table_ **.** _column_  
要对其执行读取操作的表和列的名称。 表名和列名必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 必须指定表名和列名。但是，可根据需要指定数据库名称和数据库所有者名称。  
  
_text\_ptr_  
有效的文本指针。 _text\_ptr_ 必须为 **binary(16)** 。  
  
_offset_  
使用 **text** 或 **image** 数据类型时的字节数。 它还可以是使用 **ntext** 数据类型时，开始读取 **text**、**image** 或 **ntext** 数据之前要跳过的字符的字节数。  
  
_size_ 使用 **text** 或 **image** 数据类型时的字节数。 它还可以是将 **ntext** 数据类型用于待读取数据时的字符的字节数。 如果 _size_ 为 0，则读取 4 KB 数据。  
  
HOLDLOCK  
使文本值被锁定以进行读取，直到事务结束为止。 其他用户可读取该值，但不能对其进行修改。  
  
## <a name="remarks"></a>备注  
使用 [TEXTPTR](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md) 函数获取有效的 _text\_ptr_ 值。 TEXTPTR 返回指向指定行中的 **text**、**ntext** 或 **image** 列的指针。 如果查询返回多行，则 TEXTPRT 还可以返回指向查询返回的最后一行中的 **text**、**ntext** 或 **image** 列的指针。 由于 TEXTPTR 返回 16 字节的二进制字符串，因此我们建议声明一个局部变量来保存该文本指针，然后在 READTEXT 中使用该变量。 有关创建局部变量的详细信息，请参阅 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)。  
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，可能存在行内文本指针，但该指针可能无效。 有关 text in row 选项的详细信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)  。 有关使文本指针无效的详细信息，请参阅 [sp_invalidate_textptr (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md)。  
  
如果 @@TEXTSIZE 函数的值小于为 READTEXT 指定的大小，则该值将替代为 READTEXT 指定的大小。 @@TEXTSIZE 函数指定对 SET TEXTSIZE 语句设置的返回数据字节数的限制。 有关为 TEXTSIZE 设置会话设置的详细信息，请参阅 [SET TEXTSIZE (Transact-SQL)](../../t-sql/statements/set-textsize-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
READTEXT 权限默认授予对指定的表具有 SELECT 权限的用户。 可在转移 SELECT 权限时转移权限。  
  
## <a name="examples"></a>示例  
以下示例读取 `pub_info` 表中 `pr_info` 列的第 2 个至第 26 个字符。  
  
> [!NOTE]  
>  若要运行此示例，必须安装 [pubs](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases)  示例数据库。  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr INNER JOIN publishers p  
      ON pr.pub_id = p.pub_id   
      AND p.pub_name = 'New Moon Books'  
READTEXT pub_info.pr_info @ptrval 1 25;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[@@TEXTSIZE (Transact-SQL)](../../t-sql/functions/textsize-transact-sql.md)   
[UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)   
[WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)  
  
  
