---
description: sp_schemafilter (Transact-SQL)
title: sp_schemafilter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_schemafilter_TSQL
- sp_schemafilter
helpviewer_keywords:
- sp_schemafilter
ms.assetid: 199e869b-2cd2-44ee-b2ee-69edb06a1bc4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f5b05b9976525109f6361c68f7acf7ee309f8b85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189998"
---
# <a name="sp_schemafilter-transact-sql"></a>sp_schemafilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  修改和显示在列出适于发布的 Oracle 表时排除的架构的相关信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_schemafilter [ @publisher = ] 'publisher'   
   [ , [ @schema = ] 'schema' ]   
   [ , [ @operation = ] 'operation' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @schema = ] 'schema'` 架构的名称。 *架构* 的值为 **sysname**，默认值为 NULL。  
  
`[ @operation = ] 'operation'` 要对此架构采取的操作。 *操作* 为 **nvarchar (4)**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**add**|将指定架构添加到不适合发布的架构列表中。|  
|**击落**|从不适合发布的架构列表中删除指定架构。|  
|**help**|返回不适合发布的架构列表。|  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**schemaname**|**sysname**|不适合发布的架构名称。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_schemafilter** 应仅用于异类发布服务器。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员才能 **sp_schemafilter** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
