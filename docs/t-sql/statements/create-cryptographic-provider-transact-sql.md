---
description: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_CRYPTOGRAPHIC_TSQL
- CRYPTOGRAPHIC PROVIDER
- PROVIDER_TSQL
- CREATE CRYPTOGRAPHIC
- CREATE CRYPTOGRAPHIC PROVIDER
- CRYPTOGRAPHIC_PROVIDER_TSQL
- CREATE_CRYPTOGRAPHIC_PROVIDER_TSQL
- PROVIDER
dev_langs:
- TSQL
helpviewer_keywords:
- 33085 (Database Engine error)
- CREATE CRYPTOGRAPHIC PROVIDER statement
- 33032 (Database Engine error)
ms.assetid: 059a39a6-9d32-4d3f-965b-0a1ce75229c7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 91478060af31e142f94730b9df3c0a3cdf2a24ef
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124092"
---
# <a name="create-cryptographic-provider-transact-sql"></a>CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  通过可扩展密钥管理 (EKM) 提供程序在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内创建加密提供程序。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
CREATE CRYPTOGRAPHIC PROVIDER provider_name   
    FROM FILE = path_of_DLL  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 provider_name  
 可扩展密钥管理提供程序的名称。  
  
 path_of_DLL  
 实现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可扩展的密钥管理接口的 .dll 文件的路径。 使用用于 Microsoft Azure Key Vault 的 SQL Server 连接器时，默认位置是 C:\Program Files\Microsoft SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll。  
  
## <a name="remarks"></a>备注  
 提供程序创建的所有密钥都将按照提供程序的 GUID 引用提供程序。 在 DLL 的所有版本中都将保留此 GUID。  
  
 必须使用任何证书对实现 SQLEKM 接口的 DLL 进行数字签名。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将验证此签名。 此签名包括其证书链，证书链的根目录必须安装在 Windows 系统的受信任的根证书颁发机构位置。 如果该签名未经正确验证，则 CREATE CRYPTOGRAPHIC PROVIDER 语句将失败。 有关证书和证书链的详细信息，请参阅 [SQL Server 证书和对称密钥](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
 当 EKM 提供程序 DLL 未实现所有必需的方法时，CREATE CRYPTOGRAPHIC PROVIDER 可能返回错误 33085：  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
 当用于创建 EKM 提供程序 DLL 的标头文件过期时，CREATE CRYPTOGRAPHIC PROVIDER 可能返回错误 33032：  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>权限  
 要求具有 CONTROL SERVER 权限，或者具有 sysadmin 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例通过 .dll 文件在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建名为 `SecurityProvider` 的加密提供程序。 此 .dll 文件命名为 `c:\SecurityProvider\SecurityProvider_v1.dll` 并安装在服务器上。 必须首先在服务器上安装此提供程序的证书。  
  
```sql  
-- Install the provider  
CREATE CRYPTOGRAPHIC PROVIDER SecurityProvider  
    FROM FILE = 'C:\SecurityProvider\SecurityProvider_v1.dll';  
```  
  
## <a name="see-also"></a>另请参阅  
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)](../../t-sql/statements/alter-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER (Transact-SQL)](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [使用 Azure Key Vault 设置 SQL Server TDE 可扩展密钥管理](../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)  
 [sys.cryptographic_providers](../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)  
 [sys.dm_cryptographic_provider_properties](../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-properties-transact-sql.md)
  
  
