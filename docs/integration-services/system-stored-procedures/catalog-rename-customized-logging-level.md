---
description: catalog.rename_customized_logging_level
title: catalog.rename_customized_logging_level | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b1a57d5e-3f03-4901-8b2b-bb8b371b595b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 349a3e41d55adf5bc085d1905f7e5e947e7ba266
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342231"
---
# <a name="catalogrename_customized_logging_level"></a>catalog.rename_customized_logging_level 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  重命名现有的自定义日志记录级别。 有关自定义日志记录级别的详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.rename_customized_logging_level [ @old_name = ] old_name  
    , [ @new_name = ] new_name  
```  
  
## <a name="arguments"></a>参数  
 [ @old_name = ] old_name  
 要重命名的现有自定义日志记录级别的名称。  
  
 old_name 为 nvarchar(128)。  
  
 [ @new_name = ] new_name  
 指定的自定义日志记录级别的新名称。  
  
 new_name 为 nvarchar(128)。  
  
## <a name="remarks"></a>注解  
  
## <a name="return-codes"></a>返回代码  
 0（成功）  
  
 存储过程失败时引发错误。  
  
## <a name="result-set"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色中的成员资格  
  
-   **sysadmin** 服务器角色中的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下表说明了导致存储过程失败的情况。  
  
-   用户不具有所需的权限。  
  
  
