---
title: 使用 cron 在 Linux 上计划 SSIS 包
description: 本文介绍如何使用 cron 服务在 Linux 上计划 SQL Server Integration Services (SSIS) 包。
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 75ac9be9d128f76157cee054674ecfe29de06063
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346435"
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>使用 cron 在 Linux 上计划 SQL Server Integration Services 包执行

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

在 Windows 上运行 SQL Server Integration Services (SSIS) 和 SQL Server 时，可使用 SQL Server 代理自动执行 SSIS 包。 但是，在 Linux 上运行 SQL Server 和 SSIS 时，SQL Server 代理实用工具无法用于在 Linux 上计划作业。 可改为使用 cron 服务，其在 Linux 平台上广泛用于自动执行包。

本文提供演示如何自动执行 SSIS 包的示例。 这些示例为在 Red Hat Enterprise 上运行而编写。 代码与其他 Linux 分发版（例如 Ubuntu）类似。

## <a name="prerequisites"></a>必备条件

在使用 cron 服务运行作业前，请检查它是否在你的计算机上运行。

若要检查 cron 服务的状态，请使用以下命令：`systemctl status crond.service`。

如果服务未处于活动状态（即未在运行），请咨询管理员以正确设置和配置 cron 服务。

## <a name="create-jobs"></a>创建作业

Cron 作业是可以配置为按指定间隔定期运行的任务。 作业可以像通常直接在控制台中键入的命令一样简单，也可以作为 shell 脚本运行。

为了便于管理和维护，我们建议将包执行命令放在包含描述性名称的脚本中。

以下是用于运行包的简单 shell 脚本的示例。 它仅包含一个命令，但你可以根据需要添加更多命令。

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>使用 cron 服务计划作业

定义作业后，可以使用 cron 服务将其计划为自动运行。

若要添加作业以供 cron 运行，请在 crontab 文件中添加该作业。 若要在可以添加或更新作业的编辑器中打开 crontab 文件，请使用以下命令：`crontab -e`。

若要将上述作业计划为每天凌晨 2:10 运行，请将以下行添加到 crontab 文件中：

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

保存 crontab 文件，然后退出编辑器。

若要了解示例命令的格式，请查看以下部分中的信息。
 
## <a name="format-of-a-crontab-file"></a>crontab 文件的格式

下图显示了添加到 crontab 文件中的作业行的格式说明。

![crontab 文件中的条目的格式说明](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

若要获得有关 crontab 文件格式的更加详细的说明，请使用以下命令：`man 5 crontab`。

以下是有助于解释本文示例的输出的部分示例：

![crontab 格式的详细部分说明](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的相关内容
-   [使用 SSIS 在 Linux 上提取、转换和加载数据](sql-server-linux-migrate-ssis.md)
-   [在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)
-   [使用 ssis-conf 在 Linux 上配置 SQL Server Integration Services](sql-server-linux-configure-ssis.md)
-   [适用于 Linux 上的 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md)
