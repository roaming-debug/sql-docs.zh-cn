---
description: DROP EXTERNAL FILE FORMAT (Transact-SQL)
title: DROP EXTERNAL FILE FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8cf9009b-59f9-4aac-bef1-dcf2cf0708b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29f1a3ec286185c46e6f1166030c16ace467d0ab
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752877"
---
# <a name="drop-external-file-format-transact-sql"></a>DROP EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  删除 PolyBase 外部文件格式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Drop an external file format  
DROP EXTERNAL FILE FORMAT external_file_format_name  
[;]  
```  
  
## <a name="arguments"></a>参数  
 *external_file_format_name*  
 要删除的外部文件格式名。  
  
## <a name="metadata"></a>元数据  
 要查看外部文件格式列表，请使用 [sys.external_file_formats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md) 系统视图。  
  
```sql  
SELECT * FROM sys.external_file_formats;  
```  
  
## <a name="permissions"></a>权限  
 需要 ALTER ANY EXTERNAL FILE FORMAT。  
  
## <a name="general-remarks"></a>一般备注  
 删除外部文件格式不会删除外部数据。  
  
## <a name="locking"></a>锁定  
 对外部文件格式对象采用共享锁。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-basic-syntax"></a>A. 使用基本语法  
  
```sql  
DROP EXTERNAL FILE FORMAT myfileformat;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)  
  
  

