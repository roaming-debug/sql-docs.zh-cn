---
title: 选择加密算法 | Microsoft Docs
description: 使用本指南选择加密算法来帮助保护 SQL Server 的实例，该实例支持数种常见算法。
ms.custom: ''
ms.date: 08/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: acb57d952832f5ae4d8fbfea0ee037170a854839
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236632"
---
# <a name="choose-an-encryption-algorithm"></a>选择加密算法
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  加密是希望保护 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例安全的管理员可以采用的多种深度防御方法之一。  
  
 加密算法定义了未经授权的用户无法轻松逆转的数据转换。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 允许管理员和开发人员从多种算法中进行选择，其中包括 DES、Triple DES、TRIPLE_DES_3KEY、RC2、RC4、128 位 RC4、DESX、128 位 AES、192 位 AES 和 256 位 AES。  
  
> [!NOTE]  
>  从 [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)]开始，除 AES_128、AES_192 和 AES_256 以外的所有算法都已过时。 若要使用旧算法（不推荐），必须将数据库设置为兼容级别 120 或更低。  
  
 没有一种算法能够解决所有问题，有关每种算法的优势的说明不属于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书的讨论范畴。 但是，下列一般原则适应于：  
  
-   强加密通常会比较弱的加密占用更多的 CPU 资源。  
  
-   长密钥通常会比短密钥生成更强的加密。  
  
-   非对称加密比对称加密速度慢。  
  
-   复杂的长密码比短密码更强。  

-   如果密钥仅存储在本地，通常推荐使用对称加密，如果需要无线共享密钥，则推荐使用非对称加密。
  
-   如果您正在加密大量数据，应使用对称密钥来加密数据，并使用非对称密钥来加密该对称密钥。  
  
-   不能压缩已加密的数据，但可以加密已压缩的数据。 如果使用压缩，应在加密前压缩数据。  
  
> [!IMPORTANT]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 和更高版本中，可以在任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
>   
>  对不同数据块重复使用相同的 RC4 或 RC4_128 KEY_GUID 将导致产生相同的 RC4 密钥，因为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不自动提供 salt。 重复使用相同的 RC4 密钥是已知错误，将导致加密非常不可靠。 因此，不推荐使用 RC4 和 RC4_128 关键字。 [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 有关加密算法和加密技术的详细信息，请参阅 MSDN 的 .NET Framework 开发人员指南中的 [重要安全性概念](/previous-versions/aa720225(v=vs.71)) 。  
  
 **关于 DES 算法的说明：**  
  
-   DESX 的命名不正确。 使用 ALGORITHM = DESX 创建的对称密钥实际上使用的是具有 192 位密钥的 TRIPLE DES 密码。 不提供 DESX 算法。 [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   使用 ALGORITHM = TRIPLE_DES_3KEY 创建的对称密钥使用的是具有 192 位密钥的 TRIPLE DES。  
  
-   使用 ALGORITHM = TRIPLE_DES 创建的对称密钥使用的是具有 128 位密钥的 TRIPLE DES。  
  
## <a name="related-tasks"></a>Related Tasks  
  
| 任务 | 类型 |
| ---- | ---- |
|使用对称密钥加密。|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|使用非对称密钥加密。|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|使用证书加密。|[CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|使用透明数据加密对数据库文件进行加密。|[透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|如何加密表中的列。|[加密数据列](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 加密](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [加密层次结构](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
