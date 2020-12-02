---
description: catalog.get_parameter_values（SSISDB 数据库）
title: catalog.get_parameter_values（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5b1aeaf7-c938-4aef-bafc-e4d7a82eb578
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 349a68fa94cfb479c6c3823b2b295a3a65498692
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129784"
---
# <a name="catalogget_parameter_values-ssisdb-database"></a>catalog.get_parameter_values（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目和对应包中解析和检索默认参数值。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.get_parameter_values [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id  ]  
  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含项目的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @project_name = ] project_name   
 参数所在的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ @package_name = ] package_name  
 包的名称。 指定包名称，以便从特定包中检索所有项目参数和参数。 package_name 为 nvarchar(260)。  
  
 [ @reference_id = ] reference_id   
 环境引用的唯一标识符。 此参数是可选的。 reference_id 为 bigint   。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 返回具有以下格式的表：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|参数的类型。 对于项目参数，该值是 `20`；对于包参数，值是 `30`。|  
|parameter_data_type|**nvarchar(128)**|参数的数据类型。|  
|parameter_name|**sysname**|参数的名称。|  
|parameter_value|**sql_variant**|参数值。|  
|sensitive|**bit**|当值为 `1` 时，参数值是敏感的。 当值为 `0` 时，参数值是不敏感的。|  
|必需|**bit**|如果值为 `1`，则需要参数值才能开始执行。 如果值为 `0`，则不需要参数值即可开始执行。|  
|value_set|**bit**|如果值为 `1`，则已分配参数值。 如果值为 `0`，则尚未分配参数值。|  
  
> [!NOTE]  
>  以纯文本形式显示文本值。 将显示 NULL 来替代敏感值。  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 权限，如果适用，则包含针对引用环境的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   在指定的文件夹或项目中找不到该包  
  
-   用户没有相应的权限  
  
-   指定的环境引用不存在  
  
  
