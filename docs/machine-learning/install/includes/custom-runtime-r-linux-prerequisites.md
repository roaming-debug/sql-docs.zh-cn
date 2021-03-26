---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: f14691931190da9f72d3d54a3f1b4280b679c6e3
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833724"
---
## <a name="prerequisites"></a>必备条件

安装 R 自定义运行时之前，请安装以下各项：

+ 安装适用于 Linux 的 SQL Server 2019。 可以在 Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 版本 12 和 Ubuntu 上安装 SQL Server。 有关更多信息，请参阅 [Linux 上的 SQL Server 的安装指南](../../../linux/sql-server-linux-setup.md)。

+ 升级到 SQL Server 2019 的累积更新 (CU) 3 或更高版本。 执行以下步骤：
    1. 配置用于累积更新的存储库。 有关详细信息，请参阅[配置存储库以便安装和升级 Linux 上的 SQL Server](../../../linux/sql-server-linux-change-repo.md)。

    1. 将 mssql-server 包更新为最新的累积更新。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“更新或升级 SQL Server”部分](../../../linux/sql-server-linux-setup.md#upgrade)。
