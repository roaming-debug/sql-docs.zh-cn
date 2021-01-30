---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
ms.assetid: 017301a2-4a23-4e68-82af-134f3d4892b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17942cc1e4c68c844e78a2f25398a7e175035d01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201317"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库上启用和禁用选择性 XML 索引功能。 如果不带任何参数调用，则当在特定数据库上启用选择性 XML 索引时，存储过程返回 1。  
  
> [!NOTE]  
>  若要使用此存储过程禁用选择性 XML 索引，必须使用 [ALTER DATABASE SET Options &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) 命令将数据库置于简单恢复模式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
  
      sys.sp_db_selective_xml_index[[ @db_name = ] 'db_name'],   
[[ @action = ] 'action']  
```  
  
## <a name="arguments"></a>参数  
`[ @ db_name = ] 'db_name'` 要对其启用或禁用选择性 XML 索引的数据库的名称。 如果 *db_name* 为空，则假定为当前数据库。  
  
`[ @action = ] 'action'` 确定是启用还是禁用索引。 如果传递了除 "on"、"true"、"off" 或 "false" 之外的其他值，则会引发错误。  
  
```  
  
Allowed values: 'on', 'off', 'true', 'false'  
```  
  
## <a name="return-code-values"></a>返回代码值  
 如果在特定数据库上启用选择性 XML 索引，则为 **1** 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enable-selective-xml-index-functionality"></a>A. 启用选择性 XML 索引功能  
 下面的示例在当前数据库上启用选择性 XML 索引。  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'on';  
GO  
```  
  
 下面的示例在 AdventureWorks2012 数据库上启用选择性 XML 索引。  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. 禁用选择性 XML 索引功能  
 下面的示例在当前数据库上禁用选择性 XML 索引。  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'off';  
GO  
```  
  
 下面的示例在 AdventureWorks2012 数据库上禁用选择性 XML 索引。  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. 检测是否启用了选择性 XML 索引  
 下面的示例检测是否启用了选择性 XML 索引。 如果启用选择性 XML 索引，则返回 1。  
  
```  
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
  
  
