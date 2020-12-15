---
description: sys.assemblies (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assemblies
- assemblies_TSQL
- sys.assemblies_TSQL
- assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assemblies catalog view
ms.assetid: e321753f-293f-42ab-b225-d118713df40b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab5b41aa87c9b3e200ac200db1c46b8754081ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97459912"
---
# <a name="sysassemblies-transact-sql"></a>sys.assemblies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  为每个程序集返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|程序集的名称。 在该数据库中是唯一的。|  
|principal_id|**int**|此程序集所属主体的 ID。|  
|**assembly_id**|**int**|程序集的标识号。 在数据库中是唯一的。|  
|**clr_name**|**nvarchar(4000)**|对程序集的简单名称、版本号、区域性、公钥以及体系结构进行编码的规范字符串。 该值唯一地标识公共语言运行时 (CLR) 端的程序集。|  
|**permission_set**|**tinyint**|程序集的权限集/安全级别。<br /><br /> 1 = 安全访问<br /><br /> 2 = 外部访问<br /><br /> 3 = 不安全的访问|  
|**permission_set_desc**|**nvarchar(60)**|程序集的权限集/安全级别的说明。<br /><br /> SAFE_ACCESS<br /><br /> EXTERNAL_ACCESS<br /><br /> UNSAFE_ACCESS|  
|**is_visible**|**bit**|1 = 程序集对于注册表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 入口点可见。<br /><br /> 0 = 程序集只用于受管调用方。 即，程序集提供数据库中其他程序集的内部实现。|  
|create_date|**datetime**|程序集创建或注册的日期。|  
|modify_date|**datetime**|程序集修改的日期。|  
|**is_user_defined**|**bit**|指示程序集的源。<br /><br /> 0 = **hierarchyid** 数据类型的系统定义的程序集 (如) <br /><br /> 1 = 用户定义的程序集|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 CLR 程序集目录视图 ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ASSEMBLYPROPERTY &#40;Transact-sql&#41;](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
