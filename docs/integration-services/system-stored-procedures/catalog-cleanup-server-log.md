---
description: catalog.cleanup_server_log
title: catalog.cleanup_server_log | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 0dedb685-d3a6-4bd6-8afd-58d98853deee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3784d6262bc8f3fd2653b55970d3915587a68276
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346890"
---
# <a name="catalogcleanup_server_log"></a>catalog.cleanup_server_log 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  清除操作日志，使 SSISDB 数据库进入可让您更改 SERVER_OPERATION_ENCRYPTION_LEVEL 属性的值的状态。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.cleanup_server_log  
```  
  
## <a name="arguments"></a>参数  
 无。  
  
## <a name="return-code-values"></a>返回代码值  
 0 表示成功，1 表示失败。  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 和 EXECUTE 权限，以及针对引用环境的 READ 权限（如果适用）。  
  
-   ssis_admin 数据库角色的成员资格。  
  
-   **sysadmin** 服务器角色的成员资格。  
  
## <a name="errors-and-warnings"></a>错误和警告  
 在以下情况中，此存储过程引发错误：  
  
-   SSISDB 数据库中有一个或多个活动操作。  
  
-   SSISDB 数据库未处于单用户模式下。  
  
## <a name="remarks"></a>注解  
 SQL Server 2012 Service Pack 2 将 SERVER_OPERATION_ENCRYPTION_LEVEL 属性添加到 internal.catalog_properties 表。 该属性有两个可能值：  
  
-   **PER_EXECUTION (1)** - 为每次执行创建用于保护敏感执行参数和执行日志的证书和对称密钥。 因为每次执行都会生成证书/密钥，生产环境中可能会出现性能问题（死锁、失败的维护工作等）。 但是，此设置提供的安全性级别比其他值 (2) 高。  
  
-   **PER_PROJECT (2)** - 为每个项目创建用于保护敏感参数的证书和对称密钥。 PER_PROJECT (2) 是默认值。 此设置提供比 PER_EXECUTION 级别更好的性能，因为密钥和证书针对一个项目生成一次，而不是为每次执行生成。  
  
 必须先运行 [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) 存储过程，然后才可将 SERVER_OPERATION_ENCRYPTION_LEVEL 从 2 更改为 1 或从 1 更改为 2。 在运行此存储过程之前，请执行以下操作：  
  
1.  确保在 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)表中将属性 OPERATION_CLEANUP_ENABLED 的值设置为 TRUE。  
  
2.  将 Integration Services 数据库 (SSISDB) 设置为单用户模式。 在 SQL Server Management Studio 中，启动 SSISDB 的“数据库属性”对话框，切换到“选项”选项卡，并将“限制访问”属性设置为单用户模式 (SINGLE_USER)。 运行 cleanup_server_log 存储过程后，将属性值设置回原始值。  
  
3.  运行存储过程 [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md)。  
  
4.  现在，继续在 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)表中更改 SERVER_OPERATION_ENCRYPTION_LEVEL 属性的值。  
  
5.  运行存储过程 [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md) 以从 SSISDB 数据库清理证书密钥。 从 SSISDB 数据库中删除证书和密钥可能需要很长时间，因此应在非峰值时间定期运行。  
  
     可指定范围或级别（执行与项目）以及要删除的密钥数量。 删除的默认批大小为 1000。 将级别设置为 2 时，仅当删除关联的项目时才会删除密钥和证书。  
  
 有关详细信息，请参阅以下知识库文章：[修复：在 SQL Server 2012 中使用 SSISDB 作为部署存储时的性能问题](https://support.microsoft.com/kb/2972285)  
  
## <a name="example"></a>示例  
 以下示例调用 cleanup_server_log 存储过程。  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
EXEC@return_value = [internal].[cleanup_server_log]  
SELECT'Return Value' = @return_value  
GO   
```  
  
  
