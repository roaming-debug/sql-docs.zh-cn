---
description: sp_dropdistributor (Transact-SQL)
title: sp_dropdistributor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistributor
- sp_dropdistributor_TSQL
helpviewer_keywords:
- sp_dropdistributor
ms.assetid: 0644032f-5ff0-4718-8dde-321bc9967a03
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7a08c3f8cd035db38533f9b7fd56aaa34c39399
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205152"
---
# <a name="sp_dropdistributor-transact-sql"></a>sp_dropdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  卸载分发服务器。 此存储过程在分发服务器上除分发数据库之外的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropdistributor [ [ @no_checks= ] no_checks ]   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>参数  
`[ @no_checks = ] no_checks` 指示在删除分发服务器之前是否检查依赖对象。 *no_checks* 为 **bit**，默认值为0。  
  
 如果为 **0**，则 **sp_dropdistributor** 检查以确保除分发服务器以外的所有发布和分发对象均已删除。  
  
 如果为 **1**，则在卸载分发服务器之前 **sp_dropdistributor** 删除所有发布和分发对象。  
  
`[ @ignore_distributor = ] ignore_distributor` 指示是否在未连接到分发服务器的情况下执行此存储过程。 *ignore_distributor* 为 **bit**，默认值为 **0**。  
  
 如果为 **0**，则 **Sp_dropdistributor** 连接到分发服务器并删除所有复制对象。 如果 **sp_dropdistributor** 无法连接到分发服务器，则存储过程将失败。  
  
 如果为 **1**，则不会与分发服务器建立连接，并且不会删除复制对象。 如果分发服务器正在卸载或持久脱机，才使用它。 直到分发服务器在未来某个时间重新安装之后，才会删除分发服务器中的该发布服务器的对象。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropdistributor** 在所有类型的复制中使用。  
  
 如果服务器上存在其他发布服务器或分发对象， **sp_dropdistributor** 将失败，除非 **\@ no_checks** 设置为 **1**。  
  
 在通过执行 **sp_dropdistributiondb** 删除分发数据库后，必须执行此存储过程。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributor-trans_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_dropdistributor** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)   
 [sp_changedistributor_property &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
