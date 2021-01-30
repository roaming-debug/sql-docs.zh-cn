---
description: sp_changesubscriptiondtsinfo (Transact-SQL)
title: sp_changesubscriptiondtsinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriptiondtsinfo
- sp_changesubscriptiondtsinfo_TSQL
helpviewer_keywords:
- sp_changesubscriptiondtsinfo
ms.assetid: 64fc085f-f81b-493b-b59a-ee6192d9736d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 63762571085653afea03470cfc1e453a85e8adad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194415"
---
# <a name="sp_changesubscriptiondtsinfo-transact-sql"></a>sp_changesubscriptiondtsinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改订阅的 Data Transformation Services (DTS) 包属性。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changesubscriptiondtsinfo [ [ @job_id = ] job_id ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 推送订阅的分发代理的作业 ID。 *job_id* 为 **varbinary (16)**，无默认值。 若要查找分发作业 ID，请运行 **sp_helpsubscription** 或 **sp_helppullsubscription**。  
  
`[ @dts_package_name = ] 'dts_package_name'` 指定 DTS 包的名称。 *dts_package_name* 是 **sysname**，默认值为 NULL。 例如，若要指定名为 **DTSPub_Package** 的包，请指定 `@dts_package_name = N'DTSPub_Package'` 。  
  
`[ @dts_package_password = ] 'dts_package_password'` 指定包的密码。 *dts_package_password* 的默认 **值为 NULL** ，默认值为 NULL，指定密码属性保持不变。  
  
> [!NOTE]  
>  DTS 包必须具有密码。  
  
`[ @dts_package_location = ] 'dts_package_location'` 指定包位置。 *dts_package_location* 为 **nvarchar (12)**，默认值为 NULL，该值指定包位置保持不变。 包的位置可以更改为 **分发服务器** 或 **订阅服务器**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changesubscriptiondtsinfo** 用于仅限推送订阅的快照复制和事务复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或订阅的创建者才能执行 **sp_changesubscriptiondtsinfo**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
