---
description: 通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据
title: 通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据 | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 89592db85cd95996274484d17fd968d70a552373
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88423371"
---
# <a name="migrate-data-to-or-from-columns-using-always-encrypted-with-sql-server-import-and-export-wizard"></a>通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

[SQL Server 导入和导出向导](../../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)是一种使你可以将数据从源复制到目标的工具。 本文档介绍在源和/或目标是包含使用 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 保护的列的 SQL Server 数据库时，如何使用 SQL Server 导入和导出向导。

## <a name="migration-scenarios"></a>迁移方案
通过 SQL Server 导入和导出向导，可以实现以下方案以便在加密列之间迁移数据。

### <a name="encrypt-plaintext-data-on-migration"></a>在迁移时加密纯文本数据
如果数据源包含纯文本数据，并且目标是包含加密列的 SQL Server 数据库，则可以使用 SQL Server 导入和导出向导从源检索纯文本数据，对其进行加密，然后将加密数据（已加密文本）复制到目标数据库中的加密列。 对于此迁移方案，数据源可以是 SQL Server 导入和导出向导支持的任何数据存储。 例如，文件、SQL Server 数据库或其他数据库系统中的数据库。

若要确保 SQL Server 导入和导出向导可以加密数据，需要为目标数据库连接启用 Always Encrypted，并且需要有权访问用于保护目标数据库列中数据的密钥。 有关详细信息，请参阅[为数据库连接启用和禁用 Always Encrypted](#enable-and-disable-always-encrypted-for-a-database-connection) 和[在迁移过程中加密或解密数据的权限](#permissions-for-encrypting-or-decrypting-data-during-migration)。

### <a name="decrypt-encrypted-data-on-migration"></a>在迁移时解密加密数据
如果要迁移存储在 SQL Server 数据库的加密数据库列中的数据，则可以配置 SQL Server 导入和导出向导，以便解密数据并将解密（纯文本）数据复制到目标（可以是 SQL Server 导入和导出向导支持的任何数据存储，例如文件、SQL Server 数据库或其他数据库系统中的数据库）。

若要确保 SQL Server 导入和导出向导可以解密数据，需要为源数据库连接启用 Always Encrypted，并且需要有权访问用于保护源数据库列中数据的密钥。 有关详细信息，请参阅[为数据库连接启用和禁用 Always Encrypted](#enable-and-disable-always-encrypted-for-a-database-connection) 和[在迁移过程中加密或解密数据的权限](#permissions-for-encrypting-or-decrypting-data-during-migration)。

### <a name="re-encrypt-data-on-migration"></a>在迁移时重新加密数据
如果要将数据从源 SQL Server 数据库中的加密列复制到相同或其他 SQL Server 数据库中的加密列，则可以配置 SQL Server 导入和导出向导，以便在从源检索数据之后解密数据，并在将数据插入目标数据库中的加密列之前重新加密数据。 如果目标列的架构（例如，列数据类型、加密类型和列加密密钥）与源列的架构不同，请使用此方法。

若要确保 SQL Server 导入和导出向导可以加密和解密数据，需要为源数据库连接和目标数据库连接启用 Always Encrypted，并且需要有权访问用于保护源和目标数据库列中数据的密钥。 有关详细信息，请参阅[为数据库连接启用和禁用 Always Encrypted](#enable-and-disable-always-encrypted-for-a-database-connection) 和[在迁移过程中加密或解密数据的权限](#permissions-for-encrypting-or-decrypting-data-during-migration)。

### <a name="keep-data-encrypted-during-migration"></a>使数据在迁移过程中保持加密
如果要将数据从源 SQL Server 数据库中的加密列复制到相同或其他 SQL Server 数据库中的加密列，并且目标列使用与源列完全相同的架构（包括相同数据类型、加密类型和列加密密钥），则可以配置 SQL Server 导入和导出向导，以便从源列中检索已加密文本，并将加密数据（已加密文本）插入目标 SQL Server 数据库中的加密列。 

对于此方案，可以使用支持 SQL Server 的任何数据提供程序连接到源或目标 SQL Server 数据库。 如果使用的提供程序支持 Always Encrypted 连接到目标数据库，则需要确保为数据库连接禁用 Always Encrypted。 有关详细信息，请参阅[为数据库连接启用和禁用 Always Encrypted](#enable-and-disable-always-encrypted-for-a-database-connection)。

还需要确保 SQL Server 导入和导出向导用于连接到目标数据库的数据库主体（用户）使用设置为 `ON` 的 `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` 选项进行配置。 此选项会取消在大容量复制操作中，在服务器上进行的加密元数据检查，这使向导可以将加密数据大容量插入目标数据库，而无需解密数据。 有关详细信息，请参阅[将加密数据大容量加载到受 Always Encrypted 保护的列](migrate-sensitive-data-protected-by-always-encrypted.md)。

## <a name="enable-and-disable-always-encrypted-for-a-database-connection"></a>为数据库连接启用和禁用 Always Encrypted
如果迁移方案需要 SQL Server 导入和导出向导能够加密和/或解密数据，则需要使用支持 Always Encrypted 的数据提供程序配置源 SQL Server 数据库连接和/或目标 SQL Server 数据库连接。 还需要为源和/或目标数据库连接启用 Always Encrypted。

如果不需要向导在连接上加密或解密数据，则可以使用任何数据提供程序进行连接。

SQL Server 导入和导出向导中的以下数据提供程序支持 Always Encrypted。

- 用于 SQL Server 的 .NET Framework 数据访问接口
  - 请确保运行向导的计算机使用 .NET Framework 4.6.1 或更高版本。
  - 若要为连接启用 Always Encrypted，请在连接属性中将 `Column Encryption Setting` 设置为 `Enabled`。 若要禁用 Always Encrypted，请将 `Column Encryption Setting` 设置为 `Disabled`。 有关详细信息，请参阅[使用用于 SQL Server 的 .NET Framework 数据提供程序连接到 SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server) 和[为应用程序查询启用 Always Encrypted](develop-using-always-encrypted-with-net-framework-data-provider.md#enabling-always-encrypted-for-application-queries)。
- 用于 ODBC 的 .NET Framework 数据提供程序。
  - 安装 Microsoft ODBC 驱动程序 13.1 或更高版本。
    - 若要为连接启用 Always Encrypted，请在连接属性中将 `Column Encryption` 设置为 `Enabled`。 若要禁用 Always Encrypted，请将 `Column Encryption` 设置为 `Disabled`。 有关详细信息，请参阅[使用用于 SQL Server 的 ODBC 驱动程序连接到 SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-odbc-driver-for-sql-server) 和[在 ODBC 应用程序中启用 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-in-an-odbc-application)。

## <a name="permissions-for-encrypting-or-decrypting-data-during-migration"></a>在迁移过程中加密或解密数据的权限

若要加密或解密 SQL Server 源或目标数据库中存储的数据，需要源数据库中的“查看任意列主密钥定义”和“查看任意列加密密钥定义”权限。

还需要有权访问为存储要加密或解密的数据的列配置的列主密钥：

- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- Azure Key Vault  - 需要包含列主密钥的保管库上的 get、unwrapKey 和 verify 权限。
- 密钥存储提供程序(CNG) - 所需权限和凭据；使用密钥存储或密钥时可能会提示你，具体取决于存储和 KSP 配置。
- 加密服务提供程序(CAPI) - 所需权限和凭据；使用密钥存储或密钥时可能会提示你，具体取决于存储和 CSP 配置。
有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

## <a name="next-steps"></a>后续步骤
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](always-encrypted-database-engine.md)
- [导出和导入使用 Always Encrypted 的数据库](always-encrypted-migrate-using-bacpac.md)
- [备份和还原使用 Always Encrypted 的数据库](always-encrypted-migrate-using-backup-restore.md)
- [使用 Always Encrypted 将加密数据批量加载到列中](migrate-sensitive-data-protected-by-always-encrypted.md)
