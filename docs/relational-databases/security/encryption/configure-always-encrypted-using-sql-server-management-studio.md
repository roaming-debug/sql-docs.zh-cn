---
title: 《Configure Always Encrypted using SSMS》（使用 SSMS 配置“始终加密”功能）
description: 介绍使用 SQL Server Management Studio (SSMS) 配置和管理 Always Encrypted 数据库时需要执行的任务。
ms.custom: seo-lt-2019
ms.date: 10/31/2019
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
ms.openlocfilehash: fc8018153ca9f3ee87db9d9e19b4a696537e9e38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345409"
---
# <a name="configure-always-encrypted-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 配置 Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文介绍使用 [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md)配置始终加密和管理使用始终加密的数据库时所需执行的任务。

## <a name="security-considerations-when-using-ssms-to-configure-always-encrypted"></a>使用 SSMS 配置 Always Encrypted 时的安全注意事项

当你使用 SSMS 配置始终加密时，SSMS 会处理始终加密密钥和敏感数据，因此，这些密钥和数据会以纯文本形式显示在 SSMS 进程内。 因此，请务必在安全计算机上运行 SSMS。 如果数据库托管在 SQL Server 中，请确保 SSMS 不是在托管 SQL Server 实例的计算机上运行，而是在另一台计算机上运行。 由于始终加密的主要目的是确保加密的敏感数据的安全（即使数据库系统遭到入侵），因此，在 SQL Server 计算机上执行 PowerShell 脚本来处理密钥或敏感数据会减少或抵消该功能带来的益处。 有关其他建议，请参阅 [密钥管理安全注意事项](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。

SSMS 不支持管理数据库的用户 (DBA) 与管理加密密码并有权访问纯文本数据的用户（安全管理员和/或应用程序管理员）之间的角色分离。 如果你的组织强制实施角色分离，你应使用 PowerShell 来配置始终加密。 有关详细信息，请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)和[使用 PowerShell 配置 Always Encrypted](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。 

## <a name="always-encrypted-tasks-using-ssms"></a>使用 SSMS 的 Always Encrypted 任务

- [使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)
- [使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)
- [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
- [使用 Always Encrypted 和 DAC 包配置列加密](configure-always-encrypted-using-dacpac.md)
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)
- [导出和导入使用 Always Encrypted 的数据库](always-encrypted-migrate-using-bacpac.md)
- [备份和还原使用 Always Encrypted 的数据库](always-encrypted-migrate-using-backup-restore.md)
- [通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)
