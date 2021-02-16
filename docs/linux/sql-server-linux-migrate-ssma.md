---
title: SSMA：自动迁移到 Linux 上的 SQL Server
description: 使用 SQL Server 迁移助手 (SSMA) 自动将 Microsoft Access、DB2、MySQL、Oracle 和 Sybase 数据库迁移到 Linux 上的 SQL Server。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 251bc3af-ebce-4d97-adec-afc0e7fab6cc
ms.openlocfilehash: 7a134cda88301451272644e53cb45c1fb7d3aee0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339108"
---
# <a name="automate-database-migration-to-linux-with-the-sql-server-migration-assistant-ssma"></a>使用 SQL Server 迁移助手 (SSMA) 自动将数据库迁移到 Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍 [SQL Server 迁移助手 (SSMA)](../ssma/sql-server-migration-assistant.md)，它可帮助将数据库从 Microsoft Access、DB2、MySQL、Oracle 和 Sybase 轻松地迁移到 Linux 上的 SQL Server。 SSMA 是一个 Windows 应用程序，因此请在 Windows 计算机可连接到 Linux 上的远程 SQL Server 实例时使用 SSMA。 

SSMA 支持将多种源数据库（包括 Oracle、MySQL、Sybase、DB2 和 Microsoft Access）迁移到 Linux 上的 SQL Server，并且可帮助自动化迁移任务，如：

- 评估源数据库
- 将源数据库架构转换为 Microsoft SQL Server 架构
- 迁移架构
- 迁移数据
- 测试迁移

开始前，需从下面的列表中下载源数据库对应的 SQL Server 迁移助手 (SSMA)：
- [适用于 Access 的 SSMA](https://aka.ms/ssmaforaccess)
- [适用于 DB2 的 SSMA](https://aka.ms/ssmafordb2)
- [适用于 MySql 的 SSMA](https://aka.ms/ssmaformysql) 
- [适用于 Oracle 的 SSMA](https://aka.ms/ssmafororacle)
- [适用于 Sybase ASE 的 SSMA](https://aka.ms/ssmaforsybase) 

接下来，按照 [SQL Server 迁移助手 (SSMA)](../ssma/sql-server-migration-assistant.md) 中的说明将源数据库迁移到 Linux 上的 SQL Server。

## <a name="see-also"></a>另请参阅
- [Microsoft 数据迁移博客](https://blogs.msdn.microsoft.com/datamigration)
- [SQL Server 迁移助手 (SSMA) 博客](/archive/blogs/ssma/)