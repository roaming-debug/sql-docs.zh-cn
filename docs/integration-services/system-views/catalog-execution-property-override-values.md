---
description: catalog.execution_property_override_values
title: catalog.execution_property_override_values | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 83cbdd6f-ddde-47bf-abde-36bd24272621
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 230e2057c8257a37eb96c683abaf8348307a0cba
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129459"
---
# <a name="catalogexecution_property_override_values"></a>catalog.execution_property_override_values 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示在包执行期间设置的属性覆盖值。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|property_id|**bigint**|属性覆盖值的唯一 ID。|  
|execution_id|**bigint**|执行实例的唯一 ID。|  
|property_path|**nvarchar(4000)**|指向包中属性的路径。|  
|property_value|**nvarchar(max)**|属性的覆盖值。|  
|sensitive|**bit**|当值为 1 时，属性是敏感的并在存储时加密。 当值为 0 时，属性是不敏感的并以纯文本形式存储值。|  
  
## <a name="remarks"></a>备注  
 此视图为每次执行显示一行，在每次执行中都使用“执行包”对话框上“高级”选项卡中的“属性覆盖”部分覆盖属性值。 该属性的路径从包任务的“包路径”属性派生。  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对执行实例的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
