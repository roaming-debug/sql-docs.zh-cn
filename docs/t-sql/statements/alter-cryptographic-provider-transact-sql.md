---
description: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CRYPTOGRAPHIC_PROVIDER_TSQL
- ALTER CRYPTOGRAPHIC PROVIDER
- ALTER_CRYPTOGRAPHIC_TSQL
- ALTER CRYPTOGRAPHIC
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER CRYPTOGRAPHIC PROVIDER
ms.assetid: 876b6348-fb29-49e1-befc-4217979f6416
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6abe4053b87b6417bb1c55708815bc4cc91b2ba0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201150"
---
# <a name="alter-cryptographic-provider-transact-sql"></a>ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  通过可扩展密钥管理 (EKM) 提供程序更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的加密提供程序。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
ALTER CRYPTOGRAPHIC PROVIDER provider_name   
    [ FROM FILE = path_of_DLL ]  
    ENABLE | DISABLE  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 provider_name  
 可扩展密钥管理提供程序的名称。  
  
 Path_of_DLL  
 实现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可扩展密钥管理接口的 .dll 文件的路径。  
  
 ENABLE | DISABLE  
 启用或禁用某个提供程序。  
  
## <a name="remarks"></a>备注  
 如果提供程序更改了用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中实现可扩展密钥管理的 .dll 文件，则您必须使用 ALTER CRYPTOGRAPHIC PROVIDER 语句。  
  
 使用 ALTER CRYPTOGRAPHIC PROVIDER 语句更新 .dll 文件路径时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将执行以下操作：  
-   禁用该提供程序。  
-   验证 DLL 签名并确保该 .dll 文件的 GUID 与目录中记录的 GUID 相同。  
-   更新目录中的 DLL 版本。  
  

当 EKM 提供程序设置为 DISABLE 时，新连接上任何要将该提供程序用于加密语句的尝试都将失败。  
  
若要禁用某个提供程序，必须终止使用该提供程序的所有会话。  
  
当 EKM 提供程序 dll 未实现所需的所有方法时，ALTER CRYPTOGRAPHIC PROVIDER 可能会返回错误 33085：  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
如果用于创建 EKM 提供程序 dll 的标头文件过期，ALTER CRYPTOGRAPHIC PROVIDER 可能会返回错误 33032：  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>权限  
 要求具有加密提供程序的 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中一个名为 `SecurityProvider` 的加密提供程序更改为更新版本的 .dll 文件。 该新版本名为 `c:\SecurityProvider\SecurityProvider_v2.dll` 并且安装在服务器上。 服务器上必须安装有该提供程序的证书。  
  
1. 禁止该提供程序执行升级。 这样会终止所有打开的加密会话。  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
DISABLE;  
GO  
```  

2. 升级提供程序 .dll 文件。 GUID 必须与之前的版本相同，但该版本可以不同。  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider  
FROM FILE = 'c:\SecurityProvider\SecurityProvider_v2.dll';  
GO  
```  

3. 启用升级提供程序。   
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
ENABLE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER (Transact-SQL)](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
