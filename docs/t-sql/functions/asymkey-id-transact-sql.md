---
description: ASYMKEY_ID (Transact-SQL)
title: ASYMKEY_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AsymKey_ID
- ASYMKEY_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], AsymKey_ID
- ASYMKEY_ID function
- encryption [SQL Server], asymmetric keys
- identification numbers [SQL Server], asymmetric keys
- IDs [SQL Server], asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: d697daf8-2106-4ebb-b09a-ca0be465d747
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d3bbf1ce53a84b1cf828def701d1dac59df884bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202233"
---
# <a name="asymkey_id-transact-sql"></a>ASYMKEY_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回非对称密钥的 ID。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ASYMKEY_ID ( 'Asym_Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*Asym_Key_Name*  
数据库中非对称密钥的名称。
  
## <a name="return-types"></a>返回类型
 **int**  
  
## <a name="permissions"></a>权限  
需要对非对称密钥具有相应的权限，并且调用方对非对称密钥的 VIEW 权限没有被拒绝。 有关非对称密钥权限的详细信息，请参阅 [CREATE ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-asymmetric-key-transact-sql.md)。
  
## <a name="examples"></a>示例  
此示例返回非对称密钥 `ABerglundKey11` 的 ID。
  
```sql
SELECT ASYMKEY_ID('ABerglundKey11');  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
[ALTER ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)  
[DROP ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)  
[SIGNBYASYMKEY (Transact-SQL)](../../t-sql/functions/signbyasymkey-transact-sql.md)  
[VERIFYSIGNEDBYASYMKEY (Transact-SQL)](../../t-sql/functions/verifysignedbyasymkey-transact-sql.md)  
[加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
[sys.asymmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md)  
[安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
[ASYMKEYPROPERTY (Transact-SQL)](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
[KEY_ID (Transact-SQL)](../../t-sql/functions/key-id-transact-sql.md)
  
  
