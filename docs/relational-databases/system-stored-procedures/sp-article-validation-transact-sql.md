---
description: sp_article_validation (Transact-SQL)
title: sp_article_validation (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_article_validation_TSQL
- sp_article_validation
helpviewer_keywords:
- sp_article_validation
ms.assetid: 44e7abcd-778c-4728-a03e-7e7e78d3ce22
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1b2dda5e359b062716c9c0fead89b4ad2628488a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203230"
---
# <a name="sp_article_validation-transact-sql"></a>sp_article_validation (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为指定项目启动数据验证请求。 此存储过程在发布服务器上的发布数据库中执行，或者在订阅服务器上的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_article_validation [ @publication = ] 'publication'  
    [ , [ @article = ] 'article' ]  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @subscription_level = ] subscription_level ]  
    [ , [ @reserved = ] reserved ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 项目所在的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 要验证的项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @rowcount_only = ] type_of_check_requested` 指定是否只返回表的行计数。 *type_of_check_requested* 为 **smallint**，默认值为 **1**。  
  
 如果为 **0**，则执行行计数和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 兼容的校验和。  
  
 如果为 **1**，则仅执行行计数检查。  
  
 如果为 **2**，则执行行计数和二进制校验和。  
  
`[ @full_or_fast = ] full_or_fast` 用于计算行计数的方法。 *full_or_fast* 为 **tinyint**，可以是下列值之一。  
  
|**值**|**说明**|  
|---------------|---------------------|  
|**0**|用 COUNT(*) 执行完整计数。|  
|**1**|从 **sysindexes** 执行快速计数。 计算 **sysindexes** 中的行数比计算实际表中的行的速度更快。 但是， **sysindexes** 会延迟更新，行计数可能不准确。|  
|**2** （默认值）|首先尝试使用快速方法执行条件性快速计数。 如果快速方法显示出差异，则转而使用完整方法。 如果 *expected_rowcount* 为 NULL，并且存储过程用于获取该值，则始终使用 ( * ) 的完整计数。|  
  
`[ @shutdown_agent = ] shutdown_agent` 指定分发代理是否应在完成验证后立即关闭。 *shutdown_agent* 为 **bit**，默认值为 **0**。 如果为 **0**，则分发代理不会关闭。 如果为 **1**，则分发代理在验证项目后关闭。  
  
`[ @subscription_level = ] subscription_level` 指定是否由一组订阅服务器选取验证。 *subscription_level* 为 **bit**，默认值为 **0**。 如果为 **0**，则验证应用于所有订阅服务器。 如果为 **1**，则仅将验证应用于对当前打开事务中 **sp_marksubscriptionvalidation** 的调用指定的订阅服务器的子集。  
  
`[ @reserved = ] reserved` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在发布服务器上请求验证时，不应使用 *发布服务器* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_article_validation** 用于事务复制。  
  
 **sp_article_validation** 会导致在指定项目上收集验证信息，并将验证请求发送到事务日志。 分发代理接收到该请求后，将该请求中的验证信息与订阅服务器表进行比较。 验证的结果显示在复制监视器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报中。  
  
## <a name="permissions"></a>权限  
 只有对所验证项目的源表具有 SELECT ALL 权限的用户才可以执行 **sp_article_validation**。  
  
## <a name="see-also"></a>另请参阅  
 [验证复制的数据](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_marksubscriptionvalidation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md)   
 [sp_publication_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [sp_table_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
