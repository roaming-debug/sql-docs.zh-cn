---
description: catalog.create_folder（SSISDB 数据库）
title: catalog.create_folder（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 06fb3549-e970-4ca2-a61f-59affb9c6dcc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a2e322c0b5011d8d99b45b059d71e932d4dd7c46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346790"
---
# <a name="catalogcreate_folder-ssisdb-database"></a>catalog.create_folder（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建一个文件夹。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_folder [ @folder_name = ] folder_name, [ @folder_id = ] folder_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [@folder_name =] folder_name  
 新文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@folder_name =] folder_id  
 文件夹的唯一标识符 (ID)。 folder_id 为 bigint。  
  
## <a name="return-code-value"></a>返回代码值  
 返回文件夹标识符。  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
如果已存在同名的文件夹，改存储过程返回错误。  
  
  
