---
description: sp_helptrigger (Transact-SQL)
title: sp_helptrigger (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_helptrigger
- sp_helptrigger_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptrigger
ms.assetid: e486d39b-771d-488d-a786-7136433a2203
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8b03afedd01095ddfc233eca9722e9eecde8f840
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468358"
---
# <a name="sp_helptrigger-transact-sql"></a>sp_helptrigger (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回对当前数据库的指定表定义的 DML 触发器的类型。 sp_helptrigger 不能与 DDL 触发器一起使用。 改为查询 [系统存储过程](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) 目录视图。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helptrigger [ @tabname = ] 'table'   
     [ , [ @triggertype = ] 'type' ]  
```  
  
## <a name="arguments"></a>自变量  
`[ @tabname = ] 'table'` 当前数据库中要为其返回触发器信息的表的名称。 *table* 为 **nvarchar (776)**，无默认值。  
  
`[ @triggertype = ] 'type'` 要返回其相关信息的 DML 触发器的类型。 *类型* 为 **char (6)**，默认值为 NULL，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**DELETE**|返回 DELETE 触发器信息。|  
|**INSERT**|返回 INSERT 触发器信息。|  
|**UPDATE**|返回 UPDATE 触发器信息。|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 下表显示了结果集中包含的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|trigger_name|**sysname**|触发器的名称。|  
|**trigger_owner**|**sysname**|对其定义触发器的表的所有者名称。|  
|**isupdate**|**int**|1=UPDATE 触发器<br /><br /> 0=不是 UPDATE 触发器|  
|**isdelete**|**int**|1=DELETE 触发器<br /><br /> 0=不是 DELETE 触发器|  
|**isinsert**|**int**|1=INSERT 触发器<br /><br /> 0=不是 INSERT 触发器|  
|**isafter**|**int**|1=AFTER 触发器<br /><br /> 0=不是 AFTER 触发器|  
|**isinsteadof**|**int**|1=INSTEAD OF 触发器<br /><br /> 0=不是 INSTEAD OF 触发器|  
|**trigger_schema**|**sysname**|触发器所属架构的名称。|  
  
## <a name="permissions"></a>权限  
 要求对表具有 [元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md) 权限。  
  
## <a name="examples"></a>示例  
 以下示例执行 `sp_helptrigger` 以生成有关对 `Person.Person` 表的触发器的信息。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptrigger 'Person.Person';  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
