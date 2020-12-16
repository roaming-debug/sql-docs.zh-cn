---
title: 复制加密列 (SSMS)
description: 了解如何使用 SQL Server Management Studio (SSMS) 复制加密列中的数据。
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], replicating data
- encryption [SQL Server replication]
- publishing [SQL Server replication], encrypted columns
ms.assetid: d1f8f586-e5a3-4a71-9391-11198d42bfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: aa4fafaf27c7292ff9e44181c446d7f845d87b9c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468768"
---
# <a name="replicate-data-in-encrypted-columns-sql-server-management-studio"></a>复制加密列中的数据 (SQL Server Management Studio)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  通过使用复制，您可以发布加密的列数据。 若要在订阅服务器上解密并使用此数据，则订阅服务器上也必须有用于在发布服务器上加密该数据的密钥。 复制并不提供安全机制来传输加密密钥。 您必须在订阅服务器上手动重新创建加密密钥。 本主题说明如何在发布服务器上加密列并确保订阅服务器上提供有加密密钥。  
  
 基本步骤如下所述：  
  
1.  在发布服务器上创建对称密钥。  
  
2.  使用对称密钥加密列数据。  
  
3.  发布包含加密列的表。  
  
4.  订阅发布。  
  
5.  初始化订阅。  
  
6.  使用与步骤 1 中相同的 ALGORITHM、KEY_SOURCE 和 IDENTITY_VALUE 值在订阅服务器上重新创建对称密钥。  
  
7.  访问加密列数据。  

> [!NOTE]  
>  您应当使用对称密钥来加密列数据。 可以在发布服务器和订阅服务器上采取不同方式为对称密钥本身进行保护。  
  
### <a name="to-create-and-replicate-encrypted-column-data"></a>创建和复制加密列数据  
  
1.  在发布服务器上，执行 [CREATE SYMMETRIC KEY](../../../t-sql/statements/create-symmetric-key-transact-sql.md)。  
  
    > [!IMPORTANT]  
    >  KEY_SOURCE 值是很重要的数据，可用来重新创建对称密钥和解密数据。 必须始终安全地存储和传输 KEY_SOURCE。  
  
2.  执行 [OPEN SYMMETRIC KEY](../../../t-sql/statements/open-symmetric-key-transact-sql.md) 以打开新密钥。  
  
3.  使用 [EncryptByKey](../../../t-sql/functions/encryptbykey-transact-sql.md) 函数在发布服务器上加密列数据。  
  
4.  执行 [CLOSE SYMMETRIC KEY](../../../t-sql/statements/close-symmetric-key-transact-sql.md) 以关闭密钥。  
  
5.  发布包含加密列的表。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
6.  订阅发布。 有关详细信息，请参阅[创建请求订阅](../../../relational-databases/replication/create-a-pull-subscription.md)或[创建推送订阅](../../../relational-databases/replication/create-a-push-subscription.md)。  
  
7.  初始化订阅。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
8.  在订阅服务器上，使用与步骤 1 中相同的 ALGORITHM、KEY_SOURCE 和 IDENTITY_VALUE 值执行 [CREATE SYMMETRIC KEY](../../../t-sql/statements/create-symmetric-key-transact-sql.md) 。 您可以为 ENCRYPTION BY 指定不同的值。  
  
    > [!IMPORTANT]  
    >  KEY_SOURCE 值是很重要的数据，可用来重新创建对称密钥和解密数据。 必须始终安全地存储和传输 KEY_SOURCE。  
  
9. 执行 [OPEN SYMMETRIC KEY](../../../t-sql/statements/open-symmetric-key-transact-sql.md) 以打开新密钥。  
  
10. 使用 [DecryptByKey](../../../t-sql/functions/decryptbykey-transact-sql.md) 函数在订阅服务器上解密复制的数据。  
  
11. 执行 [CLOSE SYMMETRIC KEY](../../../t-sql/statements/close-symmetric-key-transact-sql.md) 以关闭密钥。  
  
## <a name="examples"></a>示例

### <a name="a-create-keys-in-the-publication-database"></a>A. 在发布数据库中创建密钥
 此示例将创建一个对称密钥、一个用来帮助保护对称密钥的证书和一个主密钥。 这些密钥在发布数据库中创建， 然后可用于在 `SalesOrderHeader` 表中创建加密列 (EncryptedCreditCardApprovalCode)。 将在 AdvWorksSalesOrdersMerge 发布中发布此列而不是未加密的 CreditCardApprovalCode 列。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
 [!code-sql[HowTo#sp_PublishEncryptedColumn](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_1.sql)]  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_2.sql)]  
  
### <a name="b-create-keys-in-the-subscription-database"></a>B. 在订阅数据库中创建密钥
 此示例使用与第一个示例相同的 ALGORITHM、KEY_SOURCE 和 IDENTITY_VALUE 值在订阅数据库中重新创建相同的对称密钥。 此示例假设您已初始化 AdvWorksSalesOrdersMerge 发布的订阅以复制加密列。 如果可能，请在运行时提示用户输入安全凭据。 如果必须将凭据存储在脚本文件中，则必须在存储和传输过程中保护文件以防止未经授权的访问。  
  
 [!code-sql[HowTo#sp_SubscriberEncryptedColumn](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_3.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [在两个服务器上创建相同的对称密钥](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
