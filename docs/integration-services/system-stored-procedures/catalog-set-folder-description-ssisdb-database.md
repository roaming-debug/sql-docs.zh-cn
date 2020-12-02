---
description: catalog.set_folder_description（SSISDB 数据库）
title: catalog.set_folder_description（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 802416f6-5177-4db5-bca5-976dec5faf53
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 18e7cb22f94e33328ed08968052d03b0261c73df
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129639"
---
# <a name="catalogset_folder_description-ssisdb-database"></a>catalog.set_folder_description（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置文件夹的说明。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_folder_description [ @folder_name = ] folder_name  
    , [ @folder_description = ] folder_description  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @folder_description = ] folder_description  
 文件夹的说明。 folder_description 为 nvarchar(MAX)。  
  
## <a name="return-code-value"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 存储过程返回一条消息，以确认新文件夹说明的设置。  
  
  
