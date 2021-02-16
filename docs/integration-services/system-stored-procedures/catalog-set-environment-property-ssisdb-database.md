---
description: catalog.set_environment_property（SSISDB 数据库）
title: catalog.set_environment_property（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a345675b-d32e-4624-96cf-ec656730b114
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 892823abe1f7607a034f9cc22b34dd7a81c12ffc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273218"
---
# <a name="catalogset_environment_property-ssisdb-database"></a>catalog.set_environment_property（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境的属性。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_environment_property [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含环境的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @environment_name = ] environment_name   
 环境的名称。 environment_name 为 nvarchar(128)   。  
  
 [ @property_name = ] property_name  
 环境属性的名称。 property_name 为 nvarchar(128)。  
  
 [ @property_value = ] property_value  
 环境属性的值。 property_value 为 nvarchar(1024)。  
  
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
  
-   文件夹名称无效  
  
-   属性名称无效  
  
-   环境名称无效  
  
## <a name="remarks"></a>备注  
 在这一版本中，仅可设置 `Description` 属性。 `Description` 属性的属性值不能超过 4000 个字符。  
  
  
