---
description: catalog.create_environment_variable（SSISDB 数据库）
title: catalog.create_environment_variable（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346809"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建环境变量。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>参数  
 [@folder_name =] folder_name  
 包含环境的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@environment_name =] environment_name  
 环境的名称。 environment_name 为 nvarchar(128)   。  
  
 [@variable_name =] variable_name  
 环境变量名。 variable_name 为 nvarchar(128)。  
  
 [@data_type =] data_type  
 变量的数据类型。 支持的环境变量数据类型包括 Boolean、Byte、DateTime、Double、Int16、Int32、Int64、Single、String、UInt32 和 UInt64。 不支持的环境变量数据类型包括 Char、DBNull、Object 和 Sbyte。 data_type 参数的数据类型为 nvarchar(128)。  
  
 [@sensitive =] sensitive  
 指示变量是否包含敏感值。 使用值 `1` 表示环境变量的值是敏感值，值为 `0` 表示该值不是敏感值。 存储敏感值时将对其加密。 不敏感的值以纯文本形式存储。Sensitive 为 bit。  
  
 [@value =] value  
 环境变量的值。 value 为 sql_variant。  
  
 [@description =] description  
 环境变量的说明。 value 为 nvarchar(1024)。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对环境的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   文件夹名称、环境名称或环境变量名无效  
  
-   环境中已存在此变量名  
  
-   用户没有相应的权限  
  
## <a name="remarks"></a>注解  
 可以使用环境变量高效地将值分配给项目参数或包参数，以用于执行包。 环境变量启用参数值的排列。 变量名在环境中必须是唯一的。  
  
 此存储过程验证变量的数据类型，以确保它受 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录支持。  
  
> [!TIP]  
>  请考虑在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中使用 Int16 数据类型，而不使用不受支持的 Sbyte 数据类型。  
  
 使用 value 参数传递给此存储过程的值将根据下表，从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型：  
  
|Integration Services 数据类型|SQL Server 数据类型|  
|------------------------------------|--------------------------|  
|**布尔值**|**bit**|  
|**Byte**|**binary**、**varbinary**|  
|**DateTime**|**datetime**、**datetime2**、**datetimeoffset**、**smalldatetime**|  
|**双精度**|精确数字：**decimal**、**numeric**；近似数字：**float**、**real**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**单精度**|精确数字：**decimal**、**numeric**；近似数字：**float**、**real**|  
|**字符串**|**varchar**、**nvarchar**、**char**|  
|**UInt32**|intint 是与 Uint32 最接近的可用映射。）|  
|**UInt64**|bigintint 是与 Uint64 最接近的可用映射。）|  
  
  
