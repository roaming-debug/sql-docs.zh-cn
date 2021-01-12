---
description: Data-tier Application Tables - sysdac_history_internal
title: sysdac_history_internal (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdac_history_internal
- sysdac_history_internal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdac_history_internal
ms.assetid: 774a1678-0b27-42be-8adc-a6d7a4a56510
author: cawrites
ms.author: chadam
ms.openlocfilehash: eaea0060136a928e6fab1184c9fd0c08d88df09f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094886"
---
# <a name="data-tier-application-tables---sysdac_history_internal"></a>Data-tier Application Tables - sysdac_history_internal
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含为管理数据层应用程序 (DAC) 而执行的操作的相关信息。 该表存储在 **msdb** 数据库的 **dbo** 架构中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**action_id**|**int**|操作的标识符|  
|**sequence_id**|**int**|标识某一操作内的步骤。|  
|**instance_id**|**uniqueidentifier**|DAC 实例的标识符。 此列可在 [dbo.sysdac_instances &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)的 **instance_id** 列上联接。|  
|**action_type**|**tinyint**|操作类型的标识符：<br /><br /> **0** = 部署<br /><br /> **1** = 创建<br /><br /> **2** = 重命名<br /><br /> **3** = 分离<br /><br /> **4** = 删除|  
|**action_type_name**|**varchar (19)**|操作类型的名称：<br /><br /> **部署：**<br /><br /> **create**<br /><br /> **rename**<br /><br /> **取出**<br /><br /> **delete**|  
|**dac_object_type**|**tinyint**|受操作影响的对象类型的标识符：<br /><br /> **0** = dacpac<br /><br /> **1** = 登录名<br /><br /> **2** = 数据库|  
|**dac_object_type_name**|**varchar (8)**|受操作影响的对象类型的名称：<br /><br /> **dacpac** = DAC 实例<br /><br /> **login**<br /><br /> **database**|  
|**action_status**|**tinyint**|标识当前操作状态的代码：<br /><br /> **0** = 挂起<br /><br /> **1** = 成功<br /><br /> **2** = 失败|  
|**action_status_name**|**varchar (11)**|操作的当前状态：<br /><br /> **未**<br /><br /> **辉煌**<br /><br /> **失败**|  
|**必需**|**bit**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]在回滚 DAC 操作时使用。|  
|**dac_object_name_pretran**|**sysname**|在提交包含操作的事务前对象的名称。 仅用于数据库和登录名。|  
|**dac_object_name_posttran**|**sysname**|在提交包含操作的事务后对象的名称。 仅用于数据库和登录名。|  
|**sqlscript**|**nvarchar(max)**|对数据库或登录名实现操作的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。|  
|**负载**|**varbinary(max)**|在二进制编码字符串中保存的 DAC 包定义。|  
|**注释**|**varchar(max)**|记录接受了 DAC 升级中的潜在数据损失的用户的登录名。|  
|**error_string**|**nvarchar(max)**|在操作遇到错误时生成的错误消息。|  
|**created_by**|**sysname**|启动了创建此条目的操作的登录名。|  
|**date_created**|**datetime**|该条目的创建日期和时间。|  
|**date_modified**|**datetime**|最后修改该条目的日期和时间。|  
  
## <a name="remarks"></a>备注  
 DAC 管理操作（例如部署或删除 DAC）会产生多个步骤。 为每个操作都分配一个操作标识符。 为每个步骤分配一个序列号和 **sysdac_history_internal** 中的行，其中记录了该步骤的状态。 在该操作步骤开始时创建每一行，并且根据需要进行更新以便反映该操作的状态。 例如，可以将 "部署 DAC" 操作分配 **action_id** 12，并在 **sysdac_history_internal** 中获取四行：  
  
| action_id | sequence_id | action_type_name | dac_object_type_name |
| --------- | ----------- | ---------------- | -------------------- |
|12|0|create|dacpac|  
|12|1|create|login|  
|12|2|create|database|  
|12|3|重命名|database|  
  
 DAC 操作（如删除）不会从 **sysdac_history_internal** 中移除行。 使用以下查询可以手动删除在[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例上不再部署的 DAC 行：  
  
```sql  
DELETE FROM msdb.dbo.sysdac_history_internal  
WHERE instance_id NOT IN  
   (SELECT instance_id  
    FROM msdb.dbo.sysdac_instances_internal);  
```  
  
 为处于活动状态的 DAC 删除行并不影响 DAC 操作；唯一影响是您将不能报告 DAC 的完整历史记录。  
  
> [!NOTE]  
>  目前，没有删除上 **sysdac_history_internal** 行的机制 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色的成员身份。 对此视图的只读访问权限可用于具有连接到 master 数据库的权限的所有用户。  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [ &#40;Transact-sql 的 dac_instancesdbo.sys&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)   
 [sysdac_instances_internal &#40;Transact-sql&#41;](../../relational-databases/system-tables/data-tier-application-tables-sysdac-instances-internal.md)  
  
  
