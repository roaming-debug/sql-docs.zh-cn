---
description: REFERENTIAL_CONSTRAINTS (Transact-SQL)
title: REFERENTIAL_CONSTRAINTS (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- REFERENTIAL_CONSTRAINTS
- REFERENTIAL_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS view
- REFERENTIAL_CONSTRAINTS view
ms.assetid: 5d358f18-0a85-4b55-af4b-98d5f4cd1020
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e334104bdc80793bfbfe841f5e5912582d8282e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484539"
---
# <a name="referential_constraints-transact-sql"></a>REFERENTIAL_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为当前数据库中的每个 FOREIGN KEY 约束返回一行。 该信息架构视图返回当前用户对其拥有权限的对象的相关信息。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|约束限定符。|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|包含该约束的架构的名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**CONSTRAINT_NAME**|**sysname**|约束名称。|  
|**UNIQUE_CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|UNIQUE 约束限定符。|  
|**UNIQUE_CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|包含 UNIQUE 约束的架构的名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**UNIQUE_CONSTRAINT_NAME**|**sysname**|UNIQUE 约束。|  
|**MATCH_OPTION**|**varchar (** 7 **)**|引用约束匹配条件。 始终返回 SIMPLE。 这表示没有定义匹配。 当下列情况之一为真时，条件被视为匹配：<br /><br /> 外键列中至少有一个值为 NULL。<br /><br /> 外键列中的所有值都不是 NULL，并且主键表中有一行含有相同的键。|  
|**UPDATE_RULE**|**varchar (** 11 **)**|当 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句违反该约束所定义的引用完整性时执行的操作。 返回下列项之一： <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> 如果为该约束在 ON UPDATE 上指定了 NO ACTION，则对该约束中被引用主键的更新不会传播到外键。 如果因为至少有一个外键包含相同的值，而导致主键的更新违反了引用完整性，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不会对父表和引用表执行任何更改。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还将引发错误。<br /><br /> 如果为该约束在 ON UPDATE 上指定了 CASCADE，则对主键值所做的任何更改都将自动地传播到外键值。|  
|**DELETE_RULE**|**varchar (** 11 **)**|当 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句违反了该约束所定义的引用完整性时执行的操作。 返回下列项之一： <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> 如果为该约束在 ON DELETE 上定义了 NO ACTION，则对该约束中被引用主键所做的删除将不会传播到外键。 如果因为至少有一个外键包含相同的值而导致主键的删除违反了引用完整性，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不会对父表和引用表执行任何更改。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还将引发错误。<br /><br /> 如果为该约束在 ON DELETE 上指定了 CASCADE，则对主键值所做的任何更改都将自动传播到外键值。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.foreign_keys &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)  
  
