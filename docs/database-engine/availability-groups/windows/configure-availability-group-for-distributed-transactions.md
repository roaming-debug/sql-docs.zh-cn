---
title: 为可用性组配置分布式事务
description: '介绍如何为 Always On 可用性组内的数据库配置分布式事务。 '
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5587622f0f61b7b7063b246d0599d46cc8c16f0c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170779"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>为 Always On 可用性组配置分布式事务
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] 支持所有分布式事务，包括可用性组中的数据库。 本文介绍如何为分布式事务配置可用性组  

为确保分布式事务的一致性，必须配置可用性组，使其将数据库注册为分布式事务资源管理器。  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] 服务包 2 及更高版本完全支持可用性组中的分布式事务。 在服务包 2 之前的 [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)]!INCLUDE[SQL2016] 版本中，不支持涉及可用性组中的数据库的跨数据库分布式事务（即，使用同一 SQL Server 实例上的数据库的事务）。 [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] 没有此限制。 
>
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] 和 [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] 中的配置步骤相同。

在分布式事务中，客户端应用程序和 Microsoft 分布式事务处理协调器（MS DTC 或 DTC）共同配合来确保多个数据源之间的事务一致性。 DTC 是在基于 Windows Server 的受支持操作系统上提供的服务。 DTC 充当分布式事务的“事务处理协调器”  。 SQL Server 实例通常充当“资源管理器”  。 当数据库位于可用性组中时，每个数据库需为其自身的资源管理器。 

