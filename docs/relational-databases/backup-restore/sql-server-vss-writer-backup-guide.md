---
title: SQL Server 备份应用程序 - 卷影复制服务 (VSS) 和 SQL 编写器
description: 介绍 SQL 编写器组件及其在 SQL Server 数据库的 VSS 快照创建和还原过程中的角色。
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36fbf9ac4010797561e0852b96b6df6fd85b640d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347298"
---
# <a name="sql-server-back-up-applications---volume-shadow-copy-service-vss-and-sql-writer"></a>SQL Server 备份应用程序 - 卷影复制服务 (VSS) 和 SQL 编写器
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

SQL Server 通过提供一个编写器（SQL 编写器）来支持卷影复制服务 (VSS)，以便第三方备份应用程序可以使用 VSS 框架来备份数据库文件。 本文介绍了 SQL 编写器组件及其在 SQL Server 数据库的 VSS 快照创建和还原过程中的角色。 还介绍了如何配置和使用 SQL 编写器来处理 VSS 框架中的备份应用程序的详细信息。


## <a name="introduction"></a>简介

SQL Server 支持使用卷影复制服务 (VSS) 从 SQL Server 数据创建快照。 这是通过提供与 VSS 兼容的编写器（SQL 编写器）来实现的，以便第三方备份应用程序可以使用 VSS 框架来备份数据库文件。 本文介绍了 SQL 编写器组件及其在 SQL Server 数据库的 VSS 快照创建和还原过程中的角色。 还介绍了有关如何配置和使用 SQL 编写器在 VSS 框架上下文中处理备份应用程序的详细信息。

## <a name="definition-of-terms"></a>术语的定义

