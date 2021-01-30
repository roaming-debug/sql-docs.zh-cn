---
description: sys.fn_check_object_signatures (Transact-SQL)
title: sys.fn_check_object_signatures (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_check_object_signatures_TSQL
- fn_check_object_signatures_TSQL
- fn_check_object_signatures
- sys.fn_check_object_signatures
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_check_object_signatures function
ms.assetid: 47509566-d3d7-46a9-89c1-91b4895d56b9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25c4774812d900bcbc0b9979b966f7ba7a3945bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206125"
---
# <a name="sysfn_check_object_signatures-transact-sql"></a>sys.fn_check_object_signatures (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  返回所有可签名对象的列表，并指示对象是否由指定证书或非对称密钥签名。 如果对象是由指定证书或非对称密钥签名，则还会返回该对象的签名是否有效。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_ check_object_signatures (   
    { '@class' } , { @thumbprint }   
  )   
```  
  
## <a name="arguments"></a>参数  
 { '\@*类*"}  
 标识提供的指纹类型：  
  
-   “证书”  
  
-   “非对称密钥”  
  
 \@*类* 为 **sysname**。  
  
 { \@ *thumbprint* }  
 用来对密钥进行加密的证书的 SHA-1 哈希，或用来对密钥进行加密的非对称密钥的 GUID。 \@*指纹* 为 **varbinary (20)**。  
  
## <a name="tables-returned"></a>返回的表  
 下表列出了 **fn_check_object_signatures** 返回的列。  
  
|列|类型|说明|  
|------------|----------|-----------------|  
|type|**nvarchar(120)**|返回类型说明或程序集。|  
|entity_id|**int**|返回要计算的对象的对象 ID。|  
|is_signed|**int**|当对象不是由提供的指纹签名时返回 0。 当对象由提供的指纹签名时返回 1。|  
|is_signature_valid|**int**|当 is_signed 值为 1 且签名无效时，返回 0。 签名有效则返回 1。<br /><br /> 当 is_signed 值为 0 时，始终返回 0。|  
  
## <a name="remarks"></a>备注  
 使用 **fn_check_object_signatures** 确认恶意用户未篡改对象。  
  
## <a name="permissions"></a>权限  
 要求对证书或非对称密钥拥有 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
 下面的示例查找 `master` 数据库的架构签名证书，对于该架构签名证书签名的具有有效签名的对象，返回值为 1 的 `is_signed` 和值为 1 的 `is_signature_valid`。  
  
```  
USE master;  
-- Declare a variable to hold the thumbprint.  
DECLARE @thumbprint varbinary(20) ;  
-- Populate the thumbprint variable with the master database schema signing certificate.  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%' ;  
-- Evaluates the objects signed by the schema signing certificate  
SELECT type, entity_id, OBJECT_NAME(entity_id) AS [object name], is_signed, is_signature_valid  
FROM sys.fn_check_object_signatures ('certificate', @thumbprint) ;  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [IS_OBJECTSIGNED &#40;Transact-sql&#41;](../../t-sql/functions/is-objectsigned-transact-sql.md)  
  
  
