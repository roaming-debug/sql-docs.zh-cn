---
description: sp_publisherproperty (Transact-SQL)
title: sp_publisherproperty (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_publisherproperty
- sp_publisherproperty_TSQL
helpviewer_keywords:
- sp_publisherproperty
ms.assetid: 0ed1ebc1-a1bd-4aed-9f46-615c5cf07827
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 80e2e9eda3d6a4f900c4ab9f8e48dc783d23cd2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210052"
---
# <a name="sp_publisherproperty-transact-sql"></a>sp_publisherproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  显示或更改非发布服务器的发布服务器属性 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 此存储过程在分发服务器上执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_publisherproperty [ @publisher = ] 'publisher'   
   [ , [ @propertyname = ] 'propertyname' ]   
   [ , [ @propertyvalue = ] 'propertyvalue' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 异类发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @propertyname = ] 'propertyname'` 要设置的属性的名称。 *propertyname* 为 **sysname**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**xactsetbatching**|是否将发布服务器上的事务分组成在事务上一致的集合（称为 Xactset），以进行后续处理。 值为 **enabled** 表示可以创建 xactset，这是默认值。 如果值为 " **禁用** "，则表示不会创建新的 xactset 来处理现有的 xactset。|  
|**xactsetjob**|是否启用 Xactset 作业以创建 Xactset。 值为 **enabled** 表示 Xactset 作业定期运行以在发布服务器上创建 xactset。 如果值为 " **禁用** "，则表示 xactset 仅在轮询发布服务器的更改时由日志读取器代理创建。|  
|**xactsetjobinterval**|两次 Xactset 作业执行之间的间隔（分钟）。|  
  
 如果省略 *propertyname* ，则返回所有可设置的属性。  
  
 `[ @propertyvalue = ] 'propertyvalue'`  
 属性设置的新值。 *propertyvalue* 的值为 **sysname**，默认值为 NULL。 如果省略 *propertyvalue* ，则返回属性的当前设置。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|propertyname|**sysname**|返回以下可以设置的发布属性：<br /><br /> **xactsetbatching**<br /><br /> **xactsetjob**<br /><br /> **xactsetjobinterval**|  
|**propertyvalue**|**sysname**|**Propertyname** 列中的属性的当前设置。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_publisherproperty** 用于非发布服务器的事务复制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 如果仅指定了 *publisher* ，则结果集将包括所有可设置的属性的当前设置。  
  
 指定 *propertyname* 时，结果集中只显示已命名的属性。  
  
 如果指定所有参数，则属性将更改，且不返回结果集。  
  
 更改正在运行的作业的 **xactsetjobinterval** 属性时，必须重新启动该作业，以使新的时间间隔生效。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员才能 **sp_publisherproperty** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [为 Oracle 发布服务器配置事务集作业（复制 Transact-SQL 编程）](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
