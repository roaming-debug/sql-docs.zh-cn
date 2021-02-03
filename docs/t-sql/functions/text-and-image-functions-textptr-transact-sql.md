---
description: 文本与图像函数 - TEXTPTR (Transact-SQL)
title: TEXTPTR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTPTR_TSQL
- TEXTPTR
dev_langs:
- TSQL
helpviewer_keywords:
- TEXTPTR function
- viewing text pointer values
- text-pointer values
- displaying text pointer values
ms.assetid: 2672b8cb-f747-46f3-9358-9b49b3583b8e
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 39e1c646755c4ea2907b328938cac0f30f6f47d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181452"
---
# <a name="text-and-image-functions---textptr-transact-sql"></a>文本与图像函数 - TEXTPTR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回对应于 varbinary 格式的 text、ntext 或 image 列的文本指针值。 检索到的文本指针值可用于 READTEXT、WRITETEXT 和 UPDATETEXT 语句。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]没有可用的替代功能。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TEXTPTR ( column )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *column*  
 将要使用的 text、ntext 或 image 列。  
  
## <a name="return-types"></a>返回类型  
 **varbinary**  
  
## <a name="remarks"></a>备注  
 对于含行内文本的表，TEXTPTR 将为要处理的文本返回一个句柄。 即使文本值为空，仍可获得有效的文本指针。  
  
 不能对视图列使用 TEXTPTR 函数。 只能对表列使用此函数。 若要在视图列中使用 TEXTPTR 函数，必须使用 [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)将兼容级别设置为 80。 如果表不含行内文本，并且 text、ntext 或 image 列尚未使用 UPDATETEXT 语句初始化，则 TEXTPTR 将返回一个空指针。  
  
 可使用 TEXTVALID 来测试文本指针是否存在。 在没有有效的文本指针的情况下，不能使用 UPDATETEXT、WRITETEXT 或 READTEXT。  
  
 当使用 text、ntext 和 image 数据时，下列函数和语句也非常有用。  
  
|函数或语句|描述|  
|---------------------------|-----------------|  
|PATINDEX<b>('</b>%pattern%' , expression)|返回指定字符串在 text 或 ntext 列中所处的字符位置。|  
|DATALENGTH(expression)<b></b>|返回 text、ntext 和 image 列中数据的长度。|  
|SET TEXTSIZE|返回使用 SELECT 语句时返回的 text、ntext 或 image 数据的限制（字节）。|  
|SUBSTRING(text_column, start, length)<b></b>  |返回由指定的 start 偏移量和 length 指定的 varchar 字符串。 字符串的长度应小于 8 KB。|  
  
## <a name="examples"></a>示例  
  
> [!NOTE]  
>  若要运行以下示例，必须安装 pubs 数据库  。  
  
### <a name="a-using-textptr"></a>A. 使用 TEXTPTR  
 以下示例将使用 `TEXTPTR` 函数来查找与 `pubs` 数据库的 `pub_info` 表中的 `New Moon Books` 关联的 image 列 `logo`。 文本指针放在局部变量 `@ptrval.` 中。  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(logo)  
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books';  
GO  
```  
  
### <a name="b-using-textptr-with-in-row-text"></a>B. 将 TEXTPTR 用于行内文本  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，必须在以下示例所示的事务中使用行内文本指针。  
  
```sql
CREATE TABLE t1 (c1 INT, c2 TEXT);  
EXEC sp_tableoption 't1', 'text in row', 'on';  
INSERT t1 VALUES ('1', 'This is text.');  
GO  
BEGIN TRAN;  
   DECLARE @ptrval VARBINARY(16);  
   SELECT @ptrval = TEXTPTR(c2)  
   FROM t1  
   WHERE c1 = 1;  
   READTEXT t1.c2 @ptrval 0 1;  
COMMIT;  
```  
  
### <a name="c-returning-text-data"></a>C. 返回文本数据  
 以下示例将从 `pub_id` 表中选择 `pr_info` 列和 `pub_info` 列的 16 字节文本指针。  
  
```sql
USE pubs;  
GO  
SELECT pub_id, TEXTPTR(pr_info)  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id                                      
------ ----------------------------------   
0736   0x6c0000000000feffb801000001000100   
0877   0x6d0000000000feffb801000001000300   
1389   0x6e0000000000feffb801000001000500   
1622   0x700000000000feffb801000001000900   
1756   0x710000000000feffb801000001000b00   
9901   0x720000000000feffb801000001000d00   
9952   0x6f0000000000feffb801000001000700   
9999   0x730000000000feffb801000001000f00   
  
(8 row(s) affected)  
```  
  
 以下示例显示如何在不使用 TEXTPTR 的情况下，返回文本的前 `8000` 个字节。  
  
```sql
USE pubs;  
GO  
SET TEXTSIZE 8000;  
SELECT pub_id, pr_info  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id pr_info                                                                                                                                                                                                                                                           
------ -----------------------------------------------------------------  
0736   New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!                                                                                                             
0877   This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washington, D.C.  
  
This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washi   
1389   This is sample text data for Algodata Infosystems, publisher 1389 in the pubs database. Algodata Infosystems is located in Berkeley, California.  
  
9999   This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in Paris, France.  
  
This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in   
  
(8 row(s) affected)  
```  
  
### <a name="d-returning-specific-text-data"></a>D. 返回特定文本数据  
 以下示例将查找与 `pubs` 数据库的 `pub_info` 表中的 `pub_id``0736` 关联的 `text` 列 (`pr_info`)。 该例首先声明了一个局部变量 `@val`。 然后，将文本指针（长二进制字符串）置于 `@val` 中，并将其作为参数提供给 `READTEXT` 语句。 结果将返回从第五个字节（偏移量为 4）开始的 10 个字节。  
  
```sql
USE pubs;  
GO  
DECLARE @val VARBINARY(16);  
SELECT @val = TEXTPTR(pr_info)   
FROM pub_info  
WHERE pub_id = '0736';  
READTEXT pub_info.pr_info @val 4 10;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pr_info                                                                                                                                                                                                                                                           
-----------------------------------------------------------------------  
 is sample  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
 [READTEXT (Transact-SQL)](../../t-sql/queries/readtext-transact-sql.md)   
 [SET TEXTSIZE (Transact-SQL)](../../t-sql/statements/set-textsize-transact-sql.md)   
 [文本与图像函数 (Transact-SQL)]()   
 [UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)   
 [WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)  
  
