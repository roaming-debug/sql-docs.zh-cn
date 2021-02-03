---
description: 加密函数 (Transact-SQL)
title: 加密函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], cryptographic
- crypto functions
- cryptography [SQL Server], functions
- decryption [SQL Server], functions
- security functions
- encryption [SQL Server], functions
ms.assetid: 0be5626b-5a25-4d8c-9f44-7abbfccf816c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a4e0de6fbf1181c57681b8bd3f554575cb2ab92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184133"
---
# <a name="cryptographic-functions-transact-sql"></a>加密函数 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

这些函数支持数字签名、数字签名验证、加密和解密。
  
## <a name="symmetric-encryption-and-decryption"></a>对称加密和解密

:::row:::
    :::column:::
        [ENCRYPTBYKEY](../../t-sql/functions/encryptbykey-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYKEY](../../t-sql/functions/decryptbykey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ENCRYPTBYPASSPHRASE](../../t-sql/functions/encryptbypassphrase-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYPASSPHRASE](../../t-sql/functions/decryptbypassphrase-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [KEY_ID](../../t-sql/functions/key-id-transact-sql.md)
    :::column-end:::
    :::column:::
        [KEY_GUID](../../t-sql/functions/key-guid-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [DECRYPTBYKEYAUTOASYMKEY](../../t-sql/functions/decryptbykeyautoasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [KEY_NAME](../../t-sql/functions/key-name-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SYMKEYPROPERTY](../../t-sql/functions/symkeyproperty-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="asymmetric-encryption-and-decryption"></a>非对称加密和解密
  
:::row:::
    :::column:::
        [ENCRYPTBYASYMKEY](../../t-sql/functions/encryptbyasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYASYMKEY](../../t-sql/functions/decryptbyasymkey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ENCRYPTBYCERT](../../t-sql/functions/encryptbycert-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYCERT](../../t-sql/functions/decryptbycert-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ASYMKEYPROPERTY](../../t-sql/functions/asymkeyproperty-transact-sql.md)
    :::column-end:::
    :::column:::
        [ASYMKEY_ID](../../t-sql/functions/asymkey-id-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="signing-and-signature-verification"></a>签名和签名验证

:::row:::
    :::column:::
        [SIGNBYASYMKEY](../../t-sql/functions/signbyasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [VERIFYSIGNEDBYASMKEY](../../t-sql/functions/verifysignedbyasymkey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SIGNBYCERT](../../t-sql/functions/signbycert-transact-sql.md)
    :::column-end:::
    :::column:::
        [VERIGYSIGNEDBYCERT](../../t-sql/functions/verifysignedbycert-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [IS_OBJECTSIGNED](../../t-sql/functions/is-objectsigned-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;
  
## <a name="symmetric-decryption-with-automatic-key-handling"></a>含自动密钥处理的对称解密

:::row:::
    :::column:::
        [DecryptByKeyAutoCert](../../t-sql/functions/decryptbykeyautocert-transact-sql.md)
    :::column-end:::
:::row-end:::
 
&nbsp;

## <a name="encryption-hashing"></a>加密哈希运算

:::row:::
    :::column:::
        [HASHBYTES](../../t-sql/functions/hashbytes-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="certificate-copying"></a>证书复制 

:::row:::
    :::column:::
        [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md)
    :::column-end:::
    :::column:::
        [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="see-also"></a>请参阅
[函数](../../t-sql/functions/functions.md)  
[加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
[权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
[CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
[安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
  
  
