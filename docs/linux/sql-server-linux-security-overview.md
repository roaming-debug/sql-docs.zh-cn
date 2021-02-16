---
title: Linux 上 SQL Server 的安全限制
description: 了解 Linux 上 SQL Server 的限制，包括如何使用 Azure Key Vault 中存储的密钥以及不支持可扩展密钥管理。
author: VanMSFT
ms.author: vanto
ms.date: 09/12/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 64da74cc-14bf-4636-a55e-8cc1fce2aaff
ms.openlocfilehash: 68aa25c93a500b90b778be1563b0d373b07653cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346386"
---
# <a name="security-limitations-for-sql-server-on-linux"></a>Linux 上 SQL Server 的安全限制

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Linux 上的 SQL Server 当前具有以下限制：

* 提供了标准密码策略。 MUST_CHANGE 是可以配置的唯一选项。 不支持 CHECK_POLICY 选项。
* 不支持可扩展密钥管理。 
* 不支持使用 Azure Key Vault 中存储的密钥。
* SQL Server 生成自己的自签名证书，用于加密连接。 可以将 SQL Server 配置为使用用户提供的用于 TLS 的证书。 

有关 SQL Server 提供的安全功能的详细信息，请参阅[SQL Server 数据库引擎和 Azure SQL 数据库的安全中心](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)。

## <a name="next-steps"></a>后续步骤

有关常见安全任务，请参阅 [Linux 上的 SQL Server 的安全功能入门](sql-server-linux-security-get-started.md)。 有关更改 TCP 端口号、SQL Server 目录和配置跟踪标志或排序规则的脚本，请参阅 [使用 mssql conf 配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md)。
