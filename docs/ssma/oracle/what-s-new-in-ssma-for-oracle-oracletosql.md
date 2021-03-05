---
title: SSMA for Oracle (OracleToSQL) 的新增功能 |Microsoft Docs
description: 了解针对每个版本的 Oracle (OracleToSQL) SQL Server 迁移助手 (SSMA) 的更改。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f305ebb6-7393-4a43-abb3-6332b739d690
ms.author: alexiva
ms.openlocfilehash: ef5f8fd634958c75cc67002615e3c4a72dcf3a51
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186558"
---
# <a name="whats-new-in-ssma-for-oracle-oracletosql"></a>Oracle (OracleToSQL 的 SSMA 中的新增功能) 

本文列出了每个版本的 Oracle 更改 SQL Server 迁移助手 (SSMA) 。

## <a name="ssma-v817"></a>SSMA v 8.17

用于 Oracle 的 SSMA 的8.17 版本包含以下更改：

* 添加对计算列的支持
* 解决在表达式中使用区域性特定运算符字符时的分析器问题
* 更新 HTML 评估报表以使用新式编辑器显示 SQL 文本

## <a name="ssma-v816"></a>SSMA v 8.16

用于 Oracle 的 SSMA 的8.16 版本包含以下更改：

* 添加对标识列的支持
* 删除对旧分析器的支持
* 修复了对象未从数据库刷新的问题

## <a name="ssma-v815"></a>SSMA v 8.15

除了多项辅助功能改进之外，8.15 的 SSMA for Oracle 的 v 版包含以下更改：

* 添加设置以对 `%type` 和属性使用完全类型 `%rowtype` 规范
* 在新式浏览器中使用的说服评估报表
* 使用数据库提供的授权进行 Azure AD 身份验证
* 改善从文件中加载的语句的命名

## <a name="ssma-v814"></a>SSMA v 8.14

除了多项改进以确保为残障人士提供更好的辅助功能，8.14 的 SSMA for Oracle 的 v 版包含以下更改：

* 将完整的源/目标服务器版本存储到项目元数据中 (需要项目升级) 
* 尽可能使用 DBA 数据字典进行对象发现
* 解决了多个分析程序问题 (`PIVOT` / `UNPIVOT` 、 `MERGE` 备选报价) 
* 修复 `INSERTING` / `DELETING` / `UPDATING` 触发器中特殊函数的转换

## <a name="ssma-v813"></a>SSMA v 8.13

用于 Oracle 的 SSMA 的8.13 版本包含以下更改：

* 修复 `SQLCODE` `SQLERRM` 本地过程中和特殊函数的转换
* 转换过程和函数调用时，请考虑隐式类型强制转换
* 改进源连接字符串的日志记录，以帮助解决连接问题

## <a name="ssma-v812"></a>SSMA v 8.12

用于 Oracle 的 SSMA 的8.12 版本包含以下更改：

* 支持 `INSERT` / `UPDATE` / `MERGE` / `DELETE` 语句中的子查询因式分解子句
* `ON DELETE SET NULL`多路径或循环引用的情况下的转换消息 for 子句
* 改进了从动态 SQL 字符串创建的游标的转换
* 将 ODP.NET 更新为 v 19。8

## <a name="ssma-v811"></a>SSMA v 8.11

用于 Oracle 的 SSMA 的8.11 版本包含以下更改：

* 支持语句中的子查询 `INSERT ... VALUES`
* 改进了语句的转换 `COMMIT`
* 修复 bug 的 `CONNECT BY LEVEL` 转换
* 已更新分析器错误恢复逻辑，使其不变
* 使用 MSAL.NET 库进行交互式 Azure Active Directory 身份验证

## <a name="ssma-v810"></a>SSMA 8.10

SSMA for Oracle 的 v2.0 版本包含少许性能改进以及以下更改：

* 修复了具有索引的表的测试人员问题
* 为扩展包中的扩展存储过程的名称修复

