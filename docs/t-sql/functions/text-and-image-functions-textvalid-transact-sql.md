---
description: 文本与图像函数 - TEXTVALID (Transact-SQL)
title: TEXTVALID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTVALID_TSQL
- TEXTVALID
dev_langs:
- TSQL
helpviewer_keywords:
- invalid text pointers [SQL Server]
- valid text pointers [SQL Server]
- TEXTVALID function
- checking text pointers
- text-pointer values
- verifying text pointers
ms.assetid: 9411c349-b59b-4740-a270-92f91d81ad23
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 17a2d30b57e5c60624509f5adaafc2fb94fc30d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181463"
---
# <a name="text-and-image-functions---textvalid-transact-sql"></a>文本与图像函数 - TEXTVALID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  检查特定文本指针是否有效的 text、ntext 或 image 函数。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]没有可用的替代功能。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TEXTVALID ( 'table.column' ,text_ ptr )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *table*  
 要使用的表的名称。  
  
 *column*  
 要使用的列的名称。  
  
 text_ptr  
 要检查的文本指针。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 如果指针有效则返回 1，无效则返回 0。 请注意，text 列的标识符必须包含表名。 在没有有效的文本指针的情况下，不能使用 UPDATETEXT、WRITETEXT 或 READTEXT。  
  
 当使用 text、ntext 和 image 数据时，下列函数和语句也非常有用。  
  
|函数或语句|描述|  
|---------------------------|-----------------|  
|PATINDEX **(** ' _%pattern%_ ' **,** _expression_ **)**|返回指定字符串在 text 和 ntext 列中所处的字符位置。|  
|DATALENGTH(expression)|返回 text、ntext 和 image 列中数据的长度。|  
|SET TEXTSIZE|返回使用 SELECT 语句时返回的 text、ntext 或 image 数据的限制（字节）。|  
  
## <a name="examples"></a>示例  
 以下示例报告是否存在用于 `logo` 表的 `pub_info` 列中的各个值的有效文本指针。  
  
> [!NOTE]  
>  若要运行此示例，必须安装 pubs 数据库。  
  
```sql
USE pubs;  
GO  
SELECT pub_id, 'Valid (if 1) Text data'   
   = TEXTVALID ('pub_info.logo', TEXTPTR(logo))   
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id Valid (if 1) Text data   
------ ----------------------   
0736   1                        
0877   1                        
1389   1                        
1622   1                        
1756   1                        
9901   1                        
9952   1                        
9999   1                        
  
(8 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
 [SET TEXTSIZE (Transact-SQL)](../../t-sql/statements/set-textsize-transact-sql.md)   
 [文本与图像函数 (Transact-SQL)](./text-and-image-functions-textptr-transact-sql.md)   
 [TEXTPTR (Transact-SQL)](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)  
  
