---
description: sys.master_key_passwords (Transact-SQL)
title: sys.master_key_passwords (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.master_key_passwords_TSQL
- master_key_passwords_TSQL
- sys.master_key_passwords
- master_key_passwords
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_key_passwords catalog view
ms.assetid: b8e18cff-a9e6-4386-98ce-1cd855506e03
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e5aeeb3cf62d687b6f59a93ba07ed19b5f7e3fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191351"
---
# <a name="sysmaster_key_passwords-transact-sql"></a>sys.master_key_passwords (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  为使用 **sp_control_dbmasterkey_password** 存储过程添加的每个数据库主密钥密码返回一行。 用于保护主密钥的密码存储在凭据存储区中。 凭据名称采用以下格式： # #DBMKEY_<database_family_guid>_<random_password_guid # #。 该密码存储为凭据机密。 对于使用 **sp_control_dbmasterkey_password** 添加的每个密码，都有 **一行。**  
  
 此视图中的每一行都显示了一个 **credential_id** **family_guid** ，以及数据库的主密钥，该密钥受与该凭据关联的密码保护。 在 **credential_id** 上，与 **sys.** credential 的联接将返回有用的字段，例如 **create_date** 和凭据名称。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**credential_id**|**int**|密码所属的凭据的 ID。 该 ID 在服务器实例中是唯一的。|  
|**family_guid**|**uniqueidentifier**|创建时原始数据库的唯一 ID。 在还原或附加数据库后，即使更改了数据库名称，这个 GUID 也将始终保持不变。<br /><br /> 如果服务主密钥的自动解密失败， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则使用 **family_guid** 来识别可能包含用于保护数据库主密钥的密码的凭据。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sp_control_dbmasterkey_password (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
