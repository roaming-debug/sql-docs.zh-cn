---
title: 在 Linux 上配置 SQL Server 设置
description: 本文介绍如何使用 mssql-conf 工具在 Linux 上配置 SQL Server 设置。
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 72d5179d64cda06a95e531d8cbc847b33ce9b92f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237246"
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>使用 mssql-conf 工具配置 Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

mssql-conf 是随 SQL Server 2017 for Red Hat Enterprise Linux、SUSE Linux Enterprise Server 和 Ubuntu 安装的配置脚本。 它会修改存储配置值的 [mssql.conf 文件](#mssql-conf-format)。 可使用 mssql-conf 实用工具设置以下参数：

|参数|描述|
|---|---|
| [代理](#agent) | 启用 SQL Server 代理。 |
| [排序规则](#collation) | 为 Linux 上的 SQL Server 设置新排序规则。 |
| [客户反馈](#customerfeedback) | 选择 SQL Server 是否向 Microsoft 发送反馈。 |
| [数据库邮件配置文件](#dbmail) | 为 Linux 上的 SQL Server 设置默认数据库邮件配置文件。 |
| [默认数据目录](#datadir) | 更改新 SQL Server 数据库数据文件 (.mdf) 的默认目录。 |
| [默认日志目录](#datadir) | 更改新 SQL Server 数据库日志 (.ldf) 文件的默认目录。 |
| [默认 master 数据库目录](#masterdatabasedir) | 更改 master 数据库和日志文件的默认目录。|
| [默认 master 数据库文件名](#masterdatabasename) | 更改 master 数据库文件的名称。 |
| [默认转储目录](#dumpdir) | 更改新内存转储和其他故障诊断文件的默认目录。 |
| [默认错误日志目录](#errorlogdir) | 更改新 SQL Server 错误日志、默认探查器跟踪、系统健康状况会话 XE 和 Hekaton 会话 XE 文件的默认目录。 |
| [默认备份目录](#backupdir) | 更改新备份文件的默认目录。 |
| [转储类型](#coredump) | 选择要收集的转储内存转储文件的类型。 |
| [高可用性](#hadr) | 启用可用性组。 |
| [本地审核目录](#localaudit) | 设置目录以添加本地审核文件。 |
| [区域设置](#lcid) | 设置 SQL Server 要使用的区域设置。 |
| [内存限制](#memorylimit) | 设置 SQL Server 的内存限制。 |
| [网络设置](#network) | SQL Server 的其他网络设置。 |
| [Microsoft 分布式事务处理协调器](#msdtc) | 在 Linux 上配置 MSDTC 并对其进行故障排除。 |
| [TCP 端口](#tcpport) | 更改 SQL Server 侦听连接的端口。 |
| [TLS](#tls) | 配置传输级别安全性。 |
| [跟踪标志](#traceflags) | 设置服务要使用的跟踪标志。 |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

mssql-conf 是随 [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] for Red Hat Enterprise Linux、SUSE Linux Enterprise Server 和 Ubuntu 安装的配置脚本。 可以使用此实用工具设置以下参数：

|参数|描述|
|---|---|
| [代理](#agent) | 启用 SQL Server 代理 |
| [排序规则](#collation) | 为 Linux 上的 SQL Server 设置新排序规则。 |
| [客户反馈](#customerfeedback) | 选择 SQL Server 是否向 Microsoft 发送反馈。 |
| [数据库邮件配置文件](#dbmail) | 为 Linux 上的 SQL Server 设置默认数据库邮件配置文件。 |
| [默认数据目录](#datadir) | 更改新 SQL Server 数据库数据文件 (.mdf) 的默认目录。 |
| [默认日志目录](#datadir) | 更改新 SQL Server 数据库日志 (.ldf) 文件的默认目录。 |
| [默认 master 数据库文件目录](#masterdatabasedir) | 更改现有 SQL 安装中 master 数据库文件的默认目录。|
| [默认 master 数据库文件名](#masterdatabasename) | 更改 master 数据库文件的名称。 |
| [默认转储目录](#dumpdir) | 更改新内存转储和其他故障诊断文件的默认目录。 |
| [默认错误日志目录](#errorlogdir) | 更改新 SQL Server 错误日志、默认探查器跟踪、系统健康状况会话 XE 和 Hekaton 会话 XE 文件的默认目录。 |
| [默认备份目录](#backupdir) | 更改新备份文件的默认目录。 |
| [转储类型](#coredump) | 选择要收集的转储内存转储文件的类型。 |
| [高可用性](#hadr) | 启用可用性组。 |
| [本地审核目录](#localaudit) | 设置目录以添加本地审核文件。 |
| [区域设置](#lcid) | 设置 SQL Server 要使用的区域设置。 |
| [内存限制](#memorylimit) | 设置 SQL Server 的内存限制。 |
| [Microsoft 分布式事务处理协调器](#msdtc) | 在 Linux 上配置 MSDTC 并对其进行故障排除。 |
| [MLServices EULA](#mlservices-eula) | 对于 mlservices 包，接受 R 和 Python EULA。 仅适用于 SQL Server 2019。|
| [网络设置](#network) | SQL Server 的其他网络设置。 |
| [outboundnetworkaccess](#mlservices-outbound-access) |为 [mlservices](sql-server-linux-setup-machine-learning.md) R、Python 和 Java 扩展启用出站网络访问。|
| [TCP 端口](#tcpport) | 更改 SQL Server 侦听连接的端口。 |
| [TLS](#tls) | 配置传输级别安全性。 |
| [跟踪标志](#traceflags) | 设置服务要使用的跟踪标志。 |

::: moniker-end

> [!TIP]
> 还可以使用环境变量配置其中某些设置。 有关详细信息，请参阅[使用环境变量配置 SQL Server 设置](sql-server-linux-configure-environment-variables.md)。

## <a name="usage-tips"></a>使用提示

* 对于 Always On 可用性组和共享磁盘群集，始终在每个节点上进行相同的配置更改。

* 对于共享磁盘群集方案，请勿尝试重新启动 mssql-server 服务以应用更改。 SQL Server 作为应用程序运行。 应将资源脱机，然后重新联机。

* 这些示例通过指定完整路径 /opt/mssql/bin/mssql-conf 来运行 mssql-conf。 如果选择改为导航到该路径，请在当前目录 ./mssql-conf 的上下文中运行 mssql-conf。

## <a name="enable-sql-server-agent"></a><a id="agent"></a> 启用 SQL Server 代理

使用 sqlagent.enabled 设置可启用 [SQL Server 代理](sql-server-linux-run-sql-server-agent-job.md)。 默认情况下，SQL Server 代理处于禁用状态。 如果 mssql.conf 设置文件中不存在 sqlagent.enabled，则 SQL Server 在内部假定已禁用 SQL Server 代理。

若要更改此设置，请使用以下步骤：

1. 启用 SQL Server 代理：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

2. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="set-the-default-database-mail-profile-for-sql-server-on-linux"></a><a id="dbmail"></a> 为 Linux 上的 SQL Server 设置默认数据库邮件配置文件

通过 sqlpagent.databasemailprofile 可为电子邮件警报设置默认的 DB 邮件配置文件。

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```

### <a name="sql-agent-error-logs"></a><a id="agenterrorlog"></a> SQL 代理错误日志

sqlpagent.errorlogfile 和 sqlpagent.errorlogginglevel 设置允许你分别设置 SQL 代理日志文件路径和日志记录级别。 

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorfile <path>
```

SQL 代理日志记录级别是位掩码值，等于：

- 1 = 错误
- 2 = 警告
- 4 = 信息

如果要捕获所有级别，请使用 `7` 作为值。

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorlogginglevel <level>
```

## <a name="change-the-sql-server-collation"></a><a id="collation"></a> 更改 SQL Server 排序规则

使用 set-collation 选项可将排序规则值更改为支持的任何排序规则。

1. 首先，[备份服务器上的所有用户数据库](sql-server-linux-backup-and-restore-database.md)。

1. 然后，使用 [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) 存储过程分离用户数据库。

1. 运行 set-collation 选项并按照提示进行操作：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. mssql-conf 实用工具会尝试更改为指定的排序规则值并重新启动该服务。 如果出现任何错误，它会将排序规则回滚到前一个值。

1. 还原用户数据库备份。

若要获取支持的排序规则的列表，请运行 [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) 函数：`SELECT Name from sys.fn_helpcollations()`。

## <a name="configure-customer-feedback"></a><a id="customerfeedback"></a> 配置客户反馈

使用 telemetry.customerfeedback 设置可更改 SQL Server 是否向 Microsoft 发送反馈。 默认情况下，对于所有版本，此值设置为“true”。 若要更改该值，请运行以下命令：

> [!IMPORTANT]
> 无法关闭 SQL Server、Express 和 Developer 免费版本的客户反馈。

1. 使用 **telemetry.customerfeedback** 的 **set** 命令以根身份运行 mssql-conf 脚本。 以下示例通过指定 false 来关闭客户反馈。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

有关详细信息，请参阅 [Linux 上的 SQL Server 的客户反馈](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md) 和 [SQL Server 隐私声明](../sql-server/sql-server-privacy.md)。

## <a name="change-the-default-data-or-log-directory-location"></a><a id="datadir"></a> 更改默认数据或日志目录位置

使用 filelocation.defaultdatadir 和 filelocation.defaultlogdir 设置可更改创建新数据库和日志文件的位置 。 默认情况下，此位置为 /var/opt/mssql/data。 若要更改这些设置，请使用以下步骤：

1. 为新的数据库数据和日志文件创建目标目录。 以下示例创建一个新的 /tmp/data 目录：

   ```bash
   sudo mkdir /tmp/data
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. 使用 mssql-conf 通过 set 命令更改默认数据目录：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

1. 现在，为新数据库创建的所有数据库文件都将存储在此新位置。 如果要更改新数据库的日志文件 (.ldf) 位置，可以使用下面的“set”命令：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. 此命令还假定存在 /tmp/log 目录，并且它位于用户和组“mssql”下。


## <a name="change-the-default-master-database-file-directory-location"></a><a id="masterdatabasedir"></a> 更改默认的 master 数据库文件目录位置

使用 filelocation.masterdatafile 和 filelocation.masterlogfile 设置可更改 SQL Server 引擎查找 master 数据库文件的位置 。 默认情况下，此位置为 /var/opt/mssql/data。

若要更改这些设置，请使用以下步骤：

1. 为新的错误日志文件创建目标目录。 以下示例创建一个新的 /tmp/masterdatabasedir 目录：

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. 使用 mssql-conf 通过 set 命令更改主数据和日志文件的默认 master 数据库目录：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

   > [!NOTE]
   > 除了移动主数据和日志文件外，此操作还将移动所有其他系统数据库的默认位置。

1. 停止 SQL Server 服务：

   ```bash
   sudo systemctl stop mssql-server
   ```

1. 移动 master.mdf 和 masterlog.ldf：

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. 启动 SQL Server 服务：

   ```bash
   sudo systemctl start mssql-server
   ```

   > [!NOTE]
   > 如果 SQL Server 在指定目录中找不到 master.mdf 和 mastlog.ldf 文件，将在指定目录中自动创建系统数据库的模板化副本，并且 SQL Server 将成功启动。 但是，诸如用户数据库、服务器登录名、服务器证书、加密密钥、SQL 代理作业或旧 SA 登录密码等元数据将不会在新 master 数据库中更新。 必须停止 SQL Server 并将旧的 master.mdf 和 mastlog.ldf 移动到新的指定位置，然后启动 SQL Server 以继续使用现有元数据。
 
## <a name="change-the-name-of-master-database-files"></a><a id="masterdatabasename"></a> 更改 master 数据库文件的名称

使用 filelocation.masterdatafile 和 filelocation.masterlogfile 设置可更改 SQL Server 引擎查找 master 数据库文件的位置 。 还可以使用它来更改 master 数据库和日志文件的名称。 

若要更改这些设置，请使用以下步骤：

1. 停止 SQL Server 服务：

   ```bash
   sudo systemctl stop mssql-server
   ```

1. 使用 mssql-conf 通过 set 命令更改主数据和日志文件的预期 master 数据库名称：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data/mastlognew.ldf
   ```

   > [!IMPORTANT]
   > 只能在 SQL Server 成功启动后更改 master 数据库和日志文件的名称。 在初始运行之前，SQL Server 需要命名为 master.mdf 和 mastlog.ldf 的文件。

1. 更改 master 数据库数据和日志文件的名称 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. 启动 SQL Server 服务：

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="change-the-default-dump-directory-location"></a><a id="dumpdir"></a> 更改默认转储目录位置

使用 filelocation.defaultdumpdir 设置可更改每当系统崩溃时生成内存和 SQL 转储的默认位置。 默认情况下，这些文件在 /var/opt/mssql/log 中生成。

若要设置新位置，请使用以下命令：

1. 为新的转储文件创建目标目录。 以下示例创建一个新的 /tmp/dump 目录：

   ```bash
   sudo mkdir /tmp/dump
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. 使用 mssql-conf 通过 set 命令更改默认数据目录：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="change-the-default-error-log-file-directory-location"></a><a id="errorlogdir"></a> 更改默认的错误日志文件目录位置

使用 filelocation.errorlogfile 设置可更改创建新错误日志、默认探查器跟踪、系统健康状况会话 XE 和 Hekaton 会话 XE 文件的位置。 默认情况下，此位置为 /var/opt/mssql/log。 设置用于 SQL 错误日志文件的目录将成为其他日志的默认日志目录。

若要更改这些设置，请执行以下操作：

1. 为新的错误日志文件创建目标目录。 以下示例创建一个新的 /tmp/logs 目录：

   ```bash
   sudo mkdir /tmp/logs
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. 使用 mssql-conf 通过 set 命令更改默认错误日志文件名：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

`errorlog.numerrorlogs` 设置允许你指定在循环日志之前所维护的错误日志数。

## <a name="change-the-default-backup-directory-location"></a><a id="backupdir"></a> 更改默认备份目录位置

使用 filelocation.defaultbackupdir 设置可更改生成备份文件的默认位置。 默认情况下，这些文件在 /var/opt/mssql/data 中生成。

若要设置新位置，请使用以下命令：

1. 为新的备份文件创建目标目录。 以下示例创建一个新的 /tmp/backup 目录：

   ```bash
   sudo mkdir /tmp/backup
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. 使用 mssql-conf 通过“set”命令更改默认备份目录：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="specify-core-dump-settings"></a><a id="coredump"></a> 指定核心转储设置

如果某个 SQL Server 进程发生异常，SQL Server 会创建内存转储。

可使用两个选项控制 SQL Server 收集的内存转储类型：coredump.coredumptype 和 coredump.captureminiandfull 。 这两个选项与核心转储捕获的两个阶段相关。 

第一阶段的捕获由 coredump.coredumptype 设置控制，此设置确定异常期间生成的转储文件类型。 第二阶段由 coredump.captureminiandfull 设置启用。 如果 coredump.captureminiandfull 设置为 true，则会生成 coredump.coredumptype 指定的转储文件，还会生成第二个小型转储 。 将 coredump.captureminiandfull 设置为 false 可禁用第二次捕获尝试。

1. 确定是否使用 coredump.captureminiandfull 设置捕获小型转储和完全转储。

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    默认值：false

1. 使用 coredump.coredumptype 设置指定转储文件的类型。

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    默认值：miniplus

    下表列出了可能的 coredump.coredumptype 值。

    | 类型 | 说明 |
    |-----|-----|
    | **mini** | Mini 是最小的转储文件类型。 它使用 Linux 系统信息确定进程中的线程和模块。 转储仅包含主机环境线程堆栈和模块。 不包含间接内存引用或全局变量。 |
    | **miniplus** | MiniPlus 与 mini 相似，但它包括更多内存。 它理解 SQLPAL 的内部机制和主机环境，可将下面的内存区域添加到转储：</br></br> - 各种全局变量</br> - 超过 64TB 的所有内存</br> - - 在/proc/$pid/maps 中找到的所有命名区域</br> - 来自线程和堆栈的间接内存</br> - 线程信息</br> - 关联的 Teb 和 Peb</br> - 模块信息</br> - VMM 和 VAD 树 |
    | **filtered** | Filtered 采用基于减法的设计，其中包括进程中的所有内存，除非专门排除某些内存。 此设计理解 SQLPAL 的内部机制和主机环境，从转储中排除某些区域。
    | **full** | Full 是完整的过程转储，包括 /proc/$pid/maps 中的所有区域。 这并非由 coredump.captureminiandfull 设置控制。 |

## <a name="high-availability"></a><a id="hadr"></a> 高可用性

使用 hadr.hadrenabled 选项可在 SQL Server 实例上启用可用性组。 下面的命令通过将 hadr.hadrenabled 设置为 1 来启用可用性组。 必须重启 SQL Server，该设置才能生效。

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

有关如何将此项用于可用性组的信息，请参阅以下两个主题。

- [为 Linux 上的 SQL Server 配置 Always On 可用性组](sql-server-linux-availability-group-configure-ha.md)
- [为 Linux 上的 SQL Server 配置读取缩放可用性](sql-server-linux-availability-group-configure-rs.md)


## <a name="set-local-audit-directory"></a><a id="localaudit"></a> 设置本地审核目录

使用 telemetry.userrequestedlocalauditdirectory 设置可启用本地审核，并可设置创建本地审核日志的目录。

1. 为新的本地审核日志创建目标目录。 以下示例创建新的 **/tmp/audit** 目录：

   ```bash
   sudo mkdir /tmp/audit
   ```

1. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. 使用 **telemetry.userrequestedlocalauditdirectory** 的 **set** 命令以根身份运行 mssql-conf 脚本：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

有关详细信息，请参阅 [Linux 上的 SQL Server 客户反馈](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md)。

## <a name="change-the-sql-server-locale"></a><a id="lcid"></a> 更改 SQL Server 区域设置

使用 language.lcid 设置可将 SQL Server 区域设置更改为任何支持的语言标识符 (LCID)。 

1. 以下示例将区域设置更改为法语 (1036)：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. 重启 SQL Server 服务以应用更改：

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="set-the-memory-limit"></a><a id="memorylimit"></a> 设置内存限制

使用 memory.memorylimitmb 设置可控制 SQL Server 可用的物理内存量（以 MB 为单位）。 默认值为物理内存的 80%。

1. 使用 memory.memorylimitmb 的 set 命令以根用户身份运行 mssql-conf 脚本 。 以下示例将 SQL Server 可用的内存更改为 3.25 GB (3328 MB)。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. 重启 SQL Server 服务以应用更改：

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="additional-memory-settings"></a>其他内存设置

内存设置提供以下选项。

|选项 |描述 |
|--- |--- |
| memory.disablememorypressure | SQL Server 禁用内存压力。 值可以为 `true` 或 `false`。 |
| memory.memory_optimized | 启用或禁用 SQL Server 内存优化功能 - 永久性内存文件启示、内存保护。 值可以为 `true` 或 `false`。 |

## <a name="configure-msdtc"></a><a id="msdtc"></a> 配置 MSDTC

network.rpcport 和 distributedtransaction.servertcpport 设置用于配置 Microsoft 分布式事务处理协调器 (MSDTC) 。 要更改这些设置，请运行以下命令：

1. 使用 network.rpcport 的 set 命令以根用户身份运行 mssql-conf 脚本：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport <rcp_port>
   ```

2. 然后设置“distributedtransaction.servertcpport”设置：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport <servertcpport_port>
   ```

除了设置这些值之外，还必须为端口 135 配置路由和更新防火墙。 有关如何执行此操作的详细信息，请参阅[如何在 Linux 上配置 MSDTC](sql-server-linux-configure-msdtc.md)。

可以使用 mssql-conf 的其他几项设置来监视 MSDTC 并对其进行故障排除。 下表简要描述了这些设置。 有关其用法的详细信息，请参阅 Windows 支持文章[如何为 MS DTC 启用诊断跟踪](https://support.microsoft.com/help/926099/how-to-enable-diagnostic-tracing-for-ms-dtc-on-a-windows-based-compute)了解详细信息。

| mssql-conf 设置 | 说明 |
|---|---|
| distributedtransaction.allowonlysecurerpccalls | Configure secure only RPC calls for distributed transactions |
| distributedtransaction.fallbacktounsecurerpcifnecessary | 为分布式事务配置“仅安全”的 RPC 调用 |
| distributedtransaction.maxlogsize | DTC 事务日志文件大小 (MB)。 默认为 64MB |
| distributedtransaction.memorybuffersize | 存储跟踪的循环缓冲区大小。 此大小以 MB 为单位，默认为 10MB |
| distributedtransaction.servertcpport | MSDTC rpc 服务器端口 |
| distributedtransaction.trace_cm | 连接管理器中的跟踪 |
| distributedtransaction.trace_contact | 跟踪联系人池和联系人 |
| distributedtransaction.trace_gateway | 跟踪网关源 |
| distributedtransaction.trace_log | 日志跟踪 |
| distributedtransaction.trace_misc | 不能归入其他类别的跟踪 |
| distributedtransaction.trace_proxy | MSDTC 代理中生成的跟踪 |
| distributedtransaction.trace_svc | 跟踪服务和 .exe 文件启动 |
| distributedtransaction.trace_trace | 跟踪基础结构本身 |
| distributedtransaction.trace_util | 跟踪从多个位置调用的实用工具例程 |
| distributedtransaction.trace_xa | XA 事务管理器 (XATM) 跟踪源 |
| distributedtransaction.tracefilepath | 应存储跟踪文件的文件夹 |
| distributedtransaction.turnoffrpcsecurity | 为分布式事务启用或禁用 RPC 安全性 |

::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="accept-mlservices-eulas"></a><a id="mlservices-eula"></a> 接受 MLServices EULA

将[机器学习 R 或 Python 包](sql-server-linux-setup-machine-learning.md)添加到数据库引擎需要接受 R 和 Python 的开源分发许可条款。 下表枚举了与 mlservices EULA 相关的所有可用命令或选项。 对 R 和 Python 使用相同的 EULA 参数，具体取决于所安装的内容。

```bash
# For all packages: database engine and mlservices
# Setup prompts for mlservices EULAs, which you need to accept
sudo /opt/mssql/bin/mssql-conf setup

# Add R or Python to an existing installation
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml

# Alternative valid syntax
# Adds the EULA section to the INI and sets acceptulam to yes
sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y

# Rescind EULA acceptance and removes the setting
sudo /opt/mssql/bin/mssql-conf unset EULA accepteulaml
```

还可以将接受 EULA 的内容直接添加到 [mssql.conf 文件](#mssql-conf-format)：

```ini
[EULA]
accepteula = Y
accepteulaml = Y
```
:::moniker-end
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="enable-outbound-network-access"></a><a id="mlservices-outbound-access"></a> 启用出站网络访问

默认情况下，[SQL Server 机器学习服务](sql-server-linux-setup-machine-learning.md)功能中的 R、Python 和 Java 扩展的出站网络访问处于禁用状态。 若要启用出站请求，请使用 mssql-conf 设置“outboundnetworkaccess”布尔属性。

设置该属性后，重新启动 SQL Server Launchpad 服务以从 INI 文件中读取更新的值。 每当修改与扩展性相关的设置时，都会收到重启消息提醒。

```bash
# Adds the extensibility section and property.
# Sets "outboundnetworkaccess" to true.
# This setting is required if you want to access data or operations off the server.
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1

# Turns off network access but preserves the setting
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 0

# Removes the setting and rescinds network access
sudo /opt/mssql/bin/mssql-conf unset extensibility.outboundnetworkaccess
```

还可以将“outboundnetworkaccess”直接添加到 [mssql.conf 文件](#mssql-conf-format)：

```ini
[extensibility]
outboundnetworkaccess = 1
```
:::moniker-end

## <a name="change-the-tcp-port"></a><a id="tcpport"></a> 更改 TCP 端口

使用 network.tcpport 设置可更改 SQL Server 侦听连接的 TCP 端口。 默认情况下，此端口设置为 1433。 若要更改端口，请运行以下命令：

1. 使用“network.tcpport”的“set”命令以根用户身份运行 mssql-conf 脚本：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

2. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```

3. 连接到 SQL Server 后，必须在主机名或 IP 地址后用逗号 (,) 指定自定义端口。 例如，要使用 SQLCMD 进行连接，则需使用以下命令：

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a name="specify-tls-settings"></a><a id="tls"></a> 指定 TLS 设置

以下选项为在 Linux 上运行的 SQL Server 实例配置 TLS。

|选项 |说明 |
|--- |--- |
|**network.forceencryption** |如果为 1，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 强制加密所有连接。 默认情况下，此选项为 0。 |
|**network.tlscert** |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用于 TLS 的证书文件的绝对路径。 示例： `/etc/ssl/certs/mssql.pem`，证书文件必须可由 mssql 帐户访问。 Microsoft 建议使用 `chown mssql:mssql <file>; chmod 400 <file>` 限制对文件的访问。 |
|**network.tlskey** |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用于 TLS 的专用密钥文件的绝对路径。 示例：`/etc/ssl/private/mssql.key`，证书文件必须可由 mssql 帐户访问。 Microsoft 建议使用 `chown mssql:mssql <file>; chmod 400 <file>` 限制对文件的访问。 |
|**network.tlsprotocols** |SQL Server 允许的 TLS 协议列表（以逗号分隔）。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 始终尝试协商允许的最强协议。 如果客户端不支持任何允许的协议，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将拒绝连接尝试。  为实现兼容性，默认情况下允许所有支持的协议（1.2、1.1、1.0）。  如果客户端支持 TLS 1.2，Microsoft 建议仅允许 TLS 1.2。 |
|**network.tlsciphers** |指定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 允许将哪些密码用于 TLS。 必须按照 [OpenSSL 的密码列表格式](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html)设置此字符串的格式。 通常不需要更改此选项。 <br /> 默认情况下，允许使用以下密码： <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Kerberos keytab 文件的路径 |

有关使用 TLS 设置的示例，请参阅[加密与 Linux 上的 SQL Server 的连接](sql-server-linux-encrypted-connections.md)。

## <a name="network-settings"></a><a id="network"></a> 网络设置

请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)，以获取有关在 Linux 上的 SQL Server 中使用 AD 身份验证的全面信息。

以下选项是可使用 `mssql-conf` 配置的其他网络设置。

|选项 |描述 |
|--- |--- |
| network.disablesssd | 禁用查询 SSSD 来获取 AD 帐户信息，默认为 LDAP 调用。 值可以为 `true` 或 `false`。 |
| network.enablekdcfromkrb5conf | 允许从 krb5.conf 查找 KDC 信息。 值可以为 `true` 或 `false`。 |
| network.forcesecureldap | 强制使用 LDAPS 联系域控制器。 值可以为 `true` 或 `false`。 |
| network.ipaddress | 传入连接的 IP 地址。 |
| network.kerberoscredupdatefrequency | 检查需要更新的 kerberos 凭据之间的时间（以秒为单位）。 值是一个整数。|
| network.privilegedadaccount | 要用于 AD 身份验证的特权 AD 用户。 值为 `<username>`。 有关详细信息，请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md#spn)|
| uncmapping | 将 UNC 路径映射到本地路径。 例如，`sudo /opt/mssql/bin/mssql-conf set uncmapping //servername/sharename /tmp/folder`。 |

## <a name="enabledisable-traceflags"></a><a id="traceflags"></a> 启用/禁用跟踪标志

使用 traceflag 选项可启用或禁用 SQL Server 服务启动的跟踪标志。 若要启用/禁用跟踪标志，请使用以下命令：

1. 使用以下命令启用跟踪标志。 例如，对于跟踪标志 1234：

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. 可以通过单独指定跟踪标志来启用多个跟踪标志：

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. 同样，可以通过指定跟踪标志并添加“off”参数来禁用一个或多个启用的跟踪标志：

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. 重启 SQL Server 服务以应用更改：

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>删除设置

要取消设置使用 `mssql-conf set` 进行的任何设置，请使用 `unset` 选项和设置名称调用 mssql-conf。 这将清除设置，有效地将其重置为默认值。

1. 以下示例清除 network.tcpport 选项。

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. 重启 SQL Server 服务。

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>查看当前设置

要查看任何已配置的设置，请运行以下命令以输出 mssql.conf 文件的内容：

```bash
sudo cat /var/opt/mssql/mssql.conf
```

此文件中未显示的所有设置均使用其默认值。 下一部分提供示例 mssql.conf 文件。


## <a name="mssqlconf-format"></a><a id="mssql-conf-format"></a> mssql.conf format

以下 /var/opt/mssql/mssql.conf 文件提供了每个设置的示例。 可以根据需要使用此格式手动更改 mssql.conf 文件。 如果手动更改文件，则必须在应用更改之前重启 SQL Server。 要将 mssql.conf 文件与 Docker 配合使用，必须让 Docker [保留你的数据](./sql-server-linux-docker-container-deployment.md)。 首先将完整的 mssql.conf 文件添加到主机目录，然后运行容器。 [客户反馈](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md)中有一个示例。

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

```ini
[EULA]
accepteula = Y
accepteulaml = Y

[coredump]
captureminiandfull = true
coredumptype = full

[distributedtransaction]
servertcpport = 51999

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
rpcport = 13500
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end

## <a name="next-steps"></a>后续步骤

若要改用环境变量更改其中的某些配置，请参阅[使用环境变量配置 SQL Server 设置](sql-server-linux-configure-environment-variables.md)。

有关其他管理工具和方案，请参阅[管理 Linux 上的 SQL Server](sql-server-linux-management-overview.md)。