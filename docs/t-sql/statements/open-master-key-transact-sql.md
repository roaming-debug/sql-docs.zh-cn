---
description: OPEN MASTER KEY (Transact-SQL)
title: OPEN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPEN MASTER KEY DECRYPTION BY PASSWORD
- OPEN_MASTER_KEY_DECRYPTION_BY_PASSWORD_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- OPEN_MASTER_KEY_TSQL
- MASTER KEY DECRYPTION
- OPEN MASTER KEY
- MASTER_KEY_DECRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- opening Database Master Keys
- encryption [SQL Server], Database Master Key
- cryptography [SQL Server], Database Master Key
- master key decryption
- OPEN MASTER KEY statement
- database master key [SQL Server], opening
ms.assetid: 1674753e-ca1e-4913-9ba4-b442e7106121
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7248d97eef63492664a47dbba9a736ed295f1242
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476598"
---
# <a name="open-master-key-transact-sql"></a>OPEN MASTER KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  打开当前数据库的数据库主密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'password'   
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 '*password*'  
 对数据库主密钥加密的密码。  
  
## <a name="remarks"></a>注解  
 如果数据库主密钥使用服务主密钥进行加密，则在需要此数据库主密钥进行解密或加密时，它将自动打开。 在这种情况下，不必使用 **OPEN MASTER KEY** 语句。  
  
 当数据库第一次附加或还原到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时，数据库主密钥（由服务主密钥加密）的副本尚未存储在服务器中。 必须使用 **OPEN MASTER KEY** 语句解密数据库主密钥 (DMK)。 一旦 DMK 解密后，通过使用 **ALTER MASTER KEY REGENERATE** 语句向服务器提供 DMK（使用服务主密钥 (SMK) 加密）的副本，即可拥有将来启用自动解密的选项。 当数据库已从较早版本升级后，应重新生成 DMK 以使用更新的 AES 算法。 有关重新生成 DMK 的详细信息，请参阅 [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)。 重新生成 DMK 密钥以升级到 AES 所需的时间取决于 DMK 保护的对象数。 重新生成 DMK 密钥以升级到 AES 只在必需时执行一次，不影响将来作为密钥循环策略的一部分而重新生成的过程。  
  
 通过使用带 DROP ENCRYPTION BY SERVICE MASTER KEY 选项的 ALTER MASTER KEY 语句，可从自动密钥管理中排除特定数据库的数据库主密钥。 然后，必须显式打开带密码的数据库主密钥。  
  
 如果数据库主密钥在其中显式打开的事务回滚，则此密钥将保持打开。  
  
## <a name="permissions"></a>权限  
 要求对数据库具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例将打开使用密码加密的 `AdventureWorks2012` 数据库的数据库主密钥。  
  
```sql  
USE AdventureWorks2012;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例将打开使用密码加密的 master 数据库。  
  
```sql  
USE master;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
CLOSE MASTER KEY;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CLOSE MASTER KEY (Transact-SQL)](../../t-sql/statements/close-master-key-transact-sql.md)   
 [BACKUP MASTER KEY (Transact-SQL)](../../t-sql/statements/backup-master-key-transact-sql.md)   
 [RESTORE MASTER KEY (Transact-SQL)](../../t-sql/statements/restore-master-key-transact-sql.md)   
 [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [DROP MASTER KEY (Transact-SQL)](../../t-sql/statements/drop-master-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

