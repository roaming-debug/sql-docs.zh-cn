---
title: 手动故障转移 FCI - Linux 上的 SQL Server
description: 了解如何在 Linux 上的 SQL Server 上手动故障转移群集实例 (FCI)，尤其是 Red Hat Linux Enterprise、Ubuntu 和 Suse Linux Enterprise Server。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 93bff6d536a6c1c5ad4eb8d8805fefb706377594
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346292"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>运行故障转移群集实例 - Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上运行 SQL Server 故障转移群集实例 (FCI)。 如果尚未在 Linux 上创建 SQL Server FCI，请参阅[配置故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)。 

## <a name="failover"></a>故障转移

FCI 的故障转移类似于 Windows Server 故障转移群集 (WSFC)。 如果托管 FCI 的群集节点遇到某种故障，FCI 应自动故障转移到另一个节点。 与 WSFC 不同，无法设置首选所有者，因此 Pacemaker 选取即将成为 FCI 新主机的节点。

有时，可能需要将 FCI 手动故障转移到另一个节点。 此过程与 WSFC 上的 FCI 不同。 在 WSFC 上，可以在角色级别对资源进行故障转移。 在 Pacemaker 中，可以选择要移动的资源，并假设所有约束都是正确的，则其他所有约束也将随之移动。 

故障转移的方式取决于 Linux 分发版。 按照 linux 分发版的说明进行操作。

- [RHEL 或 Ubuntu](#manual-failover-rhel-or-ubuntu)
- [SLES](#manual-failover-sles)

## <a name="manual-failover-rhel-or-ubuntu"></a>手动故障转移（RHEL 或 Ubuntu）

若要执行手动故障转移，请在 Red Hat Enterprise Linux (RHEL) 或 Ubuntu 服务器上执行以下步骤。
1.  发出以下命令： 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName> 是 SQL Server FCI 的 Pacemaker 资源名称。

   \<NewHostNode> 是要托管 FCI 的群集节点的名称。 

   不会获得任何确认。

2.  在手动故障转移过程中，Pacemaker 会对已选择手动移动的资源创建位置约束。 若要查看此约束，请运行 `sudo pcs constraint`。

3.  故障转移完成后，通过发出 `sudo pcs resource clear <FCIResourceName>` 来删除该约束。 

\<FCIResourceName> 是 FCI 的 Pacemaker 资源名称。 

## <a name="manual-failover-sles"></a>手动故障转移 (SLES)


在 Suse Linux Enterprise Server (SLES) 中，使用 `migrate` 命令手动故障转移 SQL Server FCI。 例如：

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName> 是故障转移群集实例的资源名称。 

\<NewHostNode> 是新目标主机的名称。 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>后续步骤

- [配置故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
