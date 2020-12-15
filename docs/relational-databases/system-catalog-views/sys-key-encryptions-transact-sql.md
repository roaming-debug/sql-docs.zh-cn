---
description: sys.key_encryptions (Transact-SQL)
title: sys.key_encryptions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.key_encryptions
- key_encryptions_TSQL
- sys.key_encryptions_TSQL
- key_encryptions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.key_encryptions catalog view
ms.assetid: c39cecf8-af63-40b9-98e5-f84a5bf3ae54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c841e05d54ee5ba672107426c2f10b2396b84a76
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472988"
---
# <a name="syskey_encryptions-transact-sql"></a>sys.key_encryptions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为使用 CREATE SYMMETRIC KEY 语句的 ENCRYPTION BY 子句指定的每个对称密钥加密返回一行。  

  
|列名|数据类型|说明|  
|------------------|----------------|-----------------|  
|**key_id**|**int**|已加密密钥的 ID。|  
|**thumbprint**|**varbinary(32)**|用来对密钥证书的 SHA-1 哈希进行加密，或用来对密钥的对称密钥的 GUID 进行加密。|  
|**crypt_type**|**char (4)**|加密类型：<br /><br /> ESKS = 使用对称密钥进行加密<br /><br /> ESKP、ESP2 或 ESP3 = 通过密码加密<br /><br /> EPUC = 使用证书进行加密<br /><br /> EPUA = 使用非对称密钥进行加密<br /><br /> ESKM = 使用主密钥进行加密|  
|**crypt_type_desc**|**nvarchar(60)**|加密类型说明：<br /><br /> ENCRYPTION BY SYMMETRIC KEY<br /><br /> ENCRYPTION BY PASSWORD <br /> (从开始 [!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)] ，包含 CSS 使用的版本号 ) <br /><br /> ENCRYPTION BY CERTIFICATE <br /><br /> ENCRYPTION BY ASYMMETRIC KEY<br /><br /> ENCRYPTION BY MASTER KEY<br /><br /> 注意： Windows DPAPI 用于保护服务主密钥。|  
|**crypt_property**|**varbinary(max)**|标记位或加密位。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
