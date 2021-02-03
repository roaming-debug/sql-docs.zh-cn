---
description: CREATE MASTER KEY (Transact-SQL)
title: CREATE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_MASTER_KEY_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- CREATE MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], Database Master Key
- database master key [SQL Server]
- CREATE MASTER KEY statement
- cryptography [SQL Server], Database Master Key
- database master key [SQL Server], creating
ms.assetid: 1710a305-1a4f-48ec-836c-11ffd0356d76
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b77069fd4665ea72988f3eba5aee6c790c6c181e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188595"
---
# <a name="create-master-key-transact-sql"></a>CREATE MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在 master 数据库中，创建数据库主密钥。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
CREATE MASTER KEY [ ENCRYPTION BY PASSWORD ='password' ]
[ ; ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

PASSWORD ='password' 是用于加密数据库主密钥的密码。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。 password 在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]和 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 中是可选的。

## <a name="remarks"></a>注解

数据库主密钥是指用于保护证书私钥的对称密钥以及数据库中存在的非对称密钥。 当创建主密钥时，会使用 AES_256 算法以及用户提供的密码对其进行加密。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 中，不使用 Triple DES 算法。 若要启用主密钥的自动解密功能，请使用服务主密钥对该主密钥的副本进行加密，并将副本存储在数据库和 master 中。 通常，每当主密钥更改时，便会在不进行提示的情况下更新存储在 master 中的副本。 可以使用 [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md) 的 DROP ENCRYPTION BY SERVICE MASTER KEY 选项对该默认行为进行更改。 必须使用 [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md) 语句和密码打开未使用服务主密钥进行加密的主密钥。

master 中 sys.databases 目录视图的 is_master_key_encrypted_by_server 列指示是否使用服务主密钥对数据库主密钥进行加密。

可以在 sys.symmetric_keys 目录视图中查看有关数据库主密钥的信息。

对于 SQL Server 和并行数据仓库，通常用服务主密钥和至少一个密码保护主密钥。 如果以物理方式（日志传送、还原备份等）将数据库移动到另一个服务器，数据库将包含主密钥的两份副本，一份由原始服务器服务主密钥进行加密（除非已使用 `ALTER MASTER KEY DDL` 显式删除此加密），一份由在 `CREATE MASTER KEY` 或后续 `ALTER MASTER KEY DDL` 操作过程中指定的每个密码进行加密。 移动数据库后，若要恢复主密钥以及所有数据（使用主密钥加密为密钥层次结构中的根），用户需要通过用于保护主密钥的其中一个密码，使用 `OPEN MASTER KEY` 语句在新服务器上还原主密钥的备份，或在新服务器上还原原始服务主密钥的备份。

对于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]和 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]，当数据库从一个服务器移动到另一个服务器时，密码保护不能视为防止数据丢失的安全机制，因为主密钥的服务主密钥保护由 Microsoft Azure 平台进行管理。 因此，主密钥密码在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 和 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 中是可选的。

> [!IMPORTANT]
> 你应该使用 [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md) 备份主密钥，并将备份存储于另外一个安全的位置中。

服务主密钥和数据库主密钥是通过使用 AES-256 算法进行保护的。

## <a name="permissions"></a>权限

要求对数据库具有 CONTROL 权限。

## <a name="examples"></a>示例

使用以下示例在 `master` 数据库中创建数据库主密钥。 该密钥使用密码 `23987hxJ#KL95234nl0zBe` 进行加密。

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';
GO
```

## <a name="see-also"></a>另请参阅

- [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)
- [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)
- [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)
- [DROP MASTER KEY (Transact-SQL)](../../t-sql/statements/drop-master-key-transact-sql.md)
- [CLOSE MASTER KEY (Transact-SQL)](../../t-sql/statements/close-master-key-transact-sql.md)
- [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)
