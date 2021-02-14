---
description: sp_enclave_send_keys (Transact-SQL)
title: sp_enclave_send_keys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sp_enclave_send_keys
- sp_enclave_send_keys_TSQL
- sys.sp_enclave_send_keys
- sys.sp_enclave_send_keys_TSQL
helpviewer_keywords:
- sp_enclave_send_keys
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: f7730c1f019c55cd307a5bb0be2c55580403457c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075008"
---
# <a name="sp_enclave_send_keys-transact-sql"></a>sp_enclave_send_keys (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

将数据库中定义的列加密密钥发送到与 [安全 enclaves Always Encrypted](../security/encryption/always-encrypted-enclaves.md)一起使用的服务器端安全 enclave。

`sp_enclave_send_keys` 仅只发送启用了 enclave 的密钥，并对使用随机加密的列进行加密并拥有索引。 对于常规用户查询，客户端驱动程序会向 enclave 提供查询中计算所需的键。 `sp_enclave_send_keys` 发送在数据库中定义的所有列加密密钥，并将其用于索引加密列。 

`sp_enclave_send_keys` 提供了一种简单的方法来将密钥发送到 enclave，并填充列加密密钥缓存，以便进行后续索引操作。 使用 `sp_enclave_send_keys` 启用：
- 用于在加密数据库列上重新生成或更改索引或统计信息的 DBA （如果 DBA 无权访问列主密钥 (s) 。 请参阅 [使用缓存的列加密密钥调用索引操作](../security/encryption/always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys)。
- [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 完成对加密列的索引的恢复。 请参阅 [数据库恢复](../security/encryption/always-encrypted-enclaves.md#database-recovery)。
- 使用 SQL Server 的 .NET Framework 数据提供程序的应用程序将数据大容量加载到加密列。

若要成功调用 `sp_enclave_send_keys` ，需要连接到数据库，并为数据库连接启用 Always Encrypted 和 enclave 计算。 还需要有权访问列主密钥，保护列加密密钥，要发送，并且您需要访问数据库中 Always Encrypted 密钥元数据的权限。 

## <a name="syntax"></a>语法  
  
```
sp_enclave_send_keys
[ ;]  
```

## <a name="arguments"></a>参数

此存储过程没有参数。

## <a name="return-value"></a>返回值

此存储过程没有返回值。
  
## <a name="result-sets"></a>结果集

此存储过程没有结果集。
  
## <a name="permissions"></a>权限

 需要 `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` `VIEW ANY COLUMN MASTER KEY DEFINITION` 数据库中的和权限。  
  
## <a name="examples"></a>示例  
  
```sql
EXEC sp_enclave_send_keys;  
```

## <a name="see-also"></a>另请参阅
- [具有安全 Enclave 的 Always Encrypted](../security/encryption/always-encrypted-enclaves.md) 
 
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](../security/encryption/always-encrypted-enclaves-create-use-indexes.md)

- [教程：在使用随机加密且已启用 enclave 的列上创建并使用索引](../security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)
