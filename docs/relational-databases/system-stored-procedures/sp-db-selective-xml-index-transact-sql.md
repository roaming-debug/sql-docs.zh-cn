---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL)
ms.custom: ''
ms.date: 02/11/2021
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
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed30d2b8df6141b308d32dee8f2fdcec0c2eceb1
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525157"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库上启用和禁用选择性 XML 索引功能。 如果不带任何参数调用，则当在特定数据库上启用选择性 XML 索引时，存储过程返回 1。  

> [!NOTE]  
> 从开始 [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] ，不能禁用选择性 XML 索引功能。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 在中 [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] ，若要使用此存储过程禁用选择性 XML 索引功能，必须使用 [ALTER Database SET Options &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) 命令将数据库置于简单恢复模式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
      sys.sp_db_selective_xml_index[[ @dbname = ] 'dbname'],   
[[ @selective_xml_index = ] 'selective_xml_index']  
```  
  
## <a name="arguments"></a>自变量  
`[ @ dbname = ] 'dbname'` 要对其启用或禁用选择性 XML 索引的数据库的名称。 如果 *dbname* 为 NULL，则假定为当前数据库。 *@dbname* 为 **sysname**。


`[ @selective_xml_index = ] 'selective_xml_index'` 确定是启用还是禁用索引。 允许的值： "on"、"off"、"true"、"false"。 如果传递了除 "on"、"true"、"off" 或 "false" 之外的其他值，则会引发错误。 *@selective_xml_index* 是 **varchar (6)**。

  
## <a name="return-code-values"></a>返回代码值  
 如果在特定数据库上启用选择性 XML 索引，则为 **1** ; 否则为 **0** 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enable-selective-xml-index-functionality"></a>A. 启用选择性 XML 索引功能  
 下面的示例在当前数据库上启用选择性 XML 索引。  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'on';  
GO  
```  
  
 下面的示例在 AdventureWorks2012 数据库上启用选择性 XML 索引。  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. 禁用选择性 XML 索引功能  
 下面的示例在当前数据库上禁用选择性 XML 索引。  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'off';  
GO  
```  
  
 下面的示例在 AdventureWorks2012 数据库上禁用选择性 XML 索引。  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. 检测是否启用了选择性 XML 索引  
 下面的示例检测是否启用了选择性 XML 索引。 如果启用选择性 XML 索引，则返回 1。  
  
```sql
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
   