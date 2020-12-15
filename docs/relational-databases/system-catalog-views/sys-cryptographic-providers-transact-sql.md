---
description: sys.cryptographic_providers (Transact-SQL)
title: sys.cryptographic_providers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cryptographic_providers
- sys.cryptographic_providers
- sys.cryptographic_providers_TSQL
- cryptographic_providers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.cryptographic_providers catalog view
ms.assetid: 9da0da95-792e-48b4-9f60-47f0729c279c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f32f7ceea8a68a3ddf1693ae9f382ee066d51eb1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462838"
---
# <a name="syscryptographic_providers-transact-sql"></a>sys.cryptographic_providers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为每个已注册的加密提供程序返回一行。  
    
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**provider_id**|**int**|加密提供程序的标识号。|  
|name|**sysname**|加密提供程序的名称。|  
|**guid**|**uniqueidentifier**|唯一的提供程序 GUID。|  
|**version**|**nvarchar(50)**|格式为 "*aa.bb.cccc.dd*" 的提供程序版本。|  
|**dll_path**|**nvarchar(512)**|实现可扩展密钥管理 (EKM) 应用程序编程接口 (API) 的 DLL 的路径。|  
|**is_enabled**|**bit**|服务器上是否启用了此提供程序。<br /><br /> 0 = 未启用（默认值）<br /><br /> 1 = 已启用|  
  
## <a name="permissions"></a>权限  
 **Sys.cryptographic_providers** 视图对公共可见。  
  
## <a name="see-also"></a>另请参阅  
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
  
