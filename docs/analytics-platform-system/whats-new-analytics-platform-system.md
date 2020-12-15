---
title: 新增功能
description: 查看 Microsoft Analytics Platform System 中的新增功能，这是托管 MPP SQL Server 并行数据仓库的扩展本地设备。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 693e724256d8792581770e7fde4f4e6c991701bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97420164"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>分析平台系统中的新增功能-横向扩展 MPP 数据仓库
请参阅 Microsoft Analytics Platform System (AP) 最新的设备更新中的新增功能。 AP 是托管 MPP SQL Server 并行数据仓库的扩展本地设备。 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
发布日期-2020 年11月

### <a name="scvmm2016"></a>SCVMM2016
AP CU 7.7 软件将 VMM VM 升级到 Windows Server 2016 并安装 SCVMM2016。 当前正在使用的 SCVMM 2012 R2 在7月2022日结束。 需要支持更新的 SCVMM，使 CU 7.7 成为必需的升级。 客户劝尽快升级到 CU 7.7。

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>作为目标的 SQL Server 2019 的 SSIS 目标适配器
可以从 [下载站点](https://www.microsoft.com/download/details.aspx?id=57472)下载支持 SQL Server 2019 作为部署目标的新的 ap SSIS 目标适配器。

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
发布日期-2020 年4月

### <a name="rename-column"></a>重命名列
升级到 CU 7.6 后，客户将能够重命名用户创建的表的列。 有关语法、示例、限制和详细信息，请参阅 [RENAME (transact-sql) ](../t-sql/statements/rename-transact-sql.md) 。

### <a name="alter-view"></a>更改视图
客户现在可以更改视图。 有关详细信息，请参阅 [ALTER VIEW (transact-sql) ](../t-sql/statements/alter-view-transact-sql.md) 。

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
发布日期-2019 年9月

### <a name="alter-external-data-source"></a>更改外部数据源
客户将可以通过 CU 7.5 更新来更改外部数据源定义。 使用 Hadoop 名称节点高可用性的客户现在可以在发生故障转移时更改数据源以更改参数。 对于 AP，只能更改位置、RESOURCE_MANAGER_LOCATION 和凭据。 有关详细信息，请参阅 [alter external data source](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017) 。

### <a name="cdh-515-and-516-support-with-polybase"></a>支持 PolyBase 的 CDH 5.15 和5.16
具有 CU 7.5 更新的接入点上的 PolyBase 现在支持来自 Cloudera 的 CDH 5.15 和5.16 版本的 Hadoop 分发版。 将选项6用于 CDH 4.x 版本。 

### <a name="try_convert-and-try_cast-support"></a>Try_Convert 和 Try_Cast 支持
CU 7.5 AP 现在支持 [TRY_CAST](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017) 和 [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017) tsql 函数。 如果转换成功，则这两个函数都将返回转换为指定数据类型的值;否则，将返回 null。

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
发布日期-可能为2019

### <a name="loading-large-rows-with-dwloader"></a>用 dwloader 加载大量行
从 AP CU 7.4 开始，客户将能够使用新的 dwloader 将行加载到大于 32 KB (32768 字节) 的表中。 新的 dwloader 支持-l 开关，该开关采用32768到 33554432 (之间的整数值) 以加载大于 32 KB 的行。 仅当加载 (大于 32 KB) 的大行时才使用此选项，因为此开关将在客户端和服务器上分配更多的内存，并可能会减慢负载。 可以从 [下载站点](https://www.microsoft.com/download/details.aspx?id=57472)下载新 dwloader。  

### <a name="hdp-30-and-31-support-with-polybase"></a>支持 PolyBase 的 HDP 3.0 和3。1
现在，PolyBase 接入点支持 HDP 3.0 和3.1 进行此更新。 将选项7用于 HDP 2.x 版本。 有关详细信息，请参阅 [PolyBase 连接](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) 页。

### <a name="utf16-file-support-with-polybase"></a>与 PolyBase 的 UTF16 文件支持
PolyBase 现在支持读取 UTF16 (LE) 编码的带分隔符的文本文件。 有关设置的详细信息，请参阅 [创建外部文件格式](../t-sql/statements/create-external-file-format-transact-sql.md) 。 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
发布日期-2018 年12月

### <a name="common-subexpression-elimination"></a>常见子表达式清除
通过在 SQL 查询优化器中消除常见子表达式，可以通过 AP CU 7.3 提高查询性能。 改进通过两种方式改进了查询。 第一项优势是能够识别和消除此类表达式，有助于减少 SQL 编译时间。 第二个和更重要的优点是，这些冗余子表达式的数据移动操作被消除，因此查询的执行时间更快。 可在 [此处](common-sub-expression-elimination.md)找到有关此功能的详细说明。

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>AP Informatica connector for Informatica 10.2.0 已发布
我们发布了新版本的 Informatica 连接器，适用于使用 Informatica 版本10.2.0 和10.2.0 修补程序1的 AP。 可以从 [下载站点](https://www.microsoft.com/download/details.aspx?id=57472)下载新的连接器。
> [!NOTE]
> 用于 Informatica 10.2.0 或10.2.0 修补程序1的 AP Informatica connector 不适用于 strict TLS 1.2，要求 TLS 1.0 和1.1 完全正常运行。

#### <a name="supported-versions"></a>支持的版本

| AP 版本 | Informatica PowerCenter | 驱动程序 |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| AP 2016 及更高版本 | 10.2.0，10.2.0 修补程序1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
发布日期-2018 年10月

### <a name="support-for-tls-12"></a>支持 TLS 1.2
接入点 CU 7.2 支持 TLS 1.2。 现在可以将客户端计算机到 AP 和 AP 的内部通信设置为仅通过 TLS 1.2 进行通信。 安装在客户端计算机上且设置为仅在 TLS 1.2 上通信的 SSDT、SSIS 和 Dwloader 等工具现在可以使用 TLS 1.2 连接到 AP。 默认情况下，AP 将支持所有 TLS (1.0、1.1 和 1.2) 版本，以实现向后兼容性。 如果希望将你的 AP 设备设置为严格使用 TLS 1.2，你可以通过更改注册表设置来实现此目的。 

有关详细信息，请参阅 [在 ap 上配置 tls 1.2](configure-tls12-aps.md)。

### <a name="hadoop-encryption-zone-support-for-polybase"></a>适用于 PolyBase 的 Hadoop 加密区域支持
PolyBase 现在可以与 Hadoop 加密区域通信。 请参阅 [配置 Hadoop 安全性](polybase-configure-hadoop-security.md#encryptionzone)中所需的 ap 配置更改。

### <a name="insert-select-maxdop-options"></a>Insert-Select maxdop 选项
我们添加了 [功能开关](appliance-feature-switch.md) ，使你可以为插入-选择操作选取大于1的 maxdop 设置。 你现在可以将 maxdop 设置设置为0、1、2或4。 默认值为 1。

> [!IMPORTANT]  
> 增加 maxdop 有时会导致操作缓慢或死锁错误。 如果出现这种情况，请将该设置改回为 maxdop 1，然后重试该操作。

### <a name="columnstore-index-health-dmv"></a>列存储索引运行状况 DMV
可以使用 **dm_pdw_nodes_db_column_store_row_group_physical_stats** dmv 查看列存储索引运行状况信息。 使用以下视图来确定碎片，并决定何时重新生成或重新组织列存储索引。

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>ORC 和 Parquet 文件的 PolyBase 日期范围增加
使用 PolyBase 读取、导入和导出日期数据类型现在支持 ORC 和 Parquet 文件类型之前1970-01-01 到2038-01-20 之前的日期。

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>作为目标的 SQL Server 2017 的 SSIS 目标适配器
可以从 [下载站点](https://www.microsoft.com/download/details.aspx?id=57472)下载支持 SQL Server 2017 作为部署目标的新的 ap SSIS 目标适配器。

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
发布日期-2018 年7月

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>DBCC 命令不会使用并发槽 (行为更改) 
AP 支持 T-sql [dbcc 命令](../t-sql/database-console-commands/dbcc-transact-sql.md) （如 [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md)）的子集。 以前，这些命令会占用[并发插槽](./workload-management.md?view=aps-pdw-2016-au7#concurrency-slots)，从而减少了可以执行的用户加载/查询数。 `DBCC`现在，命令运行在不使用用户并发槽的本地队列中，从而提高了总体查询执行性能。

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>用目录对象替换某些元数据调用
使用目录对象进行元数据调用，而不是使用 SMO 在 AP 中显示了性能改进。 从 CU 7.1 开始，某些元数据调用现在默认使用目录对象。 如果使用元数据查询的客户遇到任何问题，则可以通过 [功能开关](appliance-feature-switch.md) 禁用此行为。

### <a name="bug-fixes"></a>Bug 修复
我们已升级到 SQL Server 2016 SP2 CU2 与 AP CU 7.1。 升级修复了下面所述的一些问题。

| Title | 说明 |
|:---|:---|
| **潜在元组移动器死锁** |升级修复了分布式事务和元组移动器后台线程中可能出现的死锁。 安装 CU 7.1 后，使用 TF634 停止元组移动器 SQL Server 启动参数或全局跟踪标志的客户可以安全地将其删除。 | 
| **某些滞后/线索查询失败** |对于包含嵌套延迟/潜在顾客函数（将出错）的 CCI 表的某些查询，此升级现已修复。 | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
发布日期-可能为2018

AP 2016 是升级到 AU7 的先决条件。 下面是 AP AU7 中的新功能：

### <a name="auto-create-and-auto-update-statistics"></a>自动创建和自动更新统计信息
默认情况下，AP AU7 自动创建和更新统计信息。 若要更新统计信息设置，管理员可以在 [Configuration Manager](appliance-configuration.md#CMTasks)中使用新功能切换菜单项。 [功能开关](appliance-feature-switch.md)控制统计信息的自动创建、自动更新和异步更新行为。 还可以通过 [ALTER DATABASE (Parallel 数据仓库) ](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) 语句来更新统计信息设置。

### <a name="t-sql"></a>T-SQL
选择 " @var 现在支持"。 有关详细信息，请参阅 [select 局部变量](../t-sql/language-elements/select-local-variable-transact-sql.md) 

现在支持查询提示哈希和订单组。 有关详细信息，请参阅 [ (transact-sql) 的提示](../t-sql/queries/hints-transact-sql-query.md)

### <a name="feature-switch"></a>功能切换
AP AU7 引入 [Configuration Manager](launch-the-configuration-manager.md)中的功能开关。 AutoStatsEnabled 和 DmsProcessStopMessageTimeoutInSeconds 现在可以由管理员更改。

### <a name="known-issues"></a>已知问题
借助 AP AU7 software，提供 Intel BIOS 更新，可修复描述为 *推理执行方通道攻击* 的问题。 攻击旨在利用被称为 *Spectre 和 Meltdown 漏洞的漏洞*。 尽管与 AP 一起打包，但 BIOS 更新是手动安装的，而不是作为 AP AU7 software 安装的一部分。

Microsoft 建议所有客户安装 BIOS 更新。 Microsoft 已衡量内核虚拟地址影 (KVAS) 、内核页表间接寻址 (KPTI) 和间接分支预测缓解 (措施在各种环境中的各种 SQL 工作负载上的影响。 度量值明显降低了某些工作负荷。 根据结果，建议你在将 BIOS 更新部署到生产环境之前，对其进行测试，从而对性能产生影响。 请参阅 [此处](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown)SQL Server 指南。

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
本部分介绍了用于 AP 2016-AU6 的新增功能。

### <a name="sql-server-2016"></a>SQL Server 2016

AP AU6 在 2016 SQL Server 最新版本上运行，并使用默认的数据库兼容级别130。 SQL Server 2016 支持新功能，例如：

- 聚集列存储索引的辅助索引。
- 用于 PolyBase 的 Kerberos。

### <a name="t-sql"></a>T-SQL
AP AU6 支持这些 T-sql 兼容性改进。  这些附加的语言元素使您可以更轻松地从 SQL Server 和其他数据源进行迁移。 

- 除了 Windows 排序规则外，现在还支持[列级 SQL 排序规则][]。
- [聚集列存储索引的非聚集索引][] 可提高在聚集列存储索引中搜索特定值的查询的性能。 
- [选择 .。。为][] 
- [sp_spaceused ( # B1 ][] 显示表或数据库中使用或保留的磁盘空间。
- [宽表][] 支持与 SQL Server 2016 相同。 对于行大小，先前的限制 32 K 已不再存在。 

**数据类型**

- [VARCHAR (max) ][]、 [NVARCHAR (Max) ][] 和 [VARBINARY (max) ][]。 这些 LOB 数据类型的最大大小为 2 GB。 使用 [Bcp 实用工具][]加载这些对象。 PolyBase 和 dwloader 目前不支持这些数据类型。 
- [SYSNAME][]
- [UNIQUEIDENTIFIER][]
- [NUMERIC][] 和 DECIMAL 数据类型。

**开窗函数**

- SELECT 语句的 OVER 子句中的[行或范围][]。
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**安全功能**

- [校验和 ( # B1 ][] 和 [BINARY_CHECKSUM ( # B3 ][]
- [HAS_PERMS_BY_NAME ( # B1 ][]

**其他函数**

- [NEWID ( # B1 ][]
- [RAND ( # B1 ][]

### <a name="polybasehadoop-enhancements"></a>PolyBase/Hadoop 增强功能

- 与 Hortonworks HDP 2.4 和 HDP 2.5 兼容
- 通过数据库范围的凭据进行 Kerberos 支持
- 凭据支持与 Azure 存储 Blob

### <a name="install-and-upgrade-enhancements"></a>安装和升级增强功能

**企业体系结构更新** 将现有设备升级到 AP AU6 将安装最新的固件和驱动程序更新（包括安全修补程序）。 

HPE 或 DELL 提供的新设备包括所有最新的更新以及：

- 最新一代处理器支持 (Broadwell) 
- 更新到 DDR4 Dimm
- 提高了 DIMM 吞吐量

**集成**

- 完全限定的域名 (FQDN) 支持使你能够将域信任设置到设备。 
- 若要使用 FQDN，需要在升级过程中执行完整升级和选择加入。 

**缩短停机时间** 安装或升级到 AP AU6 速度更快，需要的停机时间比以前的版本更少。 为了减少停机时间，请安装或升级： 

 - 通过使用包含6月2016的所有更新的映像，简化 WSUS 更新的应用
 - 应用包含驱动程序和固件更新的安全更新
 - 将最新的修补程序和设备验证实用工具置于你的设备上 (PAV) ，以便它们可以进行安装，而无需下载它们。

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[列级 SQL 排序规则]: ~/relational-databases/collations/collation-and-unicode-support.md

[聚集列存储索引中的非聚集索引]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX) ]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX) ]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX) ]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[SYSNAME]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[选择 .。。为]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ( # B1 ]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[宽表]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[bcp 实用工具]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[行或范围]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[校验和 ( # B1 ]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ( # B1 ]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ( # B1 ]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID ( # B1 ]:/sql/t-sql/functions/newid-transact-sql
[RAND ( # B1 ]:/sql/t-sql/functions/rand-transact-sql


  

