---
title: Scale Out 对高可用性的支持 | Microsoft Docs
description: 了解如何配置 SQL Server Integration Services (SSIS) Scale Out Master 端来实现高可用性。
ms.custom: performance
ms.date: 05/23/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: dd53bf6f045687f7d79f6c0e836490adb60cda44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348861"
---
# <a name="scale-out-support-for-high-availability"></a>Scale Out 对高可用性的支持

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



在 SSIS Scale Out 中，通过使用多个 Scale Out Worker 来执行包，可提供 Scale Out Worker 端的高可用性。

Scale Out Master 端的高可用性则通过[针对 SSIS 目录的 Always On](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) 和 Windows 故障转移群集实现。 在此解决方案中，Scale Out Master 的多个实例托管在 Windows 故障转移群集中。 主节点上的 Scale Out Master 服务或 SSISDB 关闭时，辅助节点上的该服务或 SSISDB 将继续接受用户请求并与 Scale Out Worker 通信。

或者，可使用 SQL Server 故障转移群集实例在 Scale Out Master 端实现 高可用性。 请参阅 [Scale Out 通过 SQL Server 故障转移群集实例对高可用性的支持](scale-out-failover-cluster-instance.md)。

要使用针对 SSIS 目录的 Always On 设置 Scale Out Master 端的高可用性，请执行以下步骤：

## <a name="1-prerequisites"></a>1.先决条件
设置 Windows 故障转移群集。 有关说明，请参阅博客文章[安装适用于 Windows Server 2012 的故障转移群集功能和工具](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733)。 在所有群集节点上安装功能和工具。

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2.在主节点上安装 Scale Out Master
在用于 Scale Out Master 的主节点上安装 SQL Server 数据库引擎服务、Integration Services 和 Scale Out Master。 

请在安装过程中执行以下操作：

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1 将运行 Scale Out Master 服务的帐户设置为域帐户
此帐户将来一定能访问 Windows 故障转移群集中辅助节点上 的 SSISDB。 由于 Scale Out Master 服务和 SSISDB 可以分开进行故障转移，因此，故障转移后它们可能位于不同的节点上。

![HA 服务器配置](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2 在 Scale Out Master 证书的 CN 中包含 Scale Out Master 服务的 DNS 主机名

此主机名是 Scale Out Master 终结点，它是作为故障转移群集中的聚集通用服务进行创建（见第 7 步）。   （确保提供 DNS 主机名而非服务器名称。）

![HA 主节点配置](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3.在辅助节点上安装 Scale Out Master
在用于 Scale Out Master 的辅助节点上安装 SQL Server 数据库引擎服务、Integration Services 和 Scale Out Master。 

使用在主节点上使用的相同 Scale Out Master 证书。 使用私钥导出主节点上的 Scale Out Master TLS/SSL 证书，并将它安装到辅助节点上的本地计算机的根证书存储中。 在辅助节点上安装 Scale Out Master 时，请选择此证书。

![HA 主节点配置 2](media/ha-master-config2.PNG)

> [!NOTE]
> 通过对其他辅助节点上的 Scale Out Master 重复这些操作，可设置多个备份 Scale Out Master。

## <a name="4-set-up-and-configure-ssisdb-support-for-always-on"></a>4.为 Always On 设置并配置 SSISDB 支持

按照[针对 SSIS 目录 (SSISDB) 的 Always On](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) 中的说明，为 Always On 设置并配置 SSISDB 支持。

此外，还需为 SSISDB 添加到的可用性组创建可用性组侦听程序。 请参阅[创建或配置可用性组侦听程序](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5.更新 Scale Out Master 服务配置文件
在主节点和辅助节点上更新 Scale Out Master 服务配置文件 `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`。 将 SqlServerName 更新为 [可用性组侦听程序 DNS 名称],[端口]   。

## <a name="6-enable-package-execution-logging"></a>6.启用包执行日志记录

使用登录名 ##MS_SSISLogDBWorkerAgentLogin##（其密码是自动生成的）在 SSISDB 中进行日志记录  。 要让日志记录适用于 SSISDB 的所有副本，请执行以下操作

### <a name="61-change-the-password-of-ms_ssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1 更改主 SQL Server 上 ##MS_SSISLogDBWorkerAgentLogin## 的密码

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2 将该登录名添加到辅助 SQL Server

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3 更新用于日志记录的连接字符串。
使用下列参数值调用存储过程 `[catalog].[update_logdb_info]`：

-   `@server_name = '[Availability Group Listener DNS name],[Port]'`

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster"></a>7.配置 Windows Server 故障转移群集的 Scale Out Master 服务角色操作

1.  在故障转移群集管理器中，连接到 Scale Out 的群集。选择群集。 在菜单中选择“操作”，然后选择“配置角色”   。

2.  在“高可用性向导”对话框的“选择角色”页中，选择“通用服务”    。 在“选择服务”页中，选择 SQL Server Integration Services Scale Out Master 14.0  。

3.  在“客户端访问点”页中，输入 Scale Out Master 服务的 DNS 主机名  。

    ![HA 向导 1](media/ha-wizard1.PNG)

4.  完成该向导。

在 Azure 虚拟机上，此配置步骤需要额外的步骤。 这些概念和这些步骤的完整解释超出了本文的范围。

1.  必须设置 Azure 域。 Windows Server 故障转移群集要求群集中的所有计算机都是同一个域的成员。 有关详细信息，请参阅[使用 Azure 门户启用 Azure Active Directory 域服务](/azure/active-directory-domain-services/create-instance)。

2. 必须设置 Azure 负载均衡器。 这是可用性组侦听程序的一项要求。 有关详细信息，请参阅[教程：使用 Azure 门户通过基本负载均衡器将内部流量在各台 VM 之间进行负载均衡](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)。

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8.在 SSISDB 中更新 Scale Out Master 地址

在主 SQL Server 中，使用参数值 `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'` 运行存储过程 `[catalog].[update_master_address]`。 

## <a name="9-add-the-scale-out-workers"></a>9.添加 Scale Out Worker

现在，可以借助 [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md) 添加 Scale Out Worker。 在连接页中输入 `[SQL Server Availability Group Listener DNS name],[Port]`。

## <a name="upgrade-scale-out-in-high-availability-environment"></a>在高可用性环境中升级 Scale Out
若要在高可用性环境中升级 Scale Out，请按照[适用于 SSIS 目录的 Always On 升级步骤](../catalog/ssis-catalog.md#Upgrade)，在每台计算机上升级 Scale Out 主要角色和 Scale Out 辅助角色，并使用新版 Scale Out 主要角色服务在上述步骤 7 中重新创建 Windows Server 故障转移群集角色。

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅以下文章：
-   [Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)