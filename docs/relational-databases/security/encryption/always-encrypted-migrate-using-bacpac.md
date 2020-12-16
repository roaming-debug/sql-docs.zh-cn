---
description: 导出和导入使用 Always Encrypted 的数据库
title: 导出和导入使用 Always Encrypted 的数据库 | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4260afaf34e531f219e7a4b76c51dca4b0179910
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480888"
---
# <a name="export-and-import-databases-using-always-encrypted"></a>导出和导入使用 Always Encrypted 的数据库 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文介绍如何导出和导入包含使用 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 保护的列的数据库。

导出数据库时，系统会以加密形式（已加密文本）从数据库中检索加密列中存储的所有数据并将其放入生成的 [BACPAC](../../data-tier-applications/data-tier-applications.md) 中。 生成的 BACPAC 还包含始终加密密钥的元数据。

将 BACPAC 导入数据库时，BACPAC 中的加密数据将加载到数据库中，并将重新创建始终加密密钥元数据。 

如果应用程序配置为查询源数据库（已导出的数据库）中存储的加密列，则无需执行任何特殊操作，即可让应用程序查询目标数据库中的加密数据，因为这两个数据库中的密钥相同。

有关如何导出和导入数据库的详细信息，请参阅：
- [导出数据层应用程序](../../data-tier-applications/export-a-data-tier-application.md)
- [导入 BACPAC 文件以创建新的用户数据库](../../data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)
- [将 Azure SQL 数据库导出到 BACPAC 文件](/azure/sql-database/sql-database-export)
- [将 BACPAC 文件导入到 Azure SQL 数据库中的数据库](/azure/sql-database/sql-database-import)
- [SqlPackage.exe](../../../tools/sqlpackage.md)

## <a name="permissions-for-migrating-databases-with-encrypted-columns"></a>迁移包含加密列的数据库所需的权限

你需要源数据库上的 *更改任意列主密钥* 和 *更改任意列加密密钥* 。 你需要目标数据库上的“更改任意列主密钥”、“更改任意列加密密钥”、“查看任意列主密钥定义”和“查看任意列加密定义”。

不需要可以访问为加密列配置的列主密钥，因为在导出和导入操作期间数据会保持加密。

## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [备份和还原使用 Always Encrypted 的数据库](always-encrypted-migrate-using-backup-restore.md)
- [通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据](always-encrypted-migrate-using-import-export-wizard.md)
- [使用 Always Encrypted 将加密数据批量加载到列中](migrate-sensitive-data-protected-by-always-encrypted.md)