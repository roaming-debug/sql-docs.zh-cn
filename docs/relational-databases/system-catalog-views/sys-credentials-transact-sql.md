---
description: sys.credentials (Transact-SQL)
title: sys. 凭据 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.credentials
- sys.credentials_TSQL
- credentials_TSQL
- credentials
dev_langs:
- TSQL
helpviewer_keywords:
- sys.credentials catalog view
ms.assetid: ea48cf80-904a-4273-a950-6d35b1b0a1b6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7a8c2a91ee749856cd503349e04984741cff95e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749227"
---
# <a name="syscredentials-transact-sql"></a>sys.credentials (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdbmi-asdw-pdw-md.md)]

  为每个服务器级凭据返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|credential_id|**int**|凭据的 ID。 在服务器中是唯一的。|  
|name|**sysname**|凭据的名称。 在服务器中是唯一的。|  
|credential_identity|**nvarchar(4000)**|要使用的标识的名称。 这通常是一个 Windows 用户。 它不必是唯一的。|  
|create_date|**datetime**|创建凭据的时间。|  
|modify_date|**datetime**|上次修改凭据的时间。|  
|target_type|**nvarchar (100)**|凭据类型。 对于传统凭据，返回 NULL；对于映射到加密提供程序的凭据，返回 CRYPTOGRAPHIC PROVIDER。 有关外部密钥管理提供程序的详细信息，请参阅 [&#40;EKM&#41;的可扩展密钥管理 ](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。|  
|target_id|**int**|该凭据映射到的对象的 ID。 对于传统凭据，返回 0；对于映射到加密提供程序的凭据，返回非 0 值。 有关外部密钥管理提供程序的详细信息，请参阅 [&#40;EKM&#41;的可扩展密钥管理 ](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。|  

## <a name="remarks"></a>备注  
有关数据库级凭据，请参阅 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)。
  
## <a name="permissions"></a>权限  
 需要 `VIEW ANY DEFINITION` 权限或 `ALTER ANY CREDENTIAL` 权限。 此外，不能拒绝主体 `VIEW ANY DEFINITION` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)   
 [凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)  
  
  
