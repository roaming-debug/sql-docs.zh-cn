---
title: 加密层次结构 | Microsoft Docs
description: 了解 SQL Server 中的分层加密和密钥管理基础结构。 将密钥存储在可扩展密钥管理模块中。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], hierarchies
- cryptography [SQL Server], hierarchies
- encryption keys [SQL Server]
- security [SQL Server], encryption
- hierarchies [SQL Server], encryption
ms.assetid: 96c276d5-1bba-4e95-b678-10f059f1fbcf
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c47cb3825c18b518c547e7c88c3c89e0c2f3c23a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477538"
---
# <a name="encryption-hierarchy"></a>加密层次结构
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用分层加密和密钥管理基础结构来加密数据。 每一层都使用证书、非对称密钥和对称密钥的组合对它下面的一层进行加密。 非对称密钥和对称密钥可以存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之外的可扩展密钥管理 (EKM) 模块中。  
  
 下图说明了加密层次结构的每一层是如何对它下面的一层进行加密的，并且显示了最常用的加密配置。 对层次结构的开始进行的访问通常受密码保护。  
  
 ![以堆积图形式显示一些加密组合。](../../../relational-databases/security/encryption/media/encryption-hierarchy-stack.gif "以堆积图形式显示一些加密组合。")  
  
 请记住以下概念：  
  
-   为了获得最佳性能，使用对称密钥（而不是证书或非对称密钥）加密数据。  
  
-   数据库主密钥受服务主密钥保护。 服务主密钥由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序创建，并且使用 Windows 数据保护 API (DPAPI) 进行加密。  
  
-   堆叠其他层的其他加密层次结构是可能的。  
  
-   可扩展密钥管理 (EKM) 模块将对称密钥或非对称密钥保存在 SQL Server 的外部。  
  
-   透明数据加密 (TDE) 必须使用称为数据库加密密钥的对称密钥，该密钥受由 master 数据库的数据库主密钥保护的证书保护，或者受存储在 EKM 中的非对称密钥保护。  
  
-   服务主密钥和所有数据库主密钥是对称密钥。  
  
 下图以另一种方式显示了相同的信息。  
  
 ![以轮形图形式显示一些加密组合。](../../../relational-databases/security/encryption/media/encryption-hierarchy-wheel.gif "以轮形图形式显示一些加密组合。")  
  
 此图说明了以下其他概念：  
  
-   在此图中，箭头表示常用的加密层次结构。  
  
-   EKM 中的对称密钥和非对称密钥可以保护对存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的对称密钥和非对称密钥进行的访问。 与 EKM 有关的虚线表示 EKM 中的密钥可以替换存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的对称密钥和非对称密钥。  
  
## <a name="encryption-mechanisms"></a>加密机制  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供了下列加密机制：  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] 函数  
  
-   非对称密钥  
  
-   对称密钥  
  
-   证书  
  
-   透明数据加密  
  
### <a name="transact-sql-functions"></a>Transact-SQL 函数  
 插入或更新项时可使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 函数对各个项进行加密。 有关详细信息，请参阅 [ENCRYPTBYPASSPHRASE (Transact SQL)](../../../t-sql/functions/encryptbypassphrase-transact-sql.md) 和 [DECRYPTBYPASSPHRASE (Transact SQL)](../../../t-sql/functions/decryptbypassphrase-transact-sql.md)。  
  
### <a name="certificates"></a>证书  
 公钥证书（通常只称为证书）是一个数字签名语句，它将公钥的值绑定到拥有对应私钥的人员、设备或服务的标识上。 证书是由证书颁发机构 (CA) 颁发和签名的。 从 CA 接收证书的实体是该证书的主体。 证书中通常包含下列信息。  
  
-   主体的公钥。  
  
-   主体的标识符信息，如姓名和电子邮件地址。  
  
-   有效期。 这是指证书被认为有效的时间长度。  
  
     证书只有在指定的有效期内有效，每个证书都包含一个“有效期始于”  和“有效期至”  日期。 这两个日期设置了有效期的界限。 证书超过有效期后，必须由已过期证书的主体请求一个新证书。  
  
-   颁发者标识符信息。  
  
-   颁发者的数字签名。  
  
     此签名用于证明主体的公钥和标识符信息之间的绑定的有效性。 （在对信息进行数字签名的过程中，信息以及发件人拥有的一些秘密信息将被转换成一个称为“签名”的标记。）  
  
 证书的主要好处是使主机不再需要为每个主体维护一组密码。 相反，主机只需要与证书颁发者建立信任关系，然后证书颁发者就可以签名无限数量的证书。  
  
 当主机（如安全 Web 服务器）将某个颁发者指定为受信任的根颁发机构时，主机将隐式信任该颁发者用来建立它所发出的证书绑定的策略。 也就是说，主机将相信该颁发者已经验证了证书主体的标识。 主机可以通过将颁发者自签名的证书（其中包含颁发者的公钥）放入主机的受信任根证书颁发机构证书存储区，将此颁发者指定为受信任的根颁发机构。 对于中间证书颁发机构或从属证书颁发机构，只有当它们具有受信任根证书颁发机构的合法路径时才会受到信任。  
  
 颁发者可以在证书到期之前便撤消该证书。 撤消后，将解除公钥与证书中声明的标识之间的绑定。 每个颁发者都维护一个证书撤消列表，此列表可由程序在检查任何给定证书的有效性时使用。  
  
 由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 创建的自签名证书遵循 X.509 标准并支持 X.509 v1 字段。  
  
### <a name="asymmetric-keys"></a>非对称密钥  
 非对称密钥由私钥和对应的公钥组成。 每个密钥都可以解密另一个密钥加密的数据。 非对称加密和解密相对来说会消耗大量资源，但它们比对称加密提供了更高的安全级别。 非对称密钥可用于加密对称密钥，以便存储在数据库中。  
  
### <a name="symmetric-keys"></a>对称密钥  
 对称密钥是加密和解密都使用的一个密钥。 使用对称密钥进行加密和解密非常快，适用于对数据库中敏感数据的日常使用。  
  
### <a name="transparent-data-encryption"></a>透明数据加密  
 透明数据加密 (TDE) 是使用对称密钥进行加密的一种特殊情况。 TDE 使用称为数据库加密密钥的对称密钥加密整个数据库。 数据库加密密钥受由数据库主密钥或存储在 EKM 模块中的非对称密钥保护的其他密钥或证书保护。 有关详细信息，请参阅[透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)。  
  
## <a name="related-content"></a>相关内容  
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [安全函数 (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [权限层次结构（数据库引擎）](../../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [安全对象](../../../relational-databases/security/securables.md)  
  
  
