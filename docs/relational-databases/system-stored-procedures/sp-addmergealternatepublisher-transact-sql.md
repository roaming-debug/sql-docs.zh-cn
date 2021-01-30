---
description: sp_addmergealternatepublisher (Transact-SQL)
title: sp_addmergealternatepublisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergealternatepublisher_TSQL
- sp_addmergealternatepublisher
helpviewer_keywords:
- sp_addmergealternatepublisher
ms.assetid: de46e0b1-d946-4021-bff6-2d8e3187656d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7c0caf60fb3b1323c24924f177ca4d8cdf1105ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202356"
---
# <a name="sp_addmergealternatepublisher-transact-sql"></a>sp_addmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为订阅服务器添加使用备用同步伙伴的功能。 发布属性必须指定订阅服务器可以与其他发布服务器同步。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergealternatepublisher [ @publisher= ] 'publisher'  
          , [ @publisher_db= ] 'publisher_db'  
          , [ @publication= ] 'publication'  
          , [ @alternate_publisher= ] 'alternate_synchronization_partner'  
          , [ @alternate_publisher_db= ] 'alternate_publisher_db'  
          , [ @alternate_publication= ] 'alternate_synchronization_partner'  
     , [ @alternate_distributor= ] 'alternate_distributor'   
    [ , [ @friendly_name= ] 'friendly_name' ]   
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @alternate_publisher = ] 'alternate_synchronization_partner'` 备用发布服务器的名称。 *alternate_synchronization_partner* **sysname**，无默认值。  
  
`[ @alternate_publisher_db = ] 'alternate_publisher_db'` 备用发布服务器上发布数据库的名称。 *alternate_publisher_db* **sysname**，无默认值。  
  
`[ @alternate_publication = ] 'alternate_synchronization_partner'` 备用同步伙伴上的发布的名称。 *alternate_synchronization_partner* **sysname**，无默认值。  
  
`[ @alternate_distributor = ] 'alternate_distributor'` 备用同步伙伴的分发服务器的名称。 *alternate_distributor* **sysname**，无默认值。  
  
`[ @friendly_name = ] 'friendly_name'` 是一个显示名称，通过该名称可以标识构成备用同步伙伴的发布服务器、发布和分发服务器之间的关联。 *friendly_name* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addmergealternatepublisher** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergealternatepublisher**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_dropmergealternatepublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergealternatepublisher-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