[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 不能阻止可用性组中数据库的分布式事务 - 即使不为分布式事务配置可用性组也是如此。 但是，如果不为分布式事务配置可用性组，在某些情况下故障转移可能不会成功。 具体而言，新主要副本 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 实例可能无法从 DTC 获取事务结果。 若要启用 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 实例，以在故障转移后从 DTC 获取未决事务的结果，请为分布式事务配置可用性组。 

除非数据库也是故障转移群集的成员，否则可用性组处理不会涉及 DTC。 在可用性组中，副本间的一致性由可用性组逻辑维持：主副本不会完成提交并向调用方确认提交，直到辅助副本确认已将日志记录保留在持久存储中。 此时主副本才声明该事务完成。 在异步模式下，我们不会等待辅助副本确认，此模式下明显有丢失少量数据的可能性。

## <a name="prerequisites"></a>先决条件

将可用性组配置为支持分布式事务前，必须满足以下先决条件：

* 参与分布式事务的 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的所有实例必须为 [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] 或更高版本。

* 可用性组必须在 Windows Server 2016 或 Windows Server 2012 R2 上运行。 对于 Windows Server 2012 R2，必须安装 KB3090973 中的更新，网址：[https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973)。  

## <a name="create-an-availability-group-for-distributed-transactions"></a>为分布式事务创建可用性组

配置可用性组以支持分布式事务。 设置可用性组以允许所有数据库都注册为资源管理器。 本文介绍如何配置可用性组，以便所有数据库都可成为 DTC 中的资源管理器。



可在 [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] 或更高版本上为分布式事务创建可用性组。 若要为分布式事务创建可用性组，请在可用性组定义中包含 `DTC_SUPPORT = PER_DB`。 以下脚本将为分布式事务创建可用性组。 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>上面的脚本创建的是简单的可用性组示例，并不针对任何特定生产环境。 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>更改分布式事务的可用性组

可在 [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] 或更高版本上更改分布式事务的可用性组。 若要更改分布式事务的可用性组，请在 `ALTER AVAILABILITY GROUP` 脚本中包含 `DTC_SUPPORT = PER_DB`。 示例脚本将更改可用性组以支持分布式事务。 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>从 [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2 开始，可更改分布式事务的可用性组。 对于 Service Pack 2 之前的 [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] 版本，则需要进行删除，然后使用 `DTC_SUPPORT = PER_DB` 设置重新创建可用性组。 

若要禁用分布式事务，请使用以下 Transact-SQL 命令：

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>分布式事务 - 技术概念

分布式事务可跨两个或多个数据库。 作为事务管理器，DTC 可协调 SQL Server 实例之间和其他数据源之间的事务。 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 数据库引擎的每个实例都可以充当资源管理器。 如果使用 `DTC_SUPPORT = PER_DB` 配置可用性组，数据库也可以充当资源管理器。 有关详细信息，请参阅 MS DTC 文档。

在数据库引擎的单个实例中具有两个或多个数据库的事务实际上是分布式事务。 该实例对分布式事务进行内部管理；对于用户而言，其操作就像本地事务一样。 当数据库位于使用 `DTC_SUPPORT = PER_DB` 配置的可用性组中（甚至是在 SQL Server 的单个实例内）时，[!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] 会将所有跨数据库的事务提升到 DTC。 

对于应用程序而言，管理分布式事务很像管理本地事务。 当事务结束时，应用程序会请求提交或回滚事务。 不同的是，分布式提交必须由事务管理器管理，以尽量避免出现因网络故障而导致事务由某些资源管理器成功提交，但由另一些资源管理器回滚的情况。 通过分两个阶段（准备阶段和提交阶段）管理提交进程可避免这种情况，这称为两阶段提交。

- **准备阶段**
   
   当事务管理器收到提交请求时，它会向该事务涉及的所有资源管理器发送准备命令。 然后，每个资源管理器将尽力使该事务持久，并且所有保存该事务日志映像的缓冲区将被刷新到磁盘中。 当每个资源管理器完成准备阶段时，它会向事务管理器返回准备成功或准备失败的消息。

- **提交阶段**
   
   如果事务管理器从所有资源管理器收到准备成功的消息，它将向每个资源管理器发送一个提交命令。 然后，资源管理器就可以完成提交。 如果所有资源管理器都报告提交成功，那么事务管理器就会向应用程序发送一个成功通知。 如果任一资源管理器报告准备失败，那么事务管理器将向每个资源管理器发送一个回滚命令，并向应用程序表明提交失败。

### <a name="detailed-steps"></a>详细步骤

以下列表说明应用程序如何与 DTC 配合完成分布式事务。

1. SQL Server 实例在 DTC 事务中登记。 当事务中有多个资源管理器或客户端请求将事务提升到 DTC 事务时，可能会出现此情况。
2. 客户端在 SQL Server 实例中的 DTC 事务下执行某些工作。
3. 客户端发起 DTC 事务提交或中止。
    - 如果客户端发起中止，事务将立即中止。
    - 如果客户端发起提交，DTC 将通知事务中的所有资源管理器准备事务，从而启动两阶段提交协议。
4. 当所有资源均成功收到准备阶段的通知后，DTC 将通知所有资源管理器提交事务。 如果有任何原因导致未成功收到通知，DTC 将中止事务。 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>为分布式事务配置可用性组的影响

参与分布式事务的每个实体都可称为资源管理器。 资源管理器的示例包括：

* 一个 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 实例。 
* 已为分布式事务配置的可用性组中的数据库。
* DTC 服务 - 也可以充当事务管理器。
* 其他数据源。 

若要参与分布式事务，[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的实例需要向 DTC 登记。 通常情况下，[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的实例在本地服务器上向 DTC 登记。 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的每个实例都会创建一个具有唯一资源管理器标识符 (RMID) 的资源管理器并将其注册到 DTC。 在默认配置中，[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 实例上的所有数据库都使用相同的 RMID。 

当数据库位于可用性组中时，数据库的读写副本或主要副本可能移动到 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的另一个实例上。 若要在此移动过程中支持分布式事务，每个数据库都应充当单独的资源管理器，并且必须具有唯一的 RMID。 当可用性组具有 `DTC_SUPPORT = PER_DB` 时，[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 将为每个数据库创建资源管理器，并且使用唯一的 RMID 将它们向 DTC 注册。 在此配置中，数据库充当于 DTC 事务的资源管理器。

有关 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 中的分布式事务的详细信息，请参阅[分布式事务](#distTran)

## <a name="manage-unresolved-transactions"></a>管理未解决的事务

故障转移后，可能无法恢复 RMID 更改期间存在的活动事务的结果。 这是因为用于登记的 RMID SQL Server 和用于恢复的 RMID SQL Server 是不同的。 以下情况可能导致 RMID 更改：

* 更改可用性组的 `DTC_SUPPORT`。 
* 在可用性组中添加或删除数据库。 
* 删除可用性组。

在上述情况下，如果主要副本故障转移到 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 的新实例，该实例将尝试联系 DTC 以确定事务结果。 DTC 无法返回结果，因为数据库用于获取恢复期间未决事务的结果的 RMID 之前未进行登记。 因此，数据库将进入 SUSPECT 状态。

新 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 错误日志中的一个条目将如下例所示：

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

前面的示例说明，DTC 无法通过故障转移后创建的事务中的新主要副本重新登记数据库。 [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] 实例不能确定分布式事务的结果，因此它将数据库标记为可疑。 事务将被标记为工作单位 (UOW)，并由 GUID 引用。 为了恢复数据库，请手动提交事务或回滚事务。 

>[!WARNING]
>手动提交或回滚事务时，应用程序可能受到影响。 请验证提交或回滚操作是否符合应用程序要求。 

仅运行以下脚本之一：

   * 若要提交事务，请更新并运行以下脚本 - 使用前一条错误消息中的未决事务 UOW 替换 `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy`，然后运行：

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * 若要回滚事务，请更新并运行以下脚本 - 使用前一条错误消息中的未决事务 UOW 替换 `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy`，然后运行：

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

提交或回滚事务后，可使用 `ALTER DATABASE` 将数据库设置为联机。 更新并运行以下脚本 - 为可疑数据库设置名称：

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

有关解决未决事务的详细信息，请参阅[手动解决事务](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10))。

## <a name="next-steps"></a>后续步骤  

[分布式事务](/dotnet/framework/data/adonet/distributed-transactions)

[Always On 可用性组：互操作性 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[事务 - AlwaysOn 可用性组和数据库镜像](transactions-always-on-availability-and-database-mirroring.md)  

[支持 XA 事务](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10))

[工作原理：会话/SPID (-2) 的 DTC 事务](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions)