---
title: 使用 ssis-conf 在 Linux 上配置 SSIS
description: 本文介绍如何使用 ssis-conf 实用工具在 Linux 上配置 SQL Server Integration Services (SSIS)。
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: da546bddb3b7b45753b224539a546f96663704c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348196"
---
# <a name="configure-sql-server-integration-services-on-linux-with-ssis-conf"></a>使用 ssis-conf 在 Linux 上配置 SQL Server Integration Services

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

在为 Red Hat Enterprise Linux 和 Ubuntu 安装 SQL Server Integration Services (SSIS) 时运行 `ssis-conf` 配置脚本。 有关安装 SSIS 的详细信息，请参阅[在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)。

也可以使用 `ssis-conf` 实用工具配置以下属性：

| Command | 说明 |
|-------------|---------------------------------------------------------------------|
| set-edition | 设置 SQL Server 的版本                                       |
| telemetry   | 启用或禁用 SQL Server Integration Services 遥测服务 |
| 安装       | 初始化并设置 Microsoft SQL Server Integration Services      |
|||

## <a name="run-ssis-conf"></a>运行 ssis-conf

本文中的示例通过指定完整路径 `/opt/ssis/bin/ssis-conf` 来运行 `ssis-conf`。 如果在运行 `ssis-conf` 之前导航到该位置，则可以在当前目录 (`./ssis-conf`) 的上下文中运行该实用工具。

确保以根权限运行本文中所述的命令。 例如，运行 `sudo /opt/ssis/bin/ssis-conf setup` 而不是 `/opt/ssis/bin/ssis-conf setup`。

若要通过提示使用你喜欢的语言运行这些命令，可以指定区域设置。 例如，若要接收中文提示，请运行以下命令：`sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`。

## <a name="use-set-edition-to-set-the-edition-of-sql-server-integration-services"></a>使用 set-edition 设置 SQL Server Integration Services 的版本

SSIS 版本与 SQL Server 版本保持一致。

输入以下命令：`$ sudo /opt/ssis/bin/ssis-conf set-edition`。

输入命令后，将收到以下提示：

```
Choose an edition of SQL Server:

1) Evaluation (free, no production use rights, 180-day limit)

2) Developer (free, no production use rights)

3) Express (free)

4) Web (PAID)

5) Standard (PAID)

6) Enterprise (PAID)

7) Enterprise Core (PAID)

8) I bought a license through a retail sales channel and have a product key to enter.

Details about editions can be found at https://go.microsoft.com/fwlink/?LinkId=852748&clcid=0x409.

Use of PAID editions of this software requires separate licensing through a Microsoft Volume Licensing program.

By choosing a PAID edition, you are verifying that you have the appropriate number of licenses in place to install and run this software.

Enter your edition (1-8):
```

如果输入 1 到 7 之间的值，系统将配置免费版或付费版。 如果输入 8，该实用工具会提示你输入已购买的产品密钥：

```
Enter the 25-character product key:
```

## <a name="use-telemetry-to-configure-customer-feedback"></a>使用 telemetry 来配置客户反馈

`telemetry` 命令用于确定 SSIS 是否向 Microsoft 发送反馈。

对于免费版（即 Express、Developer 和 Evaluation Edition），遥测服务始终处于启用状态。 如果有免费版，则无法使用 `telemetry` 命令禁用遥测。

输入以下命令：`$ sudo /opt/ssis/bin/ssis-conf telemetry`。

对于付费版，输入该命令后，将收到以下提示：

```
Send feature usage data to Microsoft. Feature usage data includes information about your hardware configuration and how you use SQL Server Integration Services.

[Yes/No]:
```

如果选择“是”，则会启用并开始运行遥测服务  。 每次引导后，该服务都会自动启动。 如果选择“否”，则会停止并禁用遥测服务  。

## <a name="use-setup-to-initialize-and-set-up-microsoft-sql-server-integration-services"></a>使用 setup 初始化并设置 Microsoft SQL Server Integration Services

每次安装 SSIS 时都使用 `setup` 命令。

输入以下命令：`sudo /opt/ssis/bin/ssis-conf setup`。

该实用工具会提示你确认或提供以下项目的值：
-   产品许可证
-   EULA 协议
-   遥测服务
-   Integration Services 使用的语言

若要通过提示使用你喜欢的语言运行 `setup` 命令，可以指定区域设置。 例如，若要接收中文提示，请运行以下命令：`sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`。

## <a name="ssisconf-format"></a>ssis.conf 格式

以下 `/var/opt/ssis/ssis.conf` 文件提供了每个设置的示例。

对于 SQL Server，可以通过更改 `mssql.conf` 文件中的值来更改系统设置。 对于 SSIS，*不能* 通过更改 `ssis.conf` 文件中的值来更改系统设置。 `ssis.conf` 文件仅显示设置的结果。 若要更改 SSIS 的设置，可以删除 `ssis.conf` 文件并再次运行 `setup` 命令。

下面是一个示例 `ssis.conf` 文件。 每个字段对应于一个设置步骤的结果。

```
[LICENSE]
                       
registered = Y        
                       
pid = enterprisecore  
                       
[EULA]
                       
accepteula = Y        
                       
[TELEMETRY]
                       
enabled = Y           
                       
[language]
                       
lcid = 2052
```

## <a name="related-content-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的相关内容
-   [使用 SSIS 在 Linux 上提取、转换和加载数据](sql-server-linux-migrate-ssis.md)
-   [在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)
-   [适用于 Linux 上的 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md)
-   [使用 cron 在 Linux 上计划 SQL Server Integration Services 包执行](sql-server-linux-schedule-ssis-packages.md)
