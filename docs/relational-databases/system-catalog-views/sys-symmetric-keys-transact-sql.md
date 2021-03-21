---
description: sys.symmetric_keys (Transact-SQL)
title: sys.symmetric_keys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- symmetric_keys
- sys.symmetric_keys
- sys.symmetric_keys_TSQL
- symmetric_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.symmetric_keys catalog view
ms.assetid: d410eae1-3a52-45de-b9a1-52d2bd93a8eb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4d768543995aa0e934c928eadc06ddbc513c146
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750317"
---
# <a name="syssymmetric_keys-transact-sql"></a>sys.symmetric_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于使用 CREATE SYMMETRIC KEY 语句创建的每个对称密钥，返回与其对应的一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|密钥的名称。 在数据库中是唯一的。|  
|principal_id|**int**|拥有密钥的数据库主体的 ID。|  
|**symmetric_key_id**|**int**|密钥的 ID。 在数据库中是唯一的。|  
|**key_length**|**int**|密钥的长度（位）。|  
|**key_algorithm**|**char(2)**|密钥使用的算法：<br /><br /> R2 = RC2<br /><br /> R4 = RC4<br /><br /> D = DES<br /><br /> D3 = Triple DES<br /><br /> DT = TRIPLE_DES_3KEY<br /><br /> DX = DESX<br /><br /> A1 = AES 128<br /><br /> A2 = AES 192<br /><br /> A3 = AES 256<br /><br /> NA = EKM 密钥|  
|**algorithm_desc**|**nvarchar(60)**|对于密钥所用算法的说明：<br /><br /> RC2<br /><br /> RC4<br /><br /> DES<br /><br /> Triple_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256<br /><br /> NULL（仅限于可扩展密钥管理算法）|  
|create_date|**datetime**|密钥的创建日期。|  
|modify_date|**datetime**|密钥的修改日期。|  
|key_guid|**uniqueidentifier**|与密钥关联的全局唯一标识符 (GUID)。 对于持久化密钥，此标识符是自动生成的。 临时密钥的 GUID 从用户提供的通行短语中派生。|  
|**key_thumbprint**|**sql_variant**|密钥的 SHA-1 哈希。 该哈希是全局唯一的。 对于非可扩展密钥管理密钥，此值将为 NULL。|  
|**provider_type**|**nvarchar(120)**|加密提供程序的类型：<br /><br /> CRYPTOGRAPHIC PROVIDER = 可扩展密钥管理密钥<br /><br /> NULL = 非可扩展密钥管理密钥|  
|**cryptographic_provider_guid**|**uniqueidentifier**|加密提供程序的 GUID。 对于非可扩展密钥管理密钥，此值将为 NULL。|  
|**cryptographic_provider_algid**|**sql_variant**|加密提供程序的算法 ID。 对于非可扩展密钥管理密钥，此值将为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>备注  
 不推荐使用 RC4 算法。 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，可以通过任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
 **关于 DES 算法的说明：**  
  
-   DESX 的命名不正确。 使用 ALGORITHM = DESX 创建的对称密钥实际上使用的是具有 192 位密钥的 TRIPLE DES 密码。 不提供 DESX 算法。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
-   使用 ALGORITHM = TRIPLE_DES_3KEY 创建的对称密钥使用的是具有 192 位密钥的 TRIPLE DES。  
  
-   使用 ALGORITHM = TRIPLE_DES 创建的对称密钥使用的是具有 128 位密钥的 TRIPLE DES。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
