---
description: sys.dm_cryptographic_provider_keys (Transact-SQL)
title: sys.dm_cryptographic_provider_keys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys
- sys.dm_cryptographic_provider_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_keys dynamic management function
ms.assetid: 5a8c1421-c56b-44b5-96e5-4f01782a0c7c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fcfeed749aea4fa18ac74ba4e78cc9dfa368828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196301"
---
# <a name="sysdm_cryptographic_provider_keys-transact-sql"></a>sys.dm_cryptographic_provider_keys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关可扩展密钥管理 (EKM) 提供程序提供的密钥的信息。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
dm_cryptographic_provider_keys ( provider_id )  
```  
  
## <a name="arguments"></a>参数  
 *provider_id*  
 EKM 提供程序的标识号，没有默认值。  
  
## <a name="tables-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**key_id**|**int**|提供程序中密钥的标识号。|  
|key_name|**nvarchar(512)**|提供程序中密钥的名称。|  
|**key_thumbprint**|**varbinary(32)**|来自密钥提供程序的指纹。|  
|**algorithm_id**|**int**|提供程序中算法的标识号。|  
|**algorithm_tag**|**int**|提供程序中算法的标记。|  
|**key_type**|**nchar(256)**|提供程序中密钥的类型。|  
|**key_length**|**int**|提供程序中密钥的长度。|  
  
## <a name="permissions"></a>权限  
 查询此视图时，它会将用户上下文送至提供程序进行身份验证并枚举用户可见的所有密钥。  
  
 如果用户无法通过 EKM 提供程序的身份验证，则不会返回任何密钥信息。  
  
## <a name="examples"></a>示例  
 下面的示例显示了标识号为 `1234567` 的提供程序的密钥属性。  
  
```  
SELECT * FROM sys.dm_cryptographic_provider_keys(1234567);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
