---
description: catalog.execution_parameter_values（SSISDB 数据库）
title: catalog.execution_parameter_values（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ec93e67b-04ce-4aae-ab96-3ad20e9793ad
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae198e909c64c84b2aa02f074b903dd4587c6c41
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835144"
---
# <a name="catalogexecution_parameter_values-ssisdb-database"></a>catalog.execution_parameter_values（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包在执行实例过程中使用的实际参数值。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|execution_parameter_id|**bigint**|执行参数的唯一标识符 (ID)。|  
|execution_id|**bigint**|执行实例的唯一 ID。|  
|object_type|**smallint**|当值为 `20` 时，参数为项目参数。 当值为 `30` 时，参数为包参数。 当值为 `50` 时，参数为以下参数之一：<br /><br /> **LOGGING_LEVEL**<br /><br /> **DUMP_ON_ERROR**<br /><br /> **DUMP_ON_EVENT**<br /><br /> **DUMP_EVENT_CODE**<br /><br /> **CALLER_INFO**<br /><br /> **SYNCHRONIZED**|  
|parameter_data_type|**nvarchar(128)**|参数的数据类型。|  
|parameter_name|**sysname**|参数的名称。|  
|parameter_value|**sql_variant**|参数值。 当 sensitive 为 `0` 时，显示纯文本值。 当 sensitive 为 `1` 时，显示 **NULL** 值。|  
|sensitive|**bit**|当值为 `1` 时，参数值是敏感的。 当值为 `0` 时，参数值是不敏感的。|  
|必需|**bit**|如果值为 `1`，则需要参数值才能开始执行。 如果值为 `0`，则不需要参数值即可开始执行。|  
|value_set|**bit**|如果值为 `1`，则已分配参数值。 如果值为 `0`，则尚未分配参数值。|  
|runtime_override|**bit**|当值为 `1` 时，会在开始执行之前更改参数值的原始值。 当值为 `0` 时，参数值为设置的原始值。|  
  
## <a name="remarks"></a>备注  
 此视图对于目录中的每个执行参数显示一行。 执行参数值时在单个执行实例中分配给项目参数或包参数的执行参数值。  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对执行实例的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
