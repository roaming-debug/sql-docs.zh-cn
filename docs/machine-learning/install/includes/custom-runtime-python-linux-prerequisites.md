---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 5356e01f91e0df7e653c626403546e27e3697a87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072861"
---
## <a name="prerequisites"></a>必备条件

安装 Python 自定义运行时之前，请安装：

+ 安装适用于 Linux 的 SQL Server 2019。 你可以安装 SQL Server on Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu。 有关更多信息，请参阅 [Linux 上的 SQL Server 的安装指南](../../../linux/sql-server-linux-setup.md)。

+ 升级到 SQL Server 2019 的累积更新 (CU) 3 或更高版本。 执行以下步骤：
    1. 配置用于累积更新的存储库。 有关详细信息，请参阅[配置存储库以便安装和升级 Linux 上的 SQL Server](../../../linux/sql-server-linux-change-repo.md)。

    1. 将 mssql-server 包更新为最新的累积更新。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“更新或升级 SQL Server”部分](../../../linux/sql-server-linux-setup.md#upgrade)。
