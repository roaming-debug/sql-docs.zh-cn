---
description: sp_helpxactsetjob (Transact-SQL)
title: sp_helpxactsetjob (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpxactsetjob
- sp_helpxactsetjob_TSQL
helpviewer_keywords:
- sp_helpxactsetjob
ms.assetid: 242cea3e-e6ac-4f84-a072-b003b920eb33
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39092ea96ed0f143af7322fea3f04d38d3e4eef0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194731"
---
# <a name="sp_helpxactsetjob-transact-sql"></a>sp_helpxactsetjob (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示有关 Oracle 发布服务器的 Xactset 作业的信息。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpxactsetjob [ @publisher = ] 'publisher'   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]作业所属的非发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**jobnumber**|**int**|Oracle 作业编号。|  
|**lastdate**|**varchar (22)**|作业运行的最后日期。|  
|**thisdate**|**varchar (22)**|更改的时间。|  
|**nextdate**|**varchar (22)**|作业将要运行的下一个日期。|  
|**broken**|**varchar(1)**|指示作业是否中断的标志。|  
|**interval**|**varchar (200)**|作业的间隔。|  
|**故障**|**int**|作业失败的次数。|  
|**xactsetjobwhat**|**varchar (200)**|作业执行的过程的名称。|  
|**xactsetjob**|**varchar(1)**|作业的状态，可以是以下状态之一：<br /><br /> **1** -作业已启用。<br /><br /> **0** -作业已禁用。|  
|**xactsetlonginterval**|**int**|作业的长间隔。|  
|**xactsetlongthreshold**|**int**|作业的长阈值。|  
|**xactsetshortinterval**|**int**|作业的短间隔。|  
|**xactsetshortthreshold**|**int**|作业的短阈值。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpxactsetjob** 用于 Oracle 发布服务器的快照复制和事务复制。  
  
 **sp_helpxactsetjob** 始终返回发布服务器上的 Xactset 作业 (HREPL_XactSetJob) 的当前设置。 如果 Xactset 作业当前在作业队列中，它还会从 USER_JOB 数据字典视图（创建在 Oracle 发布服务器的管理员帐户下）返回作业的属性。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_helpxactsetjob** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [为 Oracle 发布服务器配置事务集作业（复制 Transact-SQL 编程）](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [sp_publisherproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-publisherproperty-transact-sql.md)  
  
  
