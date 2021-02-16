---
title: 开始在云中使用 SQL Server（Linux 上）
titleSuffix: SQL Server
description: 了解如何在所选的云中的 Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 或 Ubuntu 上安装 SQL Server。
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: ae7502fcba11d9892cbcf8ed2c94ef205e0abc53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273128"
---
# <a name="quickstart-run-sql-server-in-the-cloud"></a>快速入门：在云中运行 SQL Server
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

在本快速入门中，你将在所选的云中的 Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 或 Ubuntu 上安装 SQL Server。 转到[在 Azure 门户中预配 Linux SQL Server 虚拟机](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%252fsql%252ftoc%252ftoc.json)，在 Azure 中运行 Linux 上的 SQL Server。

> [!NOTE]
> 如果选择运行付费版本的 SQL Server，则需要自带许可证 (BYOL)。

## <a name="amazon-web-services"></a>Amazon Web Services
1.  从市场中使用至少 2 GB 内存来创建 Linux AMI 
    * [RHEL 7.3+](https://aws.amazon.com/marketplace/pp/B00KWBZVK6)
    * [SLES v12 SP2+](https://aws.amazon.com/marketplace/pp/B00PMM99PI)
    * [Ubuntu 16.04](https://aws.amazon.com/marketplace/pp/B01JBL2M0O)
1.  使用 ssh 连接到 AMI
1.  按照所选的 Linux 分发的快速入门进行操作： 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  配置远程连接： 
    * 打开 [Amazon EC2 控制台]( https://console.aws.amazon.com/ec2/)
    * 在“导航”窗格中，选择“安全组”  。 
    * 选择“入站”、“编辑”和“添加规则” 
    * 添加入站规则以允许 SQL Server 侦听的端口（默认 TCP 端口 1433）上有流量

    
## <a name="digital-ocean"></a>Digital Ocean
1. 登录到[控制面板](https://cloud.digitalocean.com/login)，然后单击创建 Droplet
1. 选择至少具有 2 GB 内存的 Ubuntu 16.04 Droplet
1. 通过 ssh 连接到 Droplet
1. 按照 [Ubuntu 快速入门](quickstart-install-connect-ubuntu.md)操作
1. 配置远程连接：
    * 在控制面板顶部，点击“网络”链接，然后选择“防火墙”  
    * 添加入站规则以允许 SQL Server 侦听的端口（默认 TCP 端口 1433）上有流量
    
## <a name="google-cloud-platform"></a>Google Cloud Platform
1.  从云启动器中使用至少 2 GB 内存来创建 Linux 映像 
    * [RHEL 7.3+](https://console.cloud.google.com/launcher/details/rhel-cloud/rhel-7)
    * [SLES v12 SP4](https://console.cloud.google.com/launcher/details/suse-cloud/sles-12)
    * [Ubuntu 16.04](https://console.cloud.google.com/launcher/details/ubuntu-os-cloud/ubuntu-xenial)
1.  使用 ssh 连接到映像
1.  按照所选的 Linux 分发的快速入门进行操作： 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  配置远程连接： 
    * 转到[防火墙规则](https://console.cloud.google.com/networking/firewalls)
    * 添加入站规则以允许 SQL Server 侦听的端口上有流量（默认 tcp：1433）