## <a name="ssma-v89"></a>SSMA v 8。9

SSMA for Oracle 的6.x 版包含以下更改：

* 动态 SQL 字符串文本的转换
* 转换 `LAG` `FIRST_VALUE` 和 `LAST_VALUE` 分析函数
* 添加对基本 `ALTER TRIGGER` / `ALTER INDEX` DDL (启用/禁用等支持 ) 
* 改善了与内置函数名称匹配的列的转换
* 为可执行筛选的列生成筛选的唯一索引 `NULL`
* Azure Synapse Analytics 的可变声明转换改进
* 解决项目名称中含有特殊字符的问题

## <a name="ssma-v88"></a>SSMA v 8。8

SSMA for Oracle 的 v2.0 版本包括：

* SQL Server 对象同步稳定性改进
* 评估和转换期间的 GUI 性能改进
* 提高了分析子句的转换 `OVER PARTITION`
* 分析函数的新转换 `LEAD`
* 子查询因式分解子句的新转换
* `REPLICATE`Azure Synapse Analytics 的新分发选项
* 全新的 Oracle 语法分析器，进一步提高转换性能

## <a name="ssma-v87"></a>SSMA v 8。7

SSMA for Oracle 的 v4.0 版本在图形用户界面中提供了细微的修复和性能改进。

此外，SSMA for Oracle 现在允许基于 "高级对象选择" 对话框中的有效性状态筛选对象。

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v86"></a>SSMA v 8。6

除了旨在提高可用性和性能的目标修补集外，通过添加使用户能够在转换后的代码中省略 SSMA 扩展属性的设置，还增强了 SSMA for Oracle 的 v 8.6 版本。

若要利用此设置，请在 SSMA for Oracle 中导航到 "**工具**" "  >  **项目设置**" "  >  **常规**  >  **转换**"，然后在 "**杂项**" 下，将 "**省略扩展属性**" 设置的值更新为 **"是"**。

![省略扩展属性设置](../oracle/media/ssma-omit-extended-properties.png)

此外，SSMA for Oracle 现在提供改进的 `XMLTABLE` 子句分析。

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v85"></a>SSMA 8。5

通过对 SQL server 中的 JSON 功能 Azure Active Directory authentication 和 basic 支持以及旨在提高可用性和性能的目标修补程序集，增强了用于 Oracle 的 SSMA for Oracle 的版本8.5 版本。

此外，SSMA for Oracle 已通过以下支持得到增强：

* 将所选对象数限制为 990 (Oracle `WHERE .. IN (..)` 子句限制为1000个项目) 。
* 将数据从迁移 `RAW` 到 `UNIQUEIDENTIFIER` 。
* 子句分析 `PARALLEL_ENABLE` 。

最后，适用于 Oracle 的 SSMA 的版本8.5 现在提供：

* 提高了转换后的打包常量的性能。
* 适用于 .NET 的 Oracle 数据提供程序到版本19.5.0 的更新。

> [!IMPORTANT]
> 对于 SSMA 的8.5，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v84"></a>SSMA v2。0

SSMA for Oracle 的 v2.0 版本使用旨在解决辅助功能问题的目标修补程序进行了增强，并修复了与最大索引列相关的 bug (允许32而不是 16) 用于 SQL Server 2016 及更高版本。

此外，此版本的 SSMA for Oracle 添加了 `SYS_REFCURSOR` 作为存储过程参数的转换 `OUT` 。

> [!IMPORTANT]
> 使用 SSMA 版本7.4 到8.4，.NET 4.5.2 是必备组件。

## <a name="ssma-v83"></a>SSMA v 8。3

SSMA for Oracle 的 v 8.3 版本通过旨在改进质量和转换指标的目标修补程序进行了增强。 此外，此版本的 SSMA for Oracle 还提供了以下修补程序：

* 解决辅助功能问题。
* 为 SQL Server 中的类型添加基本支持 `hierarchyid` 。
* 解决通过同义词调用的函数的返回类型未知的问题。
* 将 ODP.NET 更新为19.3。

