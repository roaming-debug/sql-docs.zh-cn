---
description: catalog.startup
title: catalog.startup | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 271fd405-246a-4852-bfbe-f557241ce6ea
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2dc49249c3122c41d89ff8e9457484539ed2f836
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352760"
---
# <a name="catalogstartup"></a>catalog.startup 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对 SSISDB 目录的操作状态进行维护。  
  
 该存储过程可以纠正当 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务器实例出现故障时正在运行的任何包的状态。  
  
 可以选择让该存储过程在每次重新启动 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务器实例时自动运行，方法是选择“创建目录”对话框中的“在 SQL Server 启动时启用 Integration Services 存储过程自动执行”选项。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.startup  
```  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对执行实例的 READ 和 MODIFY 权限，针对项目的 READ 和 EXECUTE 权限，针对引用环境的 READ 权限（如果适用）  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
  
