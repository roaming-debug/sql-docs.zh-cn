---
description: sys.column_master_keys (Transact-SQL)
title: sys.column_master_keys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_master_key_definitions_TSQL
- column_master_key_definitions
- sys.column_master_key_definitions_TSQL
- sys.column_master_key_definitions
- column_master_keys_TSQL
- column_master_keys
- sys.column_master_keys_TSQL
- sys.column_master_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_master_key_definitions catalog view
- sys.column_master_keys catalog view
ms.assetid: fbec2efa-5fe9-4121-9b34-60497b0b2aca
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33682106df449dbdd1dd1c4244dffe89118a2964
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161414"
---
# <a name="syscolumn_master_keys-transact-sql"></a>sys.column_master_keys (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  为使用 [CREATE MASTER key](../../t-sql/statements/create-column-master-key-transact-sql.md) 语句添加的每个数据库主密钥返回一行。 每一行代表一个列主密钥 (CMK) 。  
    
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|CMK 的名称。|  
|**column_master_key_id**|**int**|列主密钥的 ID。|  
|create_date|**datetime**|列主密钥的创建日期。|  
|modify_date|**datetime**|列主密钥的上次修改日期。|  
|key_store_provider_name|**sysname**|包含 CMK 的列主密钥存储的提供程序的名称。 允许值包括：<br /><br /> MSSQL_CERTIFICATE_STORE-如果列主密钥存储是证书存储区，则为。<br /><br /> 用户定义的值（如果列主密钥存储为自定义类型）。|  
|**key_path**|**nvarchar(4000)**|密钥的列主密钥存储特定路径。 路径的格式取决于列主密钥存储类型。 例如：<br /><br /> `'CurrentUser/Personal/'<thumbprint>`<br /><br /> 对于自定义列主密钥存储，开发人员负责为自定义列主密钥存储定义密钥路径。|  
|**allow_enclave_computations**|**bit**|指示列主密钥是否已启用 enclave， (如果使用此主密钥加密的列加密密钥可用于服务器端安全 enclaves) 内的计算。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。|  
|**签名**|**varbinary(max)**|**Key_path** 和 **allow_enclave_computations**（使用由 **key_path** 引用的列主密钥生成）的数字签名。|


  
## <a name="permissions"></a>权限  
 需要 **VIEW ANY COLUMN MASTER KEY** 权限。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.column_encryption_key_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [管理具有安全 enclave 的 Always Encrypted 的密钥](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
 
  
  