## <a name="ssma-v82"></a>SSMA

适用于 Oracle 的 SSMA 的版本8.2 版本增强为：

* 添加对的支持 `DBMS_OUTPUT.ENABLE` / `DISABLE` 。
* 删除 `CAST AS FLOAT` `BINARY_FLOAT` `BINARY_DOUBLE` 默认数据迁移查询中的和列。
* 如果当前值已更改，则修复序列刷新。
* 修复与伪列 (等相关错误相关的 bug `ROWNUM` ，如 ) 是否存在具有相同名称的列。
* 修复在转换循环时出现不明确的可 `FOR` 解析标识符的崩溃。

此外，此版本还包括一组旨在提高质量和转换指标的目标修补程序，以及对的修复：

* 数据迁移后禁用的非聚集索引的问题。
* 在无提示安装期间检测 .NET Framework。
* 下载新版本时出现间歇性崩溃。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA v4.0 到 v4.0 8.2 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v81"></a>SSMA 8。1

适用于 Oracle 的 SSMA 的7.4 版已通过旨在提高质量和转换指标的目标修补程序进行了增强。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA 8.0 到 app-v 8.1 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v80"></a>SSMA 8。0

SSMA for Oracle 的 v2.0 版本通过旨在改进质量和转换指标的目标修补程序进行了增强。 此版本还提供以下新增功能：

* 支持作为目标的 **AZURE SQL 托管实例** 。 你现在可以创建面向 Azure SQL 托管实例的新项目：

  ![SQL MI 项目](../media/ssma-newproject-sqldbmi.png)

  > [!NOTE]
  > 还更新了 SSMA for Oracle 扩展包，以允许在 Azure SQL 托管实例上进行远程安装：
  >
  > ![Oracle 扩展包的 SSMA](../media/ssma-oracle-ext-pack.png)

  面向 Azure SQL 托管实例时，某些功能（包括测试人员和服务器端数据迁移）不受支持。 在[此处](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/migrate-your-oracle-database-to-azure-sql-database-managed-instance-using-ssma-8-0/)阅读详细信息。

* 转换后 **修补顾问**。 [在此处](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/)了解详细信息。

* 初步的数据库/架构选择。

  连接到源时，用户现在可以选择数据库/感兴趣的架构。 仅选择你计划迁移的架构将在初始连接期间节省时间，并提高总体 SSMA 性能。

  ![SSMA 筛选器对象](../media/ssma-filter-objects.png)

* 能够使用官方的托管 .NET 驱动程序连接到 Oracle。 OCI 驱动程序不再是使用适用于 Oracle 的 SQL Server 迁移助手的先决条件。

* `ROWID` `UROWID` 默认情况下，映射和的功能 `VARCHAR` 。 从更改 `uniqueidentifier` 为适用于显式列的数据迁移 `ROWID` 。

## <a name="ssma-v710"></a>SSMA

适用于 Oracle 的 SSMA 的版本7.10 版本包含以下更改：

* 旨在提供附加安全和隐私保护以满足全局要求更改的目标修补程序。
* 与分层查询相关的转换改进。

## <a name="ssma-v79"></a>SSMA v 7。9

SSMA for Oracle 的 v 7.9 版本包含以下更改：

* 改进质量和转换指标的目标修补程序。
* 支持将 "Continue" 语句从 Oracle 迁移到 SQL Server。
* 支持在 SSMA 命令行中更改数据类型映射和项目首选项。
* 支持使用 SQL Server Integration Services (SSIS) 迁移数据。 转换架构后，可以使用右键单击上下文菜单选项创建 SSIS 包。
* 还更改了 SSMA 中的 Azure SQL 数据库连接对话框以指定完全限定的服务器名称。 在以前版本的 SSMA 中，必须在项目设置中显式提到 Azure SQL 数据库前缀。

## <a name="ssma-v78"></a>SSMA v 7。8

