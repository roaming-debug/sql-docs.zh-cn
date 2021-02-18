---
title: 重命名故障转移群集实例
description: 本文介绍如何重命名故障转移群集中的 SQL Server 实例，这与重命名独立实例不同。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42fd6dbb5053993d5ad6c2859460e164f476bac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354021"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>重命名 SQL Server 故障转移群集实例
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例包括在故障转移群集中，则重命名虚拟服务器的过程不同于重命名独立实例的过程。 有关详细信息，请参阅 [重命名承载 SQL Server 独立实例的计算机](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)。  
  
 虚拟服务器的名称始终与 SQL 网络名称（SQL 虚拟服务器的网络名称）相同。 尽管您可以更改虚拟服务器的名称，但不能更改实例名。 例如，您可以将名为 VS1\instance1 的虚拟服务器更改为其他名称（例如 SQL35\instance1），但是名称的实例部分 (instance1) 将保持不变。  
  
 开始重命名进程之前，请阅读下列各项。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不支持对复制所涉及的服务器进行重命名。 如果主服务器永久丢失连接，则可以重命名日志传送中的辅助服务器。 有关详细信息，请参阅[日志传送和复制 (SQL Server)](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)。  
  
-   当您重命名被配置为使用数据库镜像的虚拟服务器时，必须在进行重命名操作之前先关闭数据库镜像，然后用新的虚拟服务器名称重新建立数据库镜像。 数据库镜像的元数据将不会自动更新来反映新的虚拟服务器名称。  
  
### <a name="to-rename-a-virtual-server"></a>重命名虚拟服务器  
  
1.  使用群集管理器将 SQL 网络名称更改为新名称。  
  
2.  使网络名称资源脱机。 这将使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源和其他相关资源也脱机。  
  
3.  使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源重新联机。  
  
## <a name="verify-the-renaming-operation"></a>验证重命名操作  
 虚拟服务器被重命名之后，任何使用旧名称的连接现在都必须使用新名称来连接。  
  
 若要验证重命名操作是否已完成，请从 **@@servername** 或 **sys.servers** 中选择信息。 **@@servername** 函数将返回新的虚拟服务器名称，**sys.servers** 表将显示新的虚拟服务器名称。 若要验证故障转移过程是否能够使用新名称正常工作，用户还应尝试将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源故障转移到其他节点。  
  
 对于从群集中任何节点进行的连接，都可以立即使用新名称。 但是，对于从客户端计算机使用新名称进行的连接，则必须在新名称对该客户端计算机可见之后，才能使用新名称连接到服务器。 根据网络配置，通过网络传播新名称所需的时间长度可能为几秒钟，也可能长至 3 到 5 分钟；旧的虚拟服务器名称在网络上不再可见也可能会需要一些时间。  
  
 若要最小化虚拟服务器重命名操作的网络传播延迟，请使用下列步骤：  
  
#### <a name="to-minimize-network-propagation-delay"></a>最小化网络传播延迟  
  
1.  在服务器节点上从命令提示符发出下列命令：  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>在重命名操作之后的其他注意事项  
 在重命名故障转移群集的网络名称后，需要按照下面的说明进行验证和操作，使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理和 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]在所有情况下都正常工作。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理服务：验证和执行以下针对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理服务的附加操作：  
  
-   如果 SQL 代理配置为事件转发，请修复注册表设置。 有关详细信息，请参阅[指定事件转发服务器 (SQL Server Management Studio)](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md)。  
  
-   在重命名计算机/群集网络名称后修复主服务器 (MSX) 和目标服务器 (TSX) 实例名称。 有关详情，请参阅以下主题：  
  
    -   [将多台目标服务器从主服务器脱离](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [创建多服务器环境](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   重新配置日志传送以便更新的服务器名称用于备份和还原日志。 有关详情，请参阅以下主题：  
  
    -   [配置日志传送 (SQL Server)](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [删除日志传送 (SQL Server)](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   更新依赖于服务器的名称的 Jobsteps。 有关详细信息，请参阅 [Manage Job Steps](../../../ssms/agent/manage-job-steps.md)。  
  
## <a name="see-also"></a>另请参阅  
 [重命名托管 SQL Server 独立实例的计算机](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