- **虚拟设备接口**：SQL Server 提供称为虚拟备份设备接口 (VDI) 的应用程序编程接口，使独立软件供应商能够将 SQL Server 集成到他们的产品中来支持备份和还原操作。 这些 API 设计为提供最优可靠性和性能，并支持 SQL Server 的所有备份与还原功能，包括所有热备份和快照备份功能。 有关详细信息，请参阅 [SQL Server 2005 虚拟备份设备接口规范](https://www.microsoft.com/download/details.aspx?id=17282)。 

- **请求程序**：请求从一个或多个原始卷中获取一个或多个快照集的过程（自动或 GUI）。 在本文中，请求程序还用于表示创建 SQL Server 数据库快照的备份应用程序。

## <a name="about-vss"></a>关于 VSS

VSS 提供了在 Windows 系统上运行 VSS 应用程序的系统基础结构。 尽管在很大程度上对用户和开发人员都是透明的，但 VSS 会执行以下操作：

- 在卷影复制的创建和使用中协调提供程序、编写器和请求程序的活动。
- 提供默认的系统提供程序。
- 实现任何提供程序工作所必需的低级驱动程序功能。

VSS 服务按需启动；因此，若要成功执行 VSS 操作，必须启用此服务。

## <a name="vss-components"></a>VSS 组件

VSS 协调以下协作组件的活动： 

- 提供程序拥有卷影副本数据并实例化卷影副本。
- 编写器是更改数据并参与卷影复制同步过程的应用程序。
- 请求程序启动卷影副本的创建和销毁。 我们的设计重点在于请求程序是备份应用程序的情况。

VSS 在这些参与方之间提供协调： 

![显示 VSS 在这些参与方之间如何提供协调的示意图。](media/sql-server-vss-writer-backup-guide/coordinations.png)

此图显示了参与典型的 VSS 快照活动的所有组件。 在这种情况下，SQL Server（包括 SQL 编写器）在某个编写器框中充当编写器。  其他此类编写器可能是 Exchange Server 等。

在本文档的其余部分中，假设读者熟悉 VSS 框架的术语。  有关详细信息，请参阅有关[卷影复制服务](/windows/win32/vss/volume-shadow-copy-service-overview)的文档。

## <a name="about-sql-writer"></a>关于 SQL 编写器

SQL 编写器是 SQL Server 提供的 VSS 编写器。 它处理与 SQL Server 的 VSS 交互。 SQL 编写器作为独立服务随 SQL Server 一起提供，并作为 SQL Server 安装的一部分进行安装。 默认情况下，不启用 SQL 编写器。 它需要显式启用才能在服务器计算机上运行。

SQL 编写器在 VSS 快照备份操作中的角色： 

![VSS 快照](media/sql-server-vss-writer-backup-guide/sql-vss-snapshot.png)



## <a name="configuring-the-sql-writer"></a>配置 SQL 编写器

SQL 编写器服务作为 SQL Server 安装的一部分安装在系统中，配置为在 Windows 启动时自动启动。 

### <a name="sql-writer-service-account"></a>SQL 编写器服务帐户

在安装过程中，将安装 SQL 编写器帐户以使用本地系统帐户。 由于 SQL 编写器需要使用专用的 VDI API 与 SQL Server 进行通信，因此 SQL 编写器帐户必须对 SQL Server 和 VSS 具有足够的访问权限。  将服务配置为本地系统帐户可为服务的正确运行提供足够的权限。

  > [!NOTE]
  > 若要使 SQL 编写器服务正常运行，请务必确保不会从 SQL Server 实例的“sa”角色中删除本地系统帐户。

### <a name="enabling-and-starting-sql-writer"></a>启用和启动 SQL 编写器

若要启动和使用 SQL 编写器，必须执行以下操作：

可以通过将此服务标记为“自动”来启用 SQL 编写器服务。 若要通过控制面板打开服务，请依次单击“开始”、“控制面板”，双击“管理工具”，然后双击“服务”。 在“服务”窗格中，双击 SQL 编写器服务并将“启动类型”属性修改为“自动”。

还应通过选择上述服务属性屏幕中“服务状态”属性下的“开始”按钮来启动服务。

为了方便起见，SQL 编写器服务将在首次启动时自动进行这些更改。

  > [!NOTE]
  > 在某些情况下，如果安装了 SQL Server Express 的实例，并且应用程序正在使用用户实例功能，那么 SQL Server 可能会自动启动 SQL 编写器。 这样做是为了方便在 VSS 备份操作期间枚举这些用户实例。 

## <a name="backuprestore-operations-and-supported-versions"></a>备份/还原操作以及支持的版本

### <a name="version-support"></a>版本支持

SQL 编写器是作为 SQL Server 的一部分提供的，并且只支持 SQL Server 实例。 

SQL 编写器还将枚举 SQL Server Express 的实例。 SQL Server Express 启动的用户实例也将由 SQL 编写器枚举。


## <a name="supported-backuprestore-operations"></a>支持的备份/还原操作

SQL Server（使用 SQL 编写器）将支持以下基于 VSS 的备份操作模式：

- 基于非组件
- 基于组件

### <a name="noncomponent-based-backup-operations"></a>基于非组件的备份操作

基于非组件的备份通过使用快照集中的卷列表隐式地选择数据库。 SQL 编写器检查是否有损坏的数据库，如果发现则引发错误。 损坏的数据库是指其中的文件子集由卷列表选择的数据库。

在基于非组件的模型中，仅支持具有简单恢复模型的数据库。 不支持还原后前滚。

### <a name="component-based-backup-operations"></a>基于组件的备份操作

由于应用程序（VSS 备份应用程序）将从 SQL 编写器返回的元数据中显式地选择数据库，因此 SQL 编写器首选并推荐使用基于组件的备份。 快照集应包括备份这些数据库所需的所有卷。 VSS 基础结构不会自动添加所选数据库集所需的卷。 所有备份卷都应包含在卷快照集中。 备份应用程序有责任确保快照集中包括所有备份卷。  SQL 编写器将检测损坏的数据库（备份卷在快照集外部）并使备份失败。

本主题的其余部分假设基于组件的备份用作 SQL Server 的 VSS 快照创建过程的一部分。

## <a name="features-supported-by-sql-writer"></a>SQL 编写器支持的功能


- **全文**：SQL 编写器在编写器元数据文档的数据库组件下报告包含递归文件规范的全文目录容器。  当选择数据库组件时，它们将自动包含在备份中
- **差异备份/还原**：SQL 编写器通过两种 VSS 差异机制支持差异备份/还原：按上次修改时间的部分文件和差异文件。
- **部分文件**： SQL 编写器使用 VSS 部分文件机制来报告其数据库文件中已更改的字节范围。  
- **按上次修改时间的差异文件**：SQL 编写器使用 VSS 按上次修改时间的差异文件机制来报告全文目录中的已更改文件。
- **移动式还原：** SQL 编写器在还原期间支持 VSS 新目标规范。  VSS 新目标规范允许作为还原操作的一部分重新定位数据库/日志文件或全文目录容器。
- **数据库重命名**：请求程序可能需要用新名称还原 SQL 数据库，特别是当数据库要与原始数据库并排还原时。 只要数据库保留在原始 SQL 实例中，SQL 编写器就会支持在还原操作期间重命名数据库。
- **“仅复制”备份**：有时需要创建一个适用于特殊目的的备份，例如出于测试目的而需要创建数据库副本。  这种备份不会影响数据库的总体备份和恢复进程。 使用 COPY_ONLY 选项指定备份是在“带外”完成的，不应影响备份的正常顺序。 SQL 编写器支持 SQL Server 实例的“仅复制”备份类型。
- **自动恢复数据库快照**： 通常，通过使用 VSS 框架获得的 SQL Server 数据库快照处于未恢复状态。 在数据进入恢复阶段以回滚运行中的事务并将数据库置于一致状态之前，无法安全地访问快照中的数据。 作为快照创建过程的一部分，VSS 备份应用程序可以请求快照的自动恢复。

这些新功能及其用法将在本主题的“备份和还原”选项详细信息中进行详细说明。

### <a name="what-is-not-supported"></a>不支持的功能

- SQL 编写器不支持日志备份。
- 不支持文件/文件组备份。
- 不支持页面还原。
- 不支持数据库快照，并且在创建组件和非组件 VSS 快照时将忽略数据库快照。 
- 自动关闭数据库，或启用关机的数据库。
- Linux 不提供 VSS 框架，因此 SQL 编写器在 Linux 上不可用

下表列出了 SQL 编写器/SQL Server 支持的快照备份类型，其中对于所有版本的 SQL Server，均使用 VSS 框架。

| **备份/还原操作**                 | **基于组件**           | **基于非组件** |
|:-------------------------------------------- | :---------------------------- | :--------------------- |
|完整数据备份 </br> （包括全文目录）| 是                     | 是                    |
|完整还原                                  | 是                           | 是                    |
|完整还原（无恢复）                    | 是                           | 否                     |
|差异备份                           | 是                           | 否                     |
|差异还原                          | 是                           | 否                     |
|移动式还原                             | 是                           | 否                     |
|数据库重命名                               | 是                           | 否                     |
|仅复制备份                              | 是                           | 否                     |
|自动恢复的快照                       | 是                           | 否                     |
|日志备份                                    | 否                            | 否                     |
|数据库快照                            | 否                            | 否                     |
|自动关闭数据库</br> 关闭数据库 | 是                        | 否                     |
|可用性组数据库                  | 是                           | 次要数据库时为否        | 




## <a name="snapshot-creation-process"></a>快照创建过程

VSS 框架在创建 SQL Server 快照期间协调请求程序（备份应用程序）和 SQL 编写器的活动。 为了实现这种协调，VSS 框架定义了请求程序和编写器接口。  这些接口应由参与的请求程序应用程序和编写器实现。 SQL 编写器实现了必要的编写器接口。 在快照创建过程中，VSS 框架将调用 SQL 编写器的接口。 SQL 编写器与系统上的 SQL Server 实例交互，以便于创建快照。

VSS 框架定义了一组 API 供请求程序/备份应用程序使用。 备份应用程序开发人员需要遵循这些 API 调用模式才能使用 VSS 框架快照创建过程。 下面几节将从 SQL 编写器的角度描述快照创建过程。 并且还将详细描述请求程序、VSS 框架、SQL 编写器和 SQL Server 实例之间的一些内部交互。 有关这些步骤的更多信息和 VSS 框架接口的详细信息，请参阅有关卷影复制服务的文档。    

  > [!NOTE]
  > 假设读者已比较熟悉 VSS 框架和备份创建过程。 这些部分是关于 SQL 编写器如何参与 VSS 备份创建过程的补充信息。

## <a name="snapshot-creation-workflow"></a>快照创建工作流

![数据流](media/sql-server-vss-writer-backup-guide/dataflow-chart.png)

此图显示基于组件的快照创建/备份操作期间的数据流关系图。 若要更全面地了解执行备份所涉及的基本任务，请将此概述分解为以下几个阶段：

- 备份初始化
- 备份发现阶段
- 预备份任务
- 文件的实际备份
- 备份终止



### <a name="backup-initialization"></a>备份初始化

在备份的这个阶段，请求程序（备份应用程序）绑定到快照接口“IvssBackupComponents”，并对其进行初始化以准备备份。 还会调用 VSS API“IVssGatherWriterMetadata”以指示 VSS 框架从所有编写器收集元数据。

VSS 框架将使用“OnIdentify”事件调用每个注册的编写器，包括编写器元数据的 SQL 编写器。 SQL 编写器将查询 SQL Server 实例，以获取每个数据库的备份元数据信息，并创建编写器元数据文档。 此阶段也称为元数据枚举。

编写器元数据文档包含从编写器传递到请求程序（备份应用程序）的信息。 编写器元数据文档包含以下信息：

- 应用程序 ID 和友好名称。
- 文件和组件存在的位置。
- 需要在备份中包括和排除的文件。
- 还原时应使用的选项。
- 这会通过 VSS 框架传回请求程序。

### <a name="sql-writer-metadata-document"></a>SQL 编写器元数据文档

这是由编写器（在本例中是 SQL 编写器）使用“IVssCreateWriterMetadata”接口创建的 XML 文档，包含有关编写器状态和组件的信息。 VSS API 文档中描述了编写器元数据文档的结构细节。 以下是 SQL 编写器元数据文档的一些详细信息。

- 编写器标识信息
    - **编写器名称** - L"SqlServerWriter"
    - **编写器类 ID** - 0xa65faa63、0x5ea8、0x4ebc、0x9d、0xbd、0xa0、0xc4、0xdb、0x26、0x91、0x2a 
    - **编写器实例 ID** - L"SQL Server:SQLWriter" 
    - **VSSUsageType** - VSS_UT_USERDATA 
    - **VSSSourceType** - VSS_ST_TRANSACTEDDB 
- 编写器级别信息 - VSS_APP_BACK_END 
- 还原方法规范 - VSS_RME_RESTORE_IF_CAN_REPLACE。
- 支持的备份架构 (IVssCreateWriterMetadata::SetBackupSchema API)
    - VSS_BS_DIFFERENTIAL - 差异备份
    - VSS_BS_TIMESTAMPED - 基于时间戳（用于全文目录文件）。
    - VSS_BS_LAST_MODIFY - 基于上次修改时间的差异备份，
    - VSS_BS_WRITER_SUPPORTS_NEW_TARGET - 支持“新目标位置”选项。
    - VSS_BS_WRITER_SUPPORTS_RESTORE_WITH_MOVE - 支持“移动式”还原
    - VSS_BS_COPY - 支持“仅复制”备份选项。
- 组件级信息包含 SQL 编写器提供的组件级特定信息。
   - **类型** - VSS_CT_FILEGROUP
   - **名称** - 组件的名称（数据库名称）
   - **逻辑路径** - 服务器实例的逻辑路径（命名实例的格式为“server\instance name”，默认实例的格式为“server”）
   - **组件标志**
   - **VSS_CF_APP_ROLLBACK_RECOVERY** - 表示 SQL Server 快照始终需要“恢复”阶段，以使文件在非备份（即应用程序回滚）方案中保持一致和可用。
   - 可选 - True
   - 可选择进行还原 - True 
   - 支持的还原方法 - VSS_RME_RESTORE_IF_CAN_REPLACE

SQL Server 中组件集结构的唯一扩展是引入全文目录。  全文目录是容器目录，不能表示为 VSS 数据库或日志文件，因为 VSS 数据库和日志文件没有递归规范。  因此，SQL 编写器将使用 VSS 文件组组件（VSS_CT_FILEGROUP）来表示数据库级组件，并使用文件组文件来表示数据库、日志和全文目录文件。

本文档末尾提供了一个示例编写器元数据文档。

### <a name="backup-discovery"></a>备份发现
在此阶段中，请求程序检查编写器元数据文档，并用需要备份的每个组件创建和填充“备份组件文档”。 它还指定了所需的备份选项和参数，作为本文档的一部分。 对于 SQL 编写器，需要备份的每个数据库实例都是一个单独的组件。

#### <a name="backup-components-document"></a>备份组件文档
这是请求程序（使用“IVssBackupComponents”接口）在设置还原或备份操作过程中创建的 XML 文档。 备份组件文档包含显式包含组件的列表，这些组件来自参与备份或还原操作的一个或多个编写器。 它不包含隐式包含的组件信息。 相反，编写器元数据文档只包含可能参与备份的编写器组件。 VSS API 文档中描述了备份组件文档的结构细节。

#### <a name="prebackup-tasks"></a>预备份任务
VSS下的预备份任务主要是创建包含备份数据的卷的卷影副本。 备份应用程序将从卷影副本而不是实际卷中保存数据。

请求程序通常在准备备份和创建卷影副本期间等待编写器。 如果 SQL 编写器正在参与备份操作，那么它需要配置其文件及其本身，以准备备份和卷影复制。

#### <a name="prepare-for-backup"></a>准备备份
请求程序需要设置需要执行的备份操作的类型（“IVssBackupComponents::SetBackupState”），然后通过 VSS 通知编写器，以便使用“IVssBackupComponents::PrepareForBackup”准备备份操作 。

为 SQL 编写器提供对备份组件文档的访问权限，该文档详细说明了需要备份的数据库。 所有备份卷都应包含在卷快照集中。 SQL 编写器将检测损坏的数据库（备份卷在快照集外部），并在 PostSnapshot 事件期间使备份失败。

**启动快照** 请求程序将通过调用 VSS 框架接口 DoSnapshotSet 来启动快照进程。

**创建快照** 此阶段涉及 VSS 框架和 SQL 编写器之间的一系列交互。

1. 准备快照。 SQL 编写器将调用 SQL Server 以准备创建快照。
1. _冻结_。SQL 编写器将调用 SQL Server 冻结快照中备份的每个数据库的所有数据库 I/O。 冻结事件返回到 VSS 框架后，VSS 将创建快照。
1. 解冻。 在此事件中，SQL 编写器将调用 SQL Server 实例以解冻或恢复正常的 I/O 操作。


快照创建阶段很快（小于 60 秒），以防止阻止对数据库的所有写入。

**后快照** 如果快照需要自动恢复，SQL 编写器将为已选定要出现在快照中的每个数据库执行自动恢复。 有关详细说明，请参阅“自动恢复的快照”。

#### <a name="actual-backup-of-files"></a>文件的实际备份

在此阶段，如果需要，请求程序可以将数据移动到备份介质。 这个阶段的交互是请求程序和 VSS 框架之间的交互。 不涉及 SQL 编写器。

1. 获取编写器状态。 返回编写器状态。 请求程序可能需要在此处处理任何故障。
1. 进行备份。

如果此时需要，请求程序可以将数据移动到备份介质。

#### <a name="backup-complete"></a>备份完成

此事件将指示备份已成功完成。

如果当前备份是数据库的完整备份（而不是仅副本备份），则这也是 SQL 编写器可以将备份提交为差异基准的时间。


  > [!NOTE]
  > 请求程序应显式发送此事件（备份完成事件），以允许 SQL 编写器提交差异基准备份。 如果未接收到此事件，则创建的备份将不是合格的“差异基准”备份。

#### <a name="save-writer-metadata"></a>保存编写器元数据

请求程序应保存备份组件文档和每个组件备份元数据，以及从快照备份的数据。 SQL 编写器/SQL Server 需要这些编写器元数据才能执行还原操作。

#### <a name="backup-termination"></a>备份终止

请求程序通过释放“IVssBackupComponents”接口或通过调用“IVssBackupComponents::DeleteSnapshots”来终止卷影副本 。

## <a name="restore-process"></a>还原过程

本节介绍还原操作工作流和涉及的各种步骤。

### <a name="restore-operation-workflow"></a>还原操作工作流

下图显示了在执行 VSS 还原操作期间的数据流关系图。 若要更全面地了解执行还原所涉及的基本任务，请将此概述分解为以下主题：

- 还原初始化
- 准备还原
- 实际文件还原
- 还原清理和终止

![还原过程流](media/sql-server-vss-writer-backup-guide/restore-process-flow.png)

在所有基于 VSS 组件的还原方案中，数据库还原由 SQL 编写器分两个不同的阶段处理。

- **预还原**：SQL 编写器处理验证、关闭文件句柄等。
- **还原后**：SQL 编写器附加数据库，并在需要时执行崩溃恢复。

在这两个阶段之间，备份应用程序负责移动 SQL 下方的相关数据。

### <a name="restore-initialization"></a>还原初始化

在还原的初始化阶段，请求程序需要访问存储的备份组件文档。

在备份操作期间生成的备份组件文档作为备份数据的一部分进行存储。 备份应用程序需要将这些数据传递回 VSS 框架。 SQL 编写器在恢复过程开始时获得对该数据的访问权限。

#### <a name="prepare-for-restore"></a>准备还原

在准备还原时，请求程序使用存储的备份组件文档来确定还原的内容和方法。  请求程序将选择要恢复的组件，并根据需要设置适当的恢复选项。

如果备份应用程序打算在当前还原操作的基础上应用差异备份或日志备份（即需要“使用 norecovery 还原”），则应将以下选项设置为将要还原的每个数据库的组件创建的一部分。

```
IVssBackupComponents::SetAdditionalRestores(true)
```

在备份组件文档中设置了所有所需的详细信息后，请求程序就会调用“IVssBackupComponents::PreRestore”，通过 VSS 生成将由编写器处理的预还原事件。

SQL 编写器将检查提供的备份组件文档以标识适当的数据库，并删除自备份时间以来创建的任何其他文件。 它还会检查磁盘空间并关闭所有打开的数据库文件句柄，以便请求程序可以在还原阶段复制所需的数据。 此阶段允许在请求程序执行实际文件复制之前检测任何早期错误条件。 SQL Server 还会使数据库处于还原状态。  从现在起，只有成功还原才能启动数据库。

#### <a name="restore-files"></a>还原文件

此操作完全特定于请求程序。 请求程序（备份应用程序）有责任将所需的数据库文件（或复制用于差异还原的相关数据范围）复制到适当的位置。 此操作不涉及 SQL 编写器。

#### <a name="cleanup-and-termination"></a>清理和终止

将所有数据还原到正确的位置后，请求程序将触发指示还原操作已完成的调用（IvssBackupComponents::PostRestore），此调用将指示 SQL 编写器可以启动还原后操作。  此时，SQL 编写器将执行崩溃恢复的重做阶段。 如果未请求恢复（即请求程序未指定 SetAdditionalRestores(true)），恢复步骤的撤消阶段也将在此阶段执行。

## <a name="backup-and-restore-option-details"></a>备份和还原选项详细信息

本节详细介绍 SQL 编写器支持的所有备份和还原选项。

### <a name="requestor-creates-a-volume-shadow-copy"></a>请求程序创建卷影副本

SQL 编写器可能参与卷影副本创建过程（在备份/还原上下文之外），因为数据库文件的备份卷已添加到卷快照集中。  在这种情况下，SQL 编写器只参与元数据枚举、冻结、解冻、PrepareForSnapshot 和 PostSnapshot 协调（有关详细信息，请参阅数据流关系图）。

### <a name="full-backuprestore"></a>完整备份/还原

SQL 编写器支持在非基于组件的模式和基于组件的模式下执行完整备份/还原操作。

### <a name="noncomponent-based-backup-and-restore"></a>基于非组件的备份和还原

在基于非组件的备份/还原中，请求程序指定要备份/还原的卷或文件夹树。 指定卷/文件夹中的所有数据都已备份/还原。

#### <a name="backup"></a>备份

在基于非组件的备份中，SQL 编写器使用快照集中的卷列表隐式地选择数据库。  编写器检查损坏的数据库，如果发现则引发错误。 损坏的数据库是指其中的文件子集由卷列表选择的数据库。  SQL 编写器不支持还原后前滚（差异还原或日志还原）。

#### <a name="restore"></a>还原

请求程序还原以非基于组件的模式备份的数据库。  此类还原之后不能执行前滚还原，如日志还原或差异还原。

对于基于非组件的还原操作，必须在 SQL Server 实例脱机的情况下执行还原，或者删除/分离目标数据库以确保文件处于脱机状态。  就地复制文件，然后附加数据库。 所有这些都发生在 SQL 编写器的作用域之外。

### <a name="component-based-backup-and-restore"></a>基于组件的备份和还原

在基于组件的备份中，请求程序显式地选择要备份/还原的数据库组件（从 SQL 编写器返回给客户端的元数据中）。

#### <a name="backup"></a>备份

在基于组件的备份中，所选数据库的所有备份卷都应包含在卷快照集中。  否则，SQL 编写器将检测损坏的数据库（备份卷在快照集之外）并使备份失败。  完整备份将备份数据库数据和在还原时使数据库处于事务一致状态所需的所有日志文件。

#### <a name="full-restore-without-rollforward"></a>完整还原，不前滚

数据库备份的完整还原有时不需要执行任何额外的前滚。 这可能是因为没有元数据来促进前滚，或者在某些情况下，不需要前滚。 本部分简要介绍这两种情况。  

#### <a name="no-metadatano-rollforward"></a>无元数据/无前滚

如果在备份操作期间未保存编写器元数据（基于组件的备份元数据），则必须在 SQL Server 实例脱机的情况下执行还原，或者删除/分离目标数据库以确保文件处于脱机状态。  就地复制文件，然后附加数据库。 所有这些都发生在 SQL 编写器的作用域之外。

#### <a name="metadata-exist-but-no-additional-rollforward-is-needed"></a>元数据存在，但不需要额外的前滚

请求程序还原以基于组件的模式备份的数据库，但不请求任何前滚。 在这种情况下，作为还原的一部分，SQL Server 将对数据库执行崩溃还原。

**带有附加前滚的完整还原** 请求程序可以发出指定 SetAdditionalRestores(true) 选项的还原。  此选项表示请求程序将继续进行更多前滚还原（如日志还原、差异还原等）。 这指示 SQL Server 不在还原操作结束时执行恢复步骤。

只有当编写器元数据是在备份期间保存的，并且在还原时可供 SQL 编写器使用时，才可以执行此操作。 在请求程序指示 VSS 执行还原活动之前，必须先运行 SQL Server 服务。

SQL 编写器需要以下序列：

1. 准备还原每个数据库。 此活动涉及关闭所有文件句柄，以便允许请求程序应用程序复制/装载数据库文件。
1. 文件由请求程序应用程序复制/装载。
1. 完整还原（使用 NORECOVERY）。  数据库联机，但处于“正在还原”状态。

然后，可以使用常规的 SQL 备份（差异或日志）通过 VDI/T-SQL 前滚数据库，或者使用 VSS 框架应用差异还原。

### <a name="full-text-support"></a>全文支持

SQL 编写器在编写器元数据文档的数据库组件下报告包含递归文件规范的全文目录容器。  当选择数据库组件时，它们将自动包含在备份中

### <a name="differential-backuprestore"></a>差异备份/还原

差异备份操作仅备份自最新基本完整备份后发生更改的数据。 差异备份仅包含已更改的数据库文件的那些部分。 为了执行此类备份，备份应用程序或请求程序需要有关数据库文件中更改位置的信息，以便对文件的相应部分进行备份。 在差异备份操作期间，SQL 编写器以“VSS 部分文件信息”指定的格式提供此信息。 此信息可用于仅备份数据库文件的已更改部分。

### <a name="backup"></a>备份

当使用 VSS 启动备份操作时，请求程序可以通过在备份组件文档（“IVssBackupComponents::SetBackupState”）中设置差异选项（“VSS_BT_DIFFERENTIAL”）来发出差异备份 。  SQL 编写器将部分文件信息（由 SQL Server 返回）传递给 VSS。  请求程序可以通过调用 VSS API（“IVssComponent::GetPartialFile”）来获取该文件信息。 此部分文件信息允许请求程序仅选择更改的字节范围来备份数据库文件。

在“预备份任务”阶段，SQL 编写器将确保每个选择的数据库都存在单个差异基准。

在 PostSnapshot 事件期间，SQL 编写器将从 SQL Server 获取部分文件信息，并使用“IVssComponent::AddPartialFile”调用将其添加到备份组件文档中。  

  > [!NOTE]
  > 对于差异备份，SQL 编写器仅支持单个差异基准。 不支持多个基准。

### <a name="partial-file-information-format"></a>部分文件信息格式

对于在差异备份期间备份的每个数据库，SQL 编写器将存储每个数据库文件的部分文件信息。 请求程序或备份应用程序使用此信息在文件的实际备份期间仅将文件的相关部分复制到备份介质。 有关此部分文件信息格式的详细信息，请参阅“卷影复制服务”的文档。

请求程序可以通过调用“IVssComponent::GetPartialFileCount”和“IVssComponent::GetPartialFile”来确定这些文件 。  “IVssComponent::GetPartialFile”将返回指向该文件的路径和文件名，以及指示需要在文件中备份的内容的范围字符串。

有关部分文件信息检索的详细信息，请参阅 [VSS 文档](/windows/win32/vss/volume-shadow-copy-service-overview)。

### <a name="back-up-files"></a>备份文件

在此阶段，备份应用程序应查看存储在备份组件文档中的编写器元数据，并仅备份文件的相关部分。 （对于全文目录文件，此备份应基于文件时间戳完成。 此部分内容将在本文档后面进行介绍）。

差异备份将始终与数据库的最新基准备份相关。  在还原时，SQL Server 将检测不匹配的基准备份和差异备份。 因此，备份应用程序或系统管理员有责任确保差异与预期的完整备份有关。  如果某个带外过程进行了另一次完整备份，备份应用程序可能无法还原差异，因为它不“拥有”基准备份。

当前，如果字节范围信息（部分文件信息）太大（缓冲区大小超过 64K 字节），SQL Server 将引发指示用户执行完整备份的错误。

### <a name="interesting-cases-during-backup"></a>备份期间的有趣示例

添加/删除/收缩/增长/逻辑重命名/物理重命名的文件在备份过程中将生成有趣的示例。

**获取基准后新添加的文件**

这些文件包含在部分规范中，因为 SQL 数据库文件的每个标头都需要在部分规范中。  除标题页之外，所有分配的页都需要包含在部分规范中。

**获取基准后删除的文件**

获取基准后，可以删除数据文件。  在差异备份期间，编写器元数据文档中不包含此类文件。  此外，不会有与删除的文件相关联的部分信息。

**获取基准后收缩的文件**

在服务器中禁用文件收缩之前，不会从文件收集部分信息。  这可确保永远不会包含任何与数据文件收缩区域相对应的部分信息。  

**获取基准后增长的文件**

如果增长发生在收集部分信息之前，那么部分信息应包括增长区域中分配的页面。  如果增长发生在收集部分信息之后，则部分信息不包括增长区域的变化。  在下面的部分中，我们将看到此类更改将由日志前滚还原。

**获取基准后以逻辑方式重命名的文件**

文件的逻辑重命名不会影响备份或恢复，因为编写器元数据文档或备份组件文档中没有引用文件的逻辑名称。  （请参阅编写器元数据文档：“示例”部分了解详细信息。）

**获取基准后以物理方式重命名的文件**

除非重启物理数据库，否则物理数据库文件重命名不会生效。  因此，部分信息缓冲区中的数据库配置信息或文件路径信息仍然基于旧的物理路径，这些物理路径是快照上这些数据库文件的惟一有效路径。

### <a name="restore"></a>还原

在差异还原期间，请求程序返回给 SQL 编写器的备份元数据具有备份类型信息。  因此，不需要对 SQL 编写器进行特殊处理。  SQL Server 会发现它本身就是一个差异还原。  SQL Server 处理这种差异还原的方式与处理不通过 VSS 执行的本机差异还原的方式相同。

### <a name="prerestore-phase"></a>预还原阶段

在此阶段，SQL Server 将根据差异备份的文件元数据把所有文件调整到适当的大小。  如果文件已增长，则 SQL Server 会将增长部分归零。  如果必须创建一个新文件（该文件是在获取基准文件后创建的），SQL Server 会将新文件归零。 它还会关闭所有文件句柄，以便备份应用程序能够用备份介质中恢复的数据覆盖文件。

### <a name="restore-files"></a>还原文件

客户端应根据部分文件规范恢复文件。  如编写器元数据中存储的部分文件规范中所述，应将数据还原到数据库文件的相同偏移量/范围。

添加/删除/收缩/增长/逻辑重命名/物理重命名的数据库文件在还原时将再次生成有趣的示例。

**如果数据库文件是在获取完整基准后添加的**

这些文件必须在还原准备阶段由 SQL Server 预先创建。  它们应已扩展到正确大小，并已归零。客户端只需要按照部分规范放置数据（部分规范包括所有分配的区段）。

**如果数据库文件是在获取完整基准后删除的**

SQL Server 提供的部分信息不包括此类文件删除的任何跟踪信息。  SQL Server 负责检测要删除的文件，方法是将还原的文件元数据与现有容器进行比较，并实际删除它们。  这是还原之前的准备步骤。

**如果数据库文件是在获取完整基准后增长的**

在还原准备阶段，SQL Server 必须将这些文件扩展到正确的大小。  扩展区域也必须由 SQL Server 归零。  因此，客户端甚至可以按照部分规范在增长区域中安全地布局数据。  如果文件是在获取部分信息之后生成的，则将通过重新播放与差异备份一起备份的日志来还原增长区域中的更改。

**如果数据库文件是在获取完整基准后收缩的**

SQL Server 负责根据元数据将文件截断到所需的大小。  这是还原之前的准备步骤。

**如果数据库文件是在获取完整基准后进行逻辑重命名的**

这不会影响还原，因为逻辑名称不会出现在编写器元数据文档或备份组件文档中。  当客户端将更改应用于包含系统目录信息的主数据库文件时，将还原逻辑名称更改。

**如果数据库文件是在获取完整基准后进行物理重命名的。**

如果在进行差异备份时，重命名尚未生效，则客户端仍会将数据还原到旧位置。  还原后重启数据库将使物理重命名生效。  如果在进行差异备份时，物理文件重命名已经生效，则部分数据（如果有）将从新的物理路径备份。  

### <a name="post-restore"></a>还原后

在还原后事件期间，SQL 编写器将执行数据库的常规重做操作和恢复（如果 SetAdditionalRestores() 设置为 False）。

## <a name="differential-backuprestore-for-full-text-catalogs"></a>全文目录的差异备份/还原

SQL Server 全文目录是需要与其他数据库文件一起备份或还原的数据库资源的一部分。  对于全文目录，差异备份基于时间戳。  SQL Server VSS 差异备份/还原具有单个基准备份。  换句话说，不同的容器不会有不同的基准。  对于 VSS 全文目录备份，这意味着对于所有全文目录容器，差异备份将基于单时间戳，而不像本机 SQL 差异备份，其中每个全文目录容器都有一个时间戳基础。

在 VSS 中，此时间戳表示为组件范围内的属性，该属性在完整备份期间进行设置并在后续差异备份期间使用。  

### <a name="onidentify"></a>OnIdentify

在 OnIdentify 中，SQL 编写器调用 IVssCreateWriterMetadata::SetBackupSchema() 来设置值 VSS_BS_TIMESTAMPED。  这向备份应用程序指示 SQL 编写器拥有对差异基准的管理。

### <a name="setting-the-base-timestamp"></a>设置基本时间戳

基本时间戳在完整备份期间进行设置。  在“OnPostSnapshot()”中，编写器调用“IVssComponent::SetBackupStamp()”将时间戳与组件一起存储在备份文档中 。

### <a name="differential-backup"></a>差异备份

备份应用程序将从基本完整备份中检索此时间戳，并通过调用“IVssComponent::GetBackupStamp()”从上一个基本备份中检索基本时间戳，使该时间戳对编写器可用。  然后通过调用“IVssBackupComponent::SetPreviousBackupStamp()”使它对编写器可用。  然后，编写器通过调用 IVssComponent::GetPreviousBackupStamp() 检索该时间戳，并将其转换为可用于 IVssComponent::AddDifferencedFilesByLastModifyTime() 的时间戳 。  

**备份应用程序在差异备份期间的责任** 在差异备份期间，备份应用程序负责：

- 备份上次修改时间戳大于组件中文件集的“上次修改时间”指定的时间戳的任何文件（整个文件）。
- 跟踪和检测已删除的文件。  

**备份应用程序在差异还原期间的责任** 在差异还原期间，备份应用程序负责：

- 还原所有已备份的文件，如果新文件不存在，则创建新文件；如果已存在现有文件，则覆盖现有文件。
- 如果还原的文件比现有文件大，则在放置内容之前先增大文件。
- 如果还原文件小于现有文件，则将该文件截断为与还原文件相同的大小。
- 删除所有需要删除的文件；也就是说，这些文件在差异备份时不应存在。

### <a name="copy-only-backup"></a>仅复制备份

有时有必要为特殊目的进行备份。 例如，出于测试目的，可能需要创建数据库的副本。  这种备份不会影响数据库的总体备份和恢复进程。 使用 COPY_ONLY 选项指定备份是在“带外”完成的，不应影响备份的正常顺序。 SQL 编写器支持 SQL Server 实例的“仅复制”备份类型。

在备份发现阶段，SQL 编写器将通过使用“IVssCreateWriterMetadata::SetBackupSchema”调用设置受支持的备份架构选项 VSS_BS_COPY 来指示其执行仅复制备份的能力。 请求程序可以通过调用“IVssBackupComponents::SetBackupState”将 VSS_BACKUP_TYPE 选项设置为 VSS_BT_COPY，将备份类型设置为仅复制备份。

选择仅复制备份时，假定磁盘上的文件将被复制到备份介质（由请求程序复制），而不考虑每个文件的备份历史记录的状态。 SQL Server 不会更新备份历史记录。 这种类型的备份将不会构成进一步差异备份操作的基准备份，而且它也不会影响以前差异备份的历史。

### <a name="restore-with-move"></a>移动式还原

VSS 允许备份应用程序（请求程序）使用“IVssComponent::SetNewTarget”调用指定新的还原目标。  在 PreRestore() 和 PostRestore() 中，SQL 编写器检查是否至少指定了一个新目标。 备份应用程序负责在实际文件还原/复制时间内将文件物理复制到新位置。

只允许备份应用程序为物理路径指定新目标，而不允许指定文件规范。  例如，对于位于 c:\data\test.mdf 的数据库文件，不能更改实际的文件名 test.mdf。  只能更改路径 c:\data。  对于位于 c:\ ftdata\foo 的全文目录容器，由于 VSS 中的文件规范为“*”，而 VSS 中的路径规范为 c:\ ftdata\foo，因此可以更改整个路径。  

### <a name="database-rename"></a>数据库重命名

请求程序可能需要用新名称还原 SQL 数据库，特别是当数据库要与原始数据库并排还原时。  在还原操作期间，请求程序可以通过使用 VSS 调用“IVssBackupComponents::SetRestoreOptions()”（在 wszRestoreOptions 参数中）将自定义还原选项设置为 "New Component Name" = <"New Name"> 来指定此选项。

SQL 编写器将获取新组件名称值的全部内容，并将其用作还原数据库的新名称。 如果未指定任何选项，SQL 将使用其原始名称（组件名称）还原数据库。

  > [!NOTE]
  > SQL 编写器当前不支持将数据库移动到新实例的“跨实例重命名”。

### <a name="auto-recovered-snapshots"></a>自动恢复的快照

通常，通过使用 VSS 框架获得的 SQL Server 数据库快照会处于未恢复状态。 在进入恢复阶段以回滚运行中的事务并将数据库置于一致状态之前，无法安全地访问快照中的数据。 由于快照处于只读状态，无法通过附加数据库的正常过程恢复快照。

作为快照创建过程的一部分，可以自动恢复快照。 作为编写器元数据文档的一部分，SQL 编写器将指定组件标记“VSS_CF_APP_ROLLBACK_RECOVERY”，以指示在指定快照集时需要先对快照上的数据库执行恢复，然后才能访问数据库，请求程序可以指示快照应为应用程序回滚快照（即，快照中的所有数据库文件对于应用程序的使用都应处于一致状态）或备份快照（用于备份在系统发生故障时稍后要还原的数据的快照）。

请求程序应设置 VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY，以指示应出于非备份目的备份此组件。  然后，VSS 将选定组件上指定的 SQL 编写器的 VSS_CF_APP_ROLLBACK_RECOVERY 与 VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY 关联起来，并确认正在进行自动恢复。  VSS 将使快照在有限的时间内可写入，并自动将 VSS_VOLSNAP_ATTR_AUTORECOVERY 位添加到快照上下文中。

对于 SQL Server，自动恢复应仅应用于应用回滚快照，而不应用于备份快照。 对于应用回滚快照，SQL 编写器在 PostSnapShotevent 期间启动自动恢复过程。 此过程将对快照集中每个显式选择（由请求程序选择）的 SQL Server 数据库执行以下操作：

- 将快照数据库附加到原始 SQL Server 实例（即原始数据库附加到的实例）。
- 恢复数据库（这是“附加”操作的一部分）。
- 收缩日志文件。

    如果 VSS 提供程序为“软件提供程序”，这就减少了需要由 VSS 框架完成的不必要的“写入时复制”数量。 收缩日志文件是默认行为。 可以通过将以下注册表项的值设置为 1 来禁用此行为。
    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SQLWriter\
    Settings\DisableLogShrink
    
    当快照可用于从日志中导出特定页（在特定时间点）的数据以修复联机数据库中的问题时，这可能非常有用。

- 分离数据库。

现在我们有了一个一致的、已恢复的快照，可以附加该快照以便进行查询。

### <a name="multi-database-transactions"></a>多数据库事务

在某些情况下，快照数据库可能包含一些正在运行的多数据库事务。 在恢复操作期间，SQL 编写器将使用假定中止选项将数据库附加到快照上。 这将回滚任何尚未提交的多数据库事务（包括处于准备提交状态的任何事务）。 回滚可能会导致快照集中的数据库之间出现一些不一致。 例如，考虑两个数据库 A 和 B。这两个数据库之间有一个分布式事务，该事务在数据库 A 中处于提交状态，在数据库 B 中处于准备提交状态。作为自动恢复过程的一部分，此事务将在数据库 A 中提交，并在数据库 B 中回滚。这可能会导致快照集中出现一些不一致。

将由 VSS 框架在 Longhorn 时间范围内发布的 Microsoft 分布式事务处理协调器 (MS DTC) 组件将解决跨 SQL Server 实例跨数据库的事务的不一致问题。 下一版本的 SQL Server 将修复 SQL Server 实例中跨越数据库的事务的这些不一致性。

### <a name="security-implications-for-autorecovered-snapshots"></a>自动恢复快照的安全隐患

对于 VSS 快照，在自动恢复之后，将使用访问控制列表 (acl) 保护文件，以便仅允许访问 SQL Server 帐户所属的特殊内置组。  这意味着“box 管理员”或该特殊组的成员将能够附加数据库。 请求在快照上附加数据库文件的客户端必须是内置/管理员或 SQL Server 帐户的成员。

### <a name="special-cases"></a>特殊情况

本节介绍基于 SQL 编写器的备份和还原操作期间遇到的一些特殊情况。

#### <a name="autoclose-databases"></a>自动关闭数据库

对于基于非组件的备份，在检查损坏情况时会自动关闭数据库，但在备份操作期间不会显式冻结自动关闭的数据库。

此处的预期情况是，可能存在许多已关闭的数据库，我们希望将快照的成本降至最低。  自动关闭的数据库通常用于资源稀缺的低端配置。

#### <a name="file-list"></a>文件列表

每个数据库的文件列表都是在“准备备份”事件之前的枚举步骤中进行确定的。  如果数据库文件列表在枚举和冻结之间发生更改，则数据库可能会被删除，除非应用程序重新检查文件列表。 我们并不认为这是一个问题，但这是供应商需要注意的问题。

#### <a name="stopped-instances"></a>停止的实例

如果在枚举步骤发生时没有运行 SQL Server 实例，则无法选择这些实例的任何数据库。

如果实例在枚举和“准备备份”事件之间的间隔内停止，则将忽略已停止实例中的任何数据库。

#### <a name="system-and-user-databases"></a>系统和用户数据库

SQL Server 中的系统数据库包括作为 SQL Server 一部分提供和安装的主数据库、模型数据库和 msdb 数据库。 本节介绍如何在 VSS 快照备份过程中处理这些数据库。

### <a name="system-databases"></a>系统数据库

主数据库只能通过停止实例、替换数据库文件（由备份应用程序完成）然后重启实例来还原。 不能前滚。

SQL 编写器支持联机还原模型和 msdb 数据库，而无需关闭实例。


#### <a name="simple-recovery-model-user-databases"></a>简单恢复模型用户数据库

如果主数据库与使用简单恢复模型的用户数据库一起还原，则可以使用与主数据库相同的技术还原用户数据库：关闭实例后，只需复制或装载卷。  启动 SQL 实例时，所有内容都将恢复。

#### <a name="rolling-forward-user-databases"></a>前滚用户数据库

如果要将用户数据库与主数据库恢复一起恢复和前滚，则实例不得同时启动和恢复主数据库和用户数据库。

该过程如下：

1. 确保 SQL Server 实例已停止。
1. 分两个阶段执行还原。
    1. 通过 VSS 的文件复制/卷装载，还原应同时恢复的系统数据库和用户数据库（即简单恢复模式的用户数据库）。
        1. 如果要前滚的用户数据库与系统数据库不在同一卷上，则此时不应恢复该卷。 此方案需要在备份之前进行规划。
        1. 如果用户数据库与系统数据库位于同一卷上，则需要对 SQL Server 隐藏用户数据库。
    1. 使用 -f 参数启动 SQL Server 实例。  （使用 -f 启动选项时，只能还原主数据库。）
        1. 对要前滚的每个数据库发出 ALTER DATABASE \<x> SET OFFLINE。  （分离数据库是一种替代方法）
        1. 停止 SQL Server 实例。
        1. 启动 SQL Server 实例（要前滚的用户数据库的文件对 SQL Server 不可见）。

使用 VSS 还原用户数据库 WITH NORECOVERY，如“使用前滚完整还原”中所述。

## <a name="appendix"></a>附录

### <a name="writer-metadata-document--an-example"></a>编写器元数据文档：示例

给定一个名为 DB1 的示例数据库，该数据库属于计算机 Server1 上的 SQL Server 实例 Instance1，其中包含以下数据库/日志文件：

- 存储在 c:\db\DB1.mdf 的名为“primary”的数据库文件
- 存储在 c:\db\DB1.ndf 的名为“secondary”的数据库文件
- 存储在 c:\db\DB1.ldf 的名为“log”的数据库日志文件
- 存储在 c:\db\ftdata\foo 的名为“foo”的全文目录
- 存储在 c:\db\ftdata\bar 的名为“bar”的全文目录。

下面是数据库的编写器元数据：

**数据库级别文件组组件**

- ComponentType:VSS_CT_FILEGROUP
- LogicalPath:"Server1\Instance1"
- ComponentName:"DB1"
- Caption:Null
- pbIcon:Null
- cbIcon:0
- bRestoreMetadata:FALSE
- NotifyOnBackupComplete:TRUE
- Selectable:TRUE
- SelectableForRestore:TRUE
- ComponentFlags:VSS_CF_APP_ROLLBACK_RECOVERY

**文件组文件**

- LogicalPath:"Server1\Instance1"
- GroupName:"DB1"
- Path: "c:\db"
- FileSpec:"DB1.mdf"
- Recursive:FALSE
- AlternatePath:Null
- BackupTypeMask:VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED
- 文件组文件
- LogicalPath:"Server1\Instance1"
- GroupName:"DB1"
- Path: "c:\db"
- FileSpec:"DB1.ndf"
- Recursive:FALSE
- AlternatePath:Null
- BackupTypeMask:VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**文件组文件**

- LogicalPath:"Server1\Instance1"
- GroupName:"DB1"
- Path: "c:\db"
- FileSpec:"DB1.ldf"
- Recursive:FALSE
- AlternatePath:Null
- BackupTypeMask:VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**文件组文件：**

- LogicalPath:"Server1\Instance1"
- GroupName:"DB1"
- Path: "c:\db\ftdata\foo"
- FileSpec: "*"
- Recursive:TRUE
- AlternatePath:Null
- BackupTypeMask:VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**文件组文件：**

- LogicalPath:"Server1\Instance1"
- GroupName:"DB1"
- Path: "c:\db\ftdata\bar"
- FileSpec: "*"
- Recursive:TRUE
- AlternatePath:Null
- BackupTypeMask:VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

如果服务器实例是计算机上的默认实例，则逻辑路径将成为一部分 -“Server1”。


    
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