SSMA for Oracle 的7.4 版版本包含以下更改：

* 支持：
  * 子句的行表达式 `IN` 。
  * 隐式类型转换。
  * `UID` Azure SQL 数据库的转换。
* 更改 **项目设置** 中突出显示的类型映射。
* 允许用户禁用遥测数据。

## <a name="ssma-v77"></a>SSMA v4。0

SSMA for Oracle 的 v4.0 版本包含以下更改：

* SSMA for Oracle 已通过可提高质量和转换指标的目标修补程序进行了增强。
* 基于热门需求，SSMA for Oracle 的32位版本将恢复。 与上一次实现)  (v 7.4 之前的实现相比，有两个安装包，但不能并行安装。 因此，你必须根据你拥有的连接组件选择最适合的版本。 如果可能，始终最好使用64位版本。
* SQL Server 2017 支持现已正式拥有 Linux 上支持的 Oracle 扩展包 (新的远程安装选项) 。 请注意，在 Linux 上安装时，扩展包功能是有限的，因为不支持测试人员和服务器端数据迁移功能。
* SSMA for Oracle 允许您将具体化视图作为常规表迁移， (可通过 **项目设置**  ->  **同步**  ->  **发现) 的具体化视图的支持表** 中的设置进行配置。

## <a name="ssma-v76"></a>SSMA v 7。6

SSMA for Oracle 的版本7.6 版本通过目标修补程序进行了增强，这些修补程序可提高质量和转换指标，并支持 (公共预览) SQL Server 2017。 对 Windows 和 Linux 上的 SQL Server 2017 的支持是公共预览版，不应用于生产迁移。

## <a name="ssma-v75"></a>SSMA 7。5

适用于 Oracle 的 SSMA 的7.5 版本包含以下更改：

* 增强了几项改进功能，确保为残障人士提供更好的辅助功能。
* 经过更新，以通过目标修补程序提高质量和转换指标，例如，在数据迁移过程中改进了日期和 float 数据类型的处理，这是基于客户的反馈。

## <a name="ssma-v74"></a>SSMA 7。4

SSMA for Oracle 的7.4 版包含以下更改：

* SSMA for Oracle 现在支持将 Azure Synapse 分析作为迁移的目标平台。

  ![“新建项目”窗口](../media/new-project.png)
  * 支持数据仓库存储选项，如下图所示：

  ![用于数据仓库的存储选项](../media/storage-options_red.png)
  * 支持数据分发选项，如下图所示：

  ![数据仓库的数据分布](../media/data-distribution_red.png)

* 在源和目标中的架构对象发现期间，现在可以使用 " **查询超时** " 选项。

  ![query timeout 选项](../media/query-timeout_red.png)

* 根据客户的反馈，改进了质量和转换指标。

> [!IMPORTANT]
> .NET 4.5.2 是安装 SSMA 7.4 的必备组件。 此外，从7.4 版开始，32位版本的 SSMA 即将停止使用。

## <a name="ssma-v73"></a>SSMA 7。3

SSMA for Oracle 的7.3 版包含以下更改：

* 根据客户的反馈，改进了质量和转换指标与目标修复。
* 通过以下各项公开的 SSMA 扩展性框架：
  * 将功能导出到 SQL Server Data Tools (SSDT) 项目。
    * 你现在可以将 SSMA 中的架构脚本导出到 SSDT 项目。 您可以使用架构脚本来进行其他架构更改并部署您的数据库。

      ![另存为 SSDT 项目命令](../media/export-schema-scripts_red.png)
  * 可供 SSMA 使用的库，用于执行自定义转换。
    * 你现在可以构造代码，该代码可以处理 SSMA 之前未处理的自定义语法转换和转换。
      * 此博客文章中提供了有关如何构造自定义转换器的说明， [扩展了 SQL Server 迁移助手的转换功能](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)。
      * 下载此 [博客文章](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/)中用于转换的示例项目。

## <a name="ssma-v72"></a>SSMA 7。2

SSMA for Oracle 的 v2.0 版本包含以下更改：

* 根据客户的反馈，改进了质量和转换指标与目标修复。
* 遥测增强功能可提供更好的数据点以解决客户问题并提高 SSMA 的转换率。

## <a name="ssma-v71"></a>SSMA 7。1

SSMA for Oracle 的 v2.0 版本包含以下更改：

* Windows 和 Linux 上的 SQL Server 2017 现在是支持迁移的目标平台。 此功能在 technical preview 中，允许将架构和数据移动到目标 SQL server。
* SSMA 现在支持自动更新，以下载最新版本的 SSMA。
* SSMA 可安装二进制文件现在通过 Windows Installer 包文件 ( .msi) 提供。

## <a name="may-2016"></a>2016 年 5 月

SSMA for Oracle 的5月2016版包含以下更改：

* 添加了对 SQL Server 2016 的支持。
* 添加了 Oracle 闪回存档表到 SQL Server 临时表的转换。

  > [!NOTE]
  > SSMA 不复制 Oracle 闪回数据存档表中的历史记录数据。 因此，在迁移过程中必须手动复制历史记录数据。 此外，SSMA 不会在 SQL Server 元数据资源管理器中显示历史记录表，因为它被视为系统表，你可以在 SQL Server Management Studio 中查看历史记录表。
  >
  > SQL Server 2016 不支持多个 Oracle 闪回功能，其中包括：
  >
  >   * Oracle 闪回事务查询
  >   * `DBMS_FLASHBACK` “包”
  >   * 闪回事务
  >   * 闪回数据存档
  >   * 闪回表
  >   * 闪回
  >   * 闪回数据库

* 将 Oracle VPD 策略的转换添加到了 SQL Server 策略对象 (Oracle) 行级别安全性。
* 缩短了 Oracle 初始加载的时间。
* 改进的分析器和解析程序。
* 移除了 .NET 2.0 的安装程序检查。
* 更新了从 .NET 3.5 到 .NET 4.0 的扩展包。
* 修复 `save-project` `open-project` 了和用于 SSMA 控制台的命令。
* 修复 `securepassword` 了 SSMA 控制台命令。
* 修复了初始加载的对象计数。
* 修复了 Oracle 的字符数据类型的转换。
* 修复了全局设置中的 bug。

## <a name="march-2016"></a>2016 年 3 月

SSMA for Oracle 2016 年3月预览版本添加了对的支持：

* 迁移到 SQL Server 2016。
* 将 Oracle 行级别安全性 (迁移) 存在一些限制。
* 将 Oracle in memory 表迁移到 SQL Server 列存储。

## <a name="january-2016"></a>2016 年 1 月

SSMA for Oracle 的2014年1月维护版本包含以下更改：

* 添加了对聚集索引的支持。
* 修复了 (RFC 5076207) 的缓慢 Oracle 架构查询。
* 修复了从控制台连接到 Azure。
* 已将 "查看日志" 菜单项添加到 SSMA (RFC 5706203) 。
* 添加了遥测。

## <a name="july-2014"></a>2014 年 7 月

SSMA for Oracle 的2014年7月发行版本包含以下更改：

* 添加了对 Azure SQL 数据库的支持。
* 扩展包功能已移动到架构以支持 Azure SQL 数据库。
* 添加了对 Oracle 具体化视图的支持。
* 添加了对 SQL Server 2014 内存优化表的支持。
* 为具有超过10k 对象的数据库测试了性能改进。
* 添加了用于处理大量对象的 UI 改进。
* 添加了众所周知的 LOB 架构的突出显示。
* 包含转换速度改进。
* 添加了对在 UI 中显示对象计数的支持。
* 将报表大小减少25% 以上。
* 改进了未分析构造的错误消息。

## <a name="april-2014"></a>2014 年 4 月

SSMA for Oracle 的2014年4月版包含以下更改：

* 添加了对 MS SQL Server 2014 的支持。
* 添加了对 Oracle 12 和查询优化的支持。
* 修复了有关转换到 Azure 的 bug。
* 修复了 IE 10 中不可见报表页的相关 bug。

## <a name="january-2012"></a>2012 年 1 月

SSMA for Oracle 的2012年1月版增加了对 `RowType` 和 `RecordType` 默认为的输入参数的支持 `NULL` 。

## <a name="july-2011"></a>2011 年 7 月

SSMA for Oracle 的2011年7月发行版本包含以下更改：

* 添加了对将 Oracle 序列转换为 [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] 序列生成器的支持。
* 在数据迁移过程中改进了错误报告。
* 使用保留字改进了语句的转换。
* 改进了函数中 date 值的隐式转换。

## <a name="april-2011"></a>2011 年 4 月

SSMA for Oracle 的2011年4月版包含以下更改：

* 合并的 "SSMA for Oracle" 产品，它支持 [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] 、 [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] 和 [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] 。
* 添加了对连接和迁移到的支持 [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] 。
* 增强了客户端数据迁移引擎，支持并行数据迁移。
* 提高了 `Simple` `Bulk` 恢复模式和日志记录的数据迁移性能。
* 添加了对早期版本的 SSMA (v4.0 和 4.2) 创建的项目的向后兼容性的支持。
* 添加了一种功能，用于安装 SSMA for Oracle (SxS) ，以及 SSMA (v4.0 和4.2 版) 的早期版本。
* 添加了对报表 User-Defined 类型的支持 (包括子类型、 `VARRAY` 、 `NESTED TABLE` 、对象表和对象视图) 及其在 PL/SQL 块中的用法以及特殊错误消息。

## <a name="july-2010"></a>2010 年 7 月

添加的 SSMA for Oracle 2010 年7月发行版：

* 支持迁移到 SQL Server 2008 R2。
* 用于执行命令行的新 SSMA 控制台应用程序。
* 支持使用 Server-Side 和 Client-Side 数据迁移引擎进行数据迁移。
* 支持数据迁移中的 "自定义" SELECT 语句。
* 支持从 Oracle 11g R2 迁移。

## <a name="june-2008"></a>2008年6月

SSMA for Oracle 的2008年6月发行版包含以下更改：

* 增加了对评估报表的改进，包括同义词的其他信息、可分析对象的原始源、面板和 SQL Server 徽标删除以及布局持久性。
* 增加了对象转换的改进：
  * `DBMS_LOB` `DBMS_SQL` 已添加包，转换。
  * 联接转换已修订。
  * 修改集合和记录转换后，就是通过每个字段的单独变量释放的简单事例中的记录转换。
  * 改进了记录和集合实现。
  * 添加了窗口化聚合函数。
  * `ROLLUP`/`CUBE` 添加了子句。
  * 改进 `NEXTVAL` / `CURVAL` 。
  * 添加了列分组 in `SET` 子句、分组集和分组 ID。
  * `MERGE` 已添加语句。
  * 添加了新的日期时间类型以及将记录和集合转换为添加的 CLR 数据类型。
* 添加了测试人员的新功能。 现在可以使用测试人员将表测试为对象，可以更改测试用例中多个可测试对象的调用顺序，用户可以使用记录和集合来测试过程和函数作为参数和返回值，并且添加了依赖项分析器以仅检查已使用的表。
  
## <a name="august-2007"></a>2007年8月

添加的 SSMA for Oracle 2007 年8月版已添加：

* 新的测试人员组件允许您创建、管理和运行测试用例，以验证转换后的 SQL 代码。
* 已将 Oracle 子类型、集合和本地模块的转换支持添加到 SQL 转换器。
* 利用新的同步功能，您可以将特定对象与 SQL Server 数据库同步。
* 新的转换选项。
  
## <a name="april-2007"></a>2007年4月

SSMA for Oracle 的2007年4月版是初始版本。
