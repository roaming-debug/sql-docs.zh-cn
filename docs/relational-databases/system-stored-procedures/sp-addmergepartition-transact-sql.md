---
description: sp_addmergepartition (Transact-SQL)
title: sp_addmergepartition (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepartition
- sp_addmergepartition_TSQL
helpviewer_keywords:
- sp_addmergepartition
ms.assetid: 02a5f46b-e5ff-4932-a3ff-7f0fd82d0981
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5129bcd9ee63b0c1e074294c8b69f997432240f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192332"
---
# <a name="sp_addmergepartition-transact-sql"></a>sp_addmergepartition (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为订阅创建动态筛选分区，该分区按订阅服务器上 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 或 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 的值进行筛选。 此存储过程是在发布服务器上发布的数据库中执行的，用于手动生成分区。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergepartition [ @publication = ] 'publication'  
        , [ @suser_sname = ] 'suser_sname'  
        , [ @host_name = ] 'host_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 是在其上创建分区的合并发布。 *发布* 为 **sysname**，无默认值。 如果指定 *suser_sname* ，则 *hostname* 的值必须为 NULL。  
  
`[ @suser_sname = ] 'suser_sname'` 为订阅创建分区时使用的值，该订阅按订阅服务器上 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 函数的值进行筛选。 *suser_sname* **sysname**，无默认值。  
  
`[ @host_name = ] 'host_name'` 为订阅创建分区时使用的值，该订阅按订阅服务器上 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值进行筛选。 *host_name* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addmergepartition** 用于合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-addmergepartition-tra_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergepartition**。  
  
## <a name="see-also"></a>另请参阅  
 [为包含参数化筛选器的合并发布创建快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
