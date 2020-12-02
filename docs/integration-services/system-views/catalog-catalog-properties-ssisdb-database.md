---
description: catalog.catalog_properties（SSISDB 数据库）
title: catalog.catalog_properties（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/11/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e604a382-95c8-4764-b268-742eb5c6d4cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7aa744bd7dd3d0330dc3e996b2af90d500be9d55
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129499"
---
# <a name="catalogcatalog_properties-ssisdb-database"></a>catalog.catalog_properties（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的属性。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|property_name|**nvarchar(256)**|目录属性的名称。|  
|property_value|**nvarchar(256)**|目录属性的值。|  
  
## <a name="remarks"></a>注解  
 此视图对于每个目录属性显示一行。
  
|属性名称|说明|  
|-------------------|-----------------|  
|**DEFAULT_EXECUTION_MODE**|服务器范围的默认包执行模式 - `Server` (0) 或 `Scale Out` (1)。 |
|**ENCRYPTION_ALGORITHM**|用于加密敏感数据的加密算法的类型。 支持的值包括：`DES`、`TRIPLE_DES`、`TRIPLE_DES_3KEY`、`DESX`、`AES_128`、`AES_192` 和 `AES_256`。 注意：要更改此属性，目录数据库必须处于单用户模式。|
|**IS_SCALEOUT_ENABLED**|值如果为 `True`，SSIS Scale Out 功能为启用状态。 如果未启用 Scale Out，该属性可能不会显示在视图中。|
|**MAX_PROJECT_VERSIONS**|为单个项目保留的新项目版本的数量。 当启用了版本清理时，超出了此计数的较早版本将被删除。|  
|**OPERATION_CLEANUP_ENABLED**|当值为 `TRUE` 时，将从目录中删除超过 RETENTION_WINDOW（天）的操作详细信息和操作消息。 当值为 `FALSE` 时，将在目录中存储所有操作详细信息和操作消息。 注意：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作业执行清理操作。|  
|**RETENTION_WINDOW**|操作详细信息和操作消息存储在目录中的天数。 如果值为 `-1`，则保持期窗口是无限的。 注意：如果无需清理，将 OPERATION_CLEANUP_ENABLED 设置为 FALSE 。|
|**SCHEMA_BUILD**|SSISDB 目录数据库架构的生成号。 每当创建或升级 SSISDB 目录，此号相应更改。|
|**SCHEMA_VERSION**|SSISDB 目录数据库架构的主版本号。 每当创建 SSISDB 目录或升级主版本，此号相应更改。|
|**VALIDATION_TIMEOUT**|如果验证没有在该属性指定的秒数内完成，则将停止验证。|  
|**SERVER_CUSTOMIZED_LOGGING_LEVEL**|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的默认自定义日志记录级别。 如果未创建任何自定义日志记录级别，此属性可能不会显示在视图中。|
|**SERVER_LOGGING_LEVEL**|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的默认日志记录级别。|
|**SERVER_OPERATION_ENCRYPTION_LEVEL**|如果值为 1 (`PER_EXECUTION`)，为每个执行创建用于保护敏感执行参数和执行日志的证书和对称密钥。 如果值为 2 (`PER_PROJECT`)，则为每个项目创建一次证书和对称密钥。 有关此属性的详细信息，请参阅 SSIS 存储过程 [catalog.cleanup_server_log](../system-stored-procedures/catalog-cleanup-server-log.md#remarks) 的注解。|
|**VERSION_CLEANUP_ENABLED**|值为 `TRUE` 时，将仅在目录中存储数量为 MAX_PROJECT_VERSIONS 的项目版本，而删除所有其他项目版本。 如果值为 FALSE，将在目录中存储所有项目版本。 注意：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作业执行清理操作。|
|||
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
  
