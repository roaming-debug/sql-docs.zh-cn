---
description: catalog.cleanup_server_execution_keys
title: catalog.cleanup_server_execution_keys | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a79f1006-54e8-4cbf-96f8-5ed143ebb830
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 02872a9142ecb9b010846925f87d9b852a4d4d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340420"
---
# <a name="catalogcleanup_server_execution_keys"></a>catalog.cleanup_server_execution_keys 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从 SSISDB 数据库中删除证书和对称密钥。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.cleanup_server_execution_keys [ @cleanup_flag = ] cleanup_flag ,  
[ @delete_batch_size = ] delete_batch_size  
```  
  
## <a name="arguments"></a>参数  
 [ @cleanup_flag = ] cleanup_flag  
 指示是否删除执行级别 (1) 或项目级别 (2) 证书以及对称密钥。  
  
 仅当 SERVER_OPERATION_ENCRYPTION_LEVEL 设置为 PER_EXECUTION (1) 时，使用执行级别 (1)。  
  
 仅当 SERVER_OPERATION_ENCRYPTION_LEVEL 设置为 PER_PROJECT (2) 时，使用项目级别 (2)。 将仅删除后列项目的证书和对称密钥：已删除的项目和已为其清理操作日志的项目。  
  
 [ @delete_batch_size = ] delete_batch_size  
 要删除的密钥和证书的数量。 默认值为 1000。  
  
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
  
-   **PER_EXECUTION (1)** - 为每次执行创建用于保护敏感执行参数和执行日志的证书和对称密钥。 这是默认值。 因为每次执行都会生成证书/密钥，生产环境中可能会出现性能问题（死锁、失败的维护工作等）。 但是，此设置提供的安全性级别比其他值 (2) 高。  
  
-   **PER_PROJECT (2)** - 为每个项目创建用于保护敏感参数的证书和对称密钥。 这提供比 PER_EXECUTION 级别更好的性能，因为密钥和证书是一次为一个项目生成，而不是为每个执行生成的。  
  
 必须先运行 [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) 存储过程，然后才能将 SERVER_OPERATION_ENCRYPTION_LEVEL 从 1 更改为 2 或从 2 更改为 1。 在运行此存储过程之前，请执行以下操作：  
  
1.  确保在 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)表中将属性 OPERATION_CLEANUP_ENABLED 的值设置为 TRUE。  
  
2.  将 Integration Services 数据库 (SSISDB) 设置为单用户模式。 在 SQL Server Management Studio 中，启动 SSISDB 的“数据库属性”对话框，切换到“选项”选项卡，并将“限制访问”属性设置为单用户模式 (SINGLE_USER)。 运行 cleanup_server_log 存储过程后，将属性值设置回原始值。  
  
3.  运行存储过程 [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md)。  
  
4.  现在，继续在 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)表中更改 SERVER_OPERATION_ENCRYPTION_LEVEL 属性的值。  
  
5.  运行存储过程 [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md) 以从 SSISDB 数据库清理证书密钥。 从 SSISDB 数据库中删除证书和密钥可能需要很长时间，因此应在非峰值时间定期运行。  
  
     可指定范围或级别（执行与项目）以及要删除的密钥数量。 删除的默认批大小为 1000。 将级别设置为 2 时，仅当删除关联的项目时才会删除密钥和证书。  
  
 有关详细信息，请参阅下面的知识库文章。 [修复：在 SQL Server 2012 中将 SSISDB 用作部署存储时的性能问题](https://support.microsoft.com/kb/2972285)  
  
## <a name="example"></a>示例  
 以下示例调用 cleanup_server_execution_keys 存储过程。  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
  
EXEC@return_value = [internal].[cleanup_server_execution_keys]  
@cleanup_flag = 1,  
@delete_batch_size = 500  
  
SELECT'Return Value' = @return_value  
  
GO  
```  
  
  
