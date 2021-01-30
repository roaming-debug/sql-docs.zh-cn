---
description: sys.sysobjects (Transact-SQL)
title: " (Transact-sql) 的 sys.sys对象 |Microsoft Docs"
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysobjects_TSQL
- sysobjects
- sysobjects_TSQL
- sys.sysobjects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysobjects compatibility view
- sysobjects system table
ms.assetid: 44fdc387-67b0-4139-8bf5-ed26cf640cd1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 36c8f1ad3d1d90bedd5a42f93add11a8dfdb2770
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158521"
---
# <a name="syssysobjects-transact-sql"></a>sys.sysobjects (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  在数据库中创建的每个对象（例如约束、默认值、日志、规则以及存储过程）都对应一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|对象名称|  
|id|**int**|对象标识号|  
|xtype|**char(2)**|对象类型。 可以是以下对象类型之一：<br /><br /> AF = 聚合函数 (CLR)<br /><br /> C = CHECK 约束<br /><br /> D = 默认值或 DEFAULT 约束<br /><br /> F = FOREIGN KEY 约束<br /><br /> L = 日志<br /><br /> FN = 标量函数<br /><br /> FS = 程序集 (CLR) 标量函数<br /><br /> FT = 程序集 (CLR) 表值函数<br /><br /> IF = 内联表函数<br /><br /> IT = 内部表<br /><br /> P = 存储过程<br /><br /> PC = Assembly (CLR) 存储过程<br /><br /> PK = PRIMARY KEY 约束（类型为 K）<br /><br /> RF = 复制筛选存储过程<br /><br /> S = 系统表<br /><br /> SN = 同义词<br /><br /> SQ = 服务队列<br /><br /> TA = 程序集 (CLR) DML 触发器<br /><br /> TF = 表函数<br /><br /> TR = SQL DML 触发器<br /><br /> TT = 表类型<br /><br /> U = 用户表<br /><br /> UQ = UNIQUE 约束（类型为 K）<br /><br /> V = 视图<br /><br /> X = 扩展存储过程|  
|uid|**smallint**|对象所有者的架构 ID。 对于从旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级的数据库，架构 ID 等于所有者的用户 ID。 如果用户数和角色数超过 32,767，则发生溢出或返回 NULL。<br /><br /> **\* \* 重要 \* 提示 \*** 如果使用以下任意 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DDL 语句，则必须使用 [sys.databases](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)目录视图，而不是 sys.sys对象。<br /><br /> 创建 &#124; ALTER &#124; DROP USER<br /><br /> 创建 &#124; ALTER &#124; DROP ROLE<br /><br /> 创建 &#124; ALTER &#124; DROP 应用程序角色<br /><br /> CREATE SCHEMA<br /><br /> ALTER AUTHORIZATION ON OBJECT|  
|info|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|状态|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|base_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|replinfo|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|parent_obj|**int**|父对象的对象标识号。 例如，表 ID（如果父对象是触发器或约束）。|  
|crdate|**datetime**|对象的创建日期。|  
|ftcatid|**smallint**|注册为使用全文检索的所有用户表的全文目录标识符，对于没有注册的所有用户表则为 0。|  
|schema_ver|**int**|在每次更改表的架构时都会增加的版本号。 始终返回 0。|  
|stats_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|type|**char(2)**|对象类型。 可以是以下其中一个值：<br /><br /> AF = 聚合函数 (CLR)<br /><br /> C = CHECK 约束<br /><br /> D = 默认值或 DEFAULT 约束<br /><br /> F = FOREIGN KEY 约束<br /><br /> FN = 标量函数<br /><br /> FS = 程序集 (CLR) 标量函数<br /><br /> FT = 程序集 (CLR) 表值函数 IF = 内联表函数<br /><br /> IT - 内部表<br /><br /> K = PRIMARY KEY 或 UNIQUE 约束<br /><br /> L = 日志<br /><br /> P = 存储过程<br /><br /> PC = Assembly (CLR) 存储过程<br /><br /> R = 规则<br /><br /> RF = 复制筛选存储过程<br /><br /> S = 系统表<br /><br /> SN = 同义词<br /><br /> SQ = 服务队列<br /><br /> TA = 程序集 (CLR) DML 触发器<br /><br /> TF = 表函数<br /><br /> TR = SQL DML 触发器<br /><br /> TT = 表类型<br /><br /> U = 用户表<br /><br /> V = 视图<br /><br /> X = 扩展存储过程|  
|userstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|sysstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|indexdel|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|refdate|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|版本|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|deltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|instrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|updtrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|seltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|category|**int**|用于发布、约束和标识。|  
|cache|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
