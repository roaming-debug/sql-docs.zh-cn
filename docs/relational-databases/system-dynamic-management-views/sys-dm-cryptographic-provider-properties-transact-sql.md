---
description: sys.dm_cryptographic_provider_properties (Transact-SQL)
title: sys.dm_cryptographic_provider_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cryptographic_provider_properties_TSQL
- sys.dm_cryptographic_provider_properties
- sys.dm_cryptographic_provider_properties_TSQL
- dm_cryptographic_provider_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_properties dynamic management view
ms.assetid: 024b0095-6766-4189-a39a-d316c5ec2874
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 419793e94cc85a8dba1e29f4b79694820518eaad
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384646"
---
# <a name="sysdm_cryptographic_provider_properties-transact-sql"></a>sys.dm_cryptographic_provider_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关注册的加密提供程序的信息。  
  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|provider_id|**int**|加密提供程序的标识号。|  
|GUID|**uniqueidentifier**|唯一的提供程序 GUID。|  
|provider_version|**nvarchar(256)**|格式为 " *aa.bb.cccc.dd* " 的提供程序版本。|  
|sqlcrypt_version|**nvarchar(256)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]格式为 " *aa.bb.cccc.dd* " 的加密 API 的主要版本。|  
|friendly_name|**nvarchar(2048)**|提供程序提供的名称。|  
|authentication_type|**nvarchar(256)**|WINDOWS、BASIC 或其他。|  
|symmetric_key_support|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|symmetric_key_export|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|symmetric_key_import|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|symmetric_key_persistance|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|asymmetric_key_support|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|asymmetric_key_export|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|symmetric_key_import|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
|symmetric_key_persistance|**tinyint**|0（不支持）<br /><br /> 1（支持）|  
  
## <a name="permissions"></a>权限  
 **Sys.dm_cryptographic_provider_properties** 视图对公共可见。  
  
## <a name="see-also"></a>另请参阅  
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [与安全性相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
