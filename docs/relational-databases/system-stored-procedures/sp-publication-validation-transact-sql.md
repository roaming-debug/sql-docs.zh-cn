---
description: sp_publication_validation (Transact-SQL)
title: sp_publication_validation (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_publication_validation
- sp_publication_validation_TSQL
helpviewer_keywords:
- sp_publication_validation
ms.assetid: 06be2363-00c0-4936-97c1-7347f294a936
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7e785892f73b45b6e5f6f20944c9c15ee88e8f63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199574"
---
# <a name="sp_publication_validation-transact-sql"></a>sp_publication_validation (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为指定发布中的每个项目启动项目验证请求。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_publication_validation [ @publication = ] 'publication'  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @rowcount_only = ] 'rowcount_only'` 指示是否只返回表的行计数。 *rowcount_only* 为 **smallint** ，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|执行与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 兼容的校验和。<br /><br /> 注意：水平筛选项目时，将执行行计数操作，而不是校验和操作。|  
|**1** （默认值）|仅执行行计数检查。|  
|**2**|执行行计数和二进制校验和。<br /><br /> 注意：对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本7.0 订阅服务器，只执行行计数验证。|  
  
`[ @full_or_fast = ] 'full_or_fast'` 用于计算行计数的方法。 *full_or_fast* 为 **tinyint** ，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|使用 COUNT(*) 进行完整计数。|  
|**1**|从 **sysindexes** 中快速计数。 计算 [sys.sys索引](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) 中的行比计算实际表中的行的速度要快得多。 但是，因为 [sys.sys索引](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) 被延迟更新，所以行计数可能不准确。|  
|**2** （默认值）|首先尝试使用快速方法进行条件性快速计数。 如果快速方法显示出差异，则转而使用完整方法。 如果 *expected_rowcount* 为 NULL，并且存储过程用于获取该值，则始终使用 ( * ) 的完整计数。|  
  
`[ @shutdown_agent = ] 'shutdown_agent'` 指示分发代理是否应在完成验证后立即关闭。 *shutdown_agent* 为 **bit**，默认值为 **0**。 如果为 **0**，则复制代理不会关闭。 如果为 **1**，则复制代理会在最后一篇文章验证后关闭。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在发布服务器上请求验证时，不应使用 *发布服务器* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_publication_validation** 用于事务复制。  
  
 激活与发布关联的项目后，可以随时调用 **sp_publication_validation** 。 可以手动运行一次此过程，或将其作为验证数据的定期计划作业的一部分。  
  
 如果你的应用程序具有即时更新订阅服务器， **sp_publication_validation** 可能会检测到虚假错误。 **sp_publication_validation** 首先计算发布服务器上的行计数或校验和，然后在订阅服务器上计算。 由于在发布服务器中完成行计数或校验和之后，但在订阅服务器中完成行计数或校验和之前，即时更新触发器可以将更新从订阅服务器传播到发布服务器，因此值可能会更改。 若要确保在验证发布时，在订阅服务器和发布服务器中的值不发生更改，请在验证期间在发布服务器中停止 Microsoft 分布式事务处理协调器 (MS DTC) 服务。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_publication_validation**。  
  
## <a name="see-also"></a>另请参阅  
 [验证订阅服务器上的数据](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_article_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_table_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
