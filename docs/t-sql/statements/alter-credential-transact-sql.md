---
description: ALTER CREDENTIAL (Transact-SQL)
title: ALTER CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER CREDENTIAL
- ALTER_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], credentials
- credentials [SQL Server], ALTER CREDENTIAL statement
- modifying credentials
- authentication [SQL Server], credentials
- ALTER CREDENTIAL statement
ms.assetid: b08899a6-c09e-4af4-91aa-a978ada79264
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017
ms.openlocfilehash: c16003a3d265bbebc613c6a7eacd798f5c00da6d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128131"
---
# <a name="alter-credential-transact-sql"></a>ALTER CREDENTIAL (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改凭据的属性。  

> [!IMPORTANT]
> “应执行操作”为最佳做法；“必须执行操作”用于完成任务![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
ALTER CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *credential_name*  
 指定将要更改的凭据的名称。  
  
 IDENTITY **='** _identity_name_*_'_*  
 指定从服务器外部进行连接时要使用的帐户名称。  
  
 SECRET **='** _secret_*_'_*  
 指定发送身份验证所需的机密内容。 *secret* 是可选项。
  
> [!IMPORTANT]
> Azure SQL 数据库仅支持 Azure Key Vault 和共享访问签名标识。 不支持 Windows 用户标识。
  
## <a name="remarks"></a>备注  
 当凭据发生更改时，identity_name 和 secret 的值都将重置。 如果未指定可选参数 SECRET 的值，则存储的密码值将设置为 NULL。  
  
 使用服务主密钥对密码进行加密。 如果重新生成服务主密钥，则需要使用新服务主密钥对该密码重新加密。  
  
 可以在 **sys.credentials** 目录视图中查看有关凭据的信息。  
  
## <a name="permissions"></a>权限  
 需要 ALTER ANY CREDENTIAL 权限。 如果该凭据是系统凭据，则要求具有 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-password-of-a-credential"></a>A. 更改凭据的密码  
 以下示例将更改存储在名为 `Saddles` 的凭据中的密码。 该凭据包含 Windows 登录名 `RettigB` 及其密码。 使用 SECRET 子句将新密码添加到凭据。  
  
```sql  
ALTER CREDENTIAL Saddles WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. 删除凭据的密码  
 以下示例将删除名为 `Frames` 的凭据中的密码。 该凭据包含 Windows 登录名 `Aboulrus8` 和密码。 因为未指定 SECRET 选项，所以执行该语句后，凭据的密码为空。  
  
```sql  
ALTER CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [DROP CREDENTIAL (Transact-SQL)](../../t-sql/statements/drop-credential-transact-sql.md)   
 [ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.credentials (Transact-SQL)](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
  
