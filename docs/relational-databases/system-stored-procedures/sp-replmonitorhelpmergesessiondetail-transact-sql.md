---
title: 'sp_replmonitorhelpmergesessiondetail (T-sql) '
description: 介绍 sp_replmonitorhelpmergesessiondetail 存储过程，该存储过程返回有关特定复制合并代理会话的详细文章级信息。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpmergesessiondetail
- sp_replmonitorhelpmergesessiondetail_TSQL
helpviewer_keywords:
- sp_replmonitorhelpmergesessiondetail
ms.assetid: 805c92fc-3169-410c-984d-f37e063b791d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba050d49579e217fa5be5a2dedcd7aa21cf0c99d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204376"
---
# <a name="sp_replmonitorhelpmergesessiondetail-transact-sql"></a>sp_replmonitorhelpmergesessiondetail (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回有关特定复制合并代理会话的项目级详细信息，该会话用于监视合并复制。 结果集包括在会话过程中被同步的每个项目的详细信息行。 另外还包括提供会话初始化的行以及汇总上载和下载会话阶段的行。 此存储过程针对分发服务器的分发数据库或订阅服务器的订阅数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorhelpmergesessiondetail [ @session_id = ] session_id  
```  
  
## <a name="arguments"></a>参数  
`[ @session_id = ] session_id` 指定一个代理会话。 *session_id* 为 **int** ，无默认值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**PhaseID**|**int**|同步会话的阶段，可以是下列值之一：<br /><br /> **0** = 初始化或汇总行<br /><br /> **1** = 上载<br /><br /> **2** = 下载|  
|**ArticleName**|**sysname**|被同步的项目的名称。 **ArticleName** 还包含结果集中不表示项目详细信息的行的汇总信息。|  
|**PercentComplete**|**decimal**|指示当前正在运行或已失败的会话中，给定的项目详细信息行中应用的全部更改的百分比。|  
|**RelativeCost**|**decimal**|指示同步项目所花时间占会话的同步总时间的百分比。|  
|**持续时间**|**int**|代理会话的长度。|  
|**Inserts**|**int**|会话中的插入数。|  
|**更新**|**int**|会话中的更新数。|  
|**Deletes**|**int**|会话中的删除数。|  
|**冲突**|**int**|会话中发生的冲突数。|  
|**ErrorID**|**int**|会话错误的 ID。|  
|**SeqNo**|**int**|结果集中会话的顺序。|  
|**RowType**|**int**|指示结果集中的每一行所提供的信息类型。<br /><br /> **0** = 初始化<br /><br /> **1** = 上载摘要<br /><br /> **2** = 项目上传详细信息<br /><br /> **3** = 下载摘要<br /><br /> **4** = 项目下载详细信息|  
|**SchemaChanges**|**int**|会话中的架构更改数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorhelpmergesessiondetail** 用于监视合并复制。  
  
 在订阅服务器上执行时， **sp_replmonitorhelpmergesessiondetail** 仅返回最近5合并代理会话的详细信息。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上的分发数据库或订阅服务器上的 **replmonitor** 固定数据库角色的 **db_owner** 成员才能执行 **sp_replmonitorhelpmergesessiondetail**。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
