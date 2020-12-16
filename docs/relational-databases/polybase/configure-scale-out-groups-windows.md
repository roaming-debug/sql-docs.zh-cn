---
title: 在 Windows 上配置 PolyBase 横向扩展组 | Microsoft Docs
description: 设置 PolyBase 横向扩展组以创建 SQL Server 实例群集。 这提高了来自外部源的大型数据集的查询性能。
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: tutorial
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 1592632bdeca9f8c3cdceeb61b06c1bd92550f53
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477918"
---
# <a name="configure-polybase-scale-out-groups-on-windows"></a>在 Windows 上配置 PolyBase 横向扩展组

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文介绍如何在 Windows 上设置 [PolyBase 横向扩展组](polybase-scale-out-groups.md)。 此功能将创建 SQL Server 实例的群集来处理来自外部数据源的大型数据集（如 Hadoop 或 Azure Blob 存储），从而通过一种扩展的方式提高查询性能。

## <a name="prerequisites"></a>先决条件
  
- 同一个域中有多台计算机  
  
- 使用域用户帐户来运行 PolyBase 服务  
  
## <a name="process-overview"></a>过程概述

以下步骤汇总了用于创建 PolyBase 横向扩展组的过程。 下一节提供了各步骤更详细的演练过程。
  
1. 在 N 台计算机上安装相同版本的具有 PolyBase 的 SQL Server。
  
2. 选择一个 SQL Server 实例作为头节点。 
  
3. 使用 [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md)将剩余的 SQL Server 实例添加为计算节点。

4. 使用 [sys.dm_exec_compute_nodes (Transact SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md) 监视组中的节点。

5. 可选。 使用 [sp_polybase_leave_group (Transact-SQL)](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-leave-group.md) 从 删除计算节点。

## <a name="example-walk-through"></a>示例演练

它将演示使用以下内容配置 PolyBase 组的步骤：  
  
1. 域 *PQTH4A* 中的两台计算机，计算机名称为：  
  
   - PQTH4A-CMP01  
  
   - PQTH4A-CMP02  
  
2. 域帐户：PQTH4A\PolyBaseUser   

## <a name="install-sql-server-with-polybase-on-all-machines"></a>在所有机器上安装具有 PolyBase 的 SQL Server

1. 运行 setup.exe。
  
2. 在“功能选择”页上，选择“针对外部数据的 PolyBase 查询服务”  。
  
3. 在“服务器配置”页上，对 SQL Server PolyBase 引擎和 SQL Server PolyBase 数据移动服务使用域帐户 PQTH4A\PolybaseUser  。
  
4. 在“PolyBase 配置”页中，选择“将 SQL Server 实例用作 PolyBase 横向扩展组的一部分”  选项。 这将打开防火墙以允许 PolyBase 服务的传入连接。 如果头节点是命名实例，则必须手动将 SQL Server 端口添加到头节点上的 Windows 防火墙，同时在头节点上启动 SQL Browser。
  
5. 安装完成后，运行“services.msc”  。 验证 SQL Server、PolyBase 引擎和 PolyBase 数据移动服务是否在运行。
  
   ![PolyBase 服务](../../relational-databases/polybase/media/polybase-services.png "PolyBase 服务")  
  
## <a name="select-one-sql-server-as-head-node"></a>选择一个 SQL Server 作为头节点  
  
安装完成后，两台计算机都可充当 PolyBase 组头节点。 在此示例中，我们将选择 PQTH4A-CMP01 上的“MSSQLSERVER”作为头节点。
  
## <a name="add-other-sql-server-instances-as-compute-nodes"></a>将其他 SQL Server 实例添加为计算节点  
  
1. 连接到 PQTH4A-CMP02 上的 SQL Server。
  
2. 运行存储过程 [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md)。

   ```sql
   -- Enter head node details:
   -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';
   ```  

3. 在计算节点 (PQTH4A-CMP02) 上运行 services.msc。
  
4. 关闭 PolyBase 引擎并重启 PolyBase 数据移动服务。

> [!NOTE] 
> 当 Polybase Engine 服务重启或在头节点中停止时，只要关闭数据移动服务 (DMS) 与 Polybase Engine 服务 (DW) 之间的信道，DMS 服务就会停止。 如果 DW 引擎重启两次以上，则 DMS 会进入 90 分钟的静默期，并且必须等待 90 分钟才能进行下一次自动启动尝试。 在这种情况下，应在所有节点上手动启动此服务。

## <a name="optional-remove-a-compute-node"></a>可选：删除计算节点  
  
1. 连接到计算节点 SQL Server (PQTH4A-CMP02)。
  
2. 运行存储过程 sp_polybase_leave_group。
  
    ```sql  
    EXEC sp_polybase_leave_group;  
    ```  
  
3. 在正在删除的计算节点 (PQTH4A-CMP02) 上运行 services.msc。
  
4. 启动 PolyBase 引擎。 重启 PolyBase 数据移动服务。
  
5. 验证是否已通过运行 DMV sys.dm_exec_compute_nodes on PQTH4A-CMP01 删除该节点。 现在，PQTH4A-CMP02 将充当独立头节点  
  
## <a name="next-steps"></a>后续步骤  

有关故障排除，请参阅 [PolyBase troubleshooting with dynamic management views](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))。
  
有关 PolyBase 的详细信息，请参阅 [PolyBase 概述](../../relational-databases/polybase/polybase-guide.md)。