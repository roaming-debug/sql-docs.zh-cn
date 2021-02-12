---
title: SSMA for DB2 (DB2ToSQL) 的新增功能 |Microsoft Docs
description: 了解有关每个版本的 DB2 (DB2ToSQL) SQL Server 迁移助手 (SSMA) 的更改。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1cc38f85-3caa-42d0-8c76-a380c1d15c67
ms.author: alexiva
ms.openlocfilehash: a047a1528bafe0f04fca72661d4e32cc8b15fc89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069292"
---
# <a name="whats-new-in-ssma-for-db2-db2tosql"></a>DB2 (DB2ToSQL 的 SSMA 中的新增功能) 

本文列出了每个版本中的 DB2 更改 SQL Server 迁移助手 (SSMA) 。

## <a name="ssma-v816"></a>SSMA v 8.16

用于 DB2 的 SSMA 的8.16 版本包含以下更改：

* 修复列别名和特殊字符的转换
* 更正子句的转换 `SELECTIVITY`
* 改善子句的转换 `WITH ROW MOVEMENT`
* 删除对旧分析器的支持
* 修复了对象未从数据库刷新的问题

## <a name="ssma-v815"></a>SSMA v 8.15

除了多项辅助功能改进以外，用于 DB2 的 SSMA 的8.15 版本包含以下更改：

* 纠正 `MIN` / `MAX` 聚合函数与日期/时间参数的转换
* `VARCHAR_FORMAT`使用占位符时修复模拟函数中的 bug `DD`
* 改善数据类型的类型映射 `TIME`
* `ROUND` `TRUNC` 通过数值自变量改进和函数的转换
* 在新式浏览器中使用的说服评估报表
* 使用数据库提供的授权进行 Azure AD 身份验证
* 改善从文件中加载的语句的命名

## <a name="ssma-v814"></a>SSMA v 8.14

除了多项改进以确保为残障人士提供更好的可访问性之外，8.14 的 SSMA for DB2 的 v 版升级需要项目升级，因为它现在将完整的源/目标服务器版本存储在项目元数据中。

## <a name="ssma-v813"></a>SSMA v 8.13

用于 DB2 的 SSMA 的8.13 版本包含以下更改：

* 支持筛选的唯一索引
* 转换过程和函数调用时，请考虑隐式类型强制转换
* 改进源连接字符串的日志记录，以帮助解决连接问题

## <a name="ssma-v812"></a>SSMA v 8.12

用于 DB2 的 SSMA 的8.12 版本包含以下更改：

* 函数的转换 `STRIP`
* 改进了过程选项分析

## <a name="ssma-v811"></a>SSMA v 8.11

用于 DB2 的 SSMA 的8.11 版本包含以下更改：

* 支持 DB2 for i (7.1 及更高版本) 
* 转换 `SQLSTATE` 和 `SQLCODE`
* 函数内带副作用的运算符的转换错误消息
* 使用 MSAL.NET 库进行交互式 Azure Active Directory 身份验证

## <a name="ssma-v810"></a>SSMA 8.10

SSMA for DB2 的 v2.0 版本解决了外键发现中的回归，并包含了极小的性能改进。

## <a name="ssma-v89"></a>SSMA v 8。9

用于 DB2 的 SSMA 的 v 8.9 版本包含以下更改：

* 解决函数的转换问题 `TIMESTAMPDIFF`
* 在存在分区索引时修复索引发现
* 在其他架构中定义主索引时，解决外键发现问题
* 改善了与内置函数名称匹配的列的转换
* 解决项目名称中含有特殊字符的问题

## <a name="ssma-v88"></a>SSMA v 8。8

适用于 DB2 的 SSMA 的 8.8 8.8 版本包括：

* SQL Server 对象同步稳定性改进
* 评估和转换期间的 GUI 性能改进
* 更新了从到的映射以便 `ROWID` `varbinary(40)` 于数据迁移
* 改进了语句的转换 `SELECT ... FROM NEW/OLD TABLE`
* `ALTER`过程和函数的语句的新转换
* 析构分配的新转换

## <a name="ssma-v87"></a>SSMA v 8。7

SSMA for DB2 的 v4.0 版本包括全新的 DB2 语法分析器，以及图形用户界面中的小修补和性能改进。

此外，SSMA for DB2 现在提供：

* 在 LUW 上从 DB2 进行迁移时，对外键的发现进行修复。
* 改进了语句的转换 `SELECT ... FOR UPDATE` 。
* 改进了 `COUNT` MQ 表中函数的转换。
* 语句的转换 `SAVEPOINT` 。
* 转换用于模拟 `NULL` 子句中值的 DB2's 行为 `ORDER BY` 。
* 分析语句支持 `ASSOCIATE RESULT SET` 。

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v86"></a>SSMA v 8。6

除了旨在提高可用性和性能的目标修补集外，通过添加使用户能够在转换后的代码中省略 SSMA 扩展属性的设置，还增强了 SSMA for DB2 的 v 8.6 版本。

若要利用此设置，请在 SSMA for DB2 中导航到 "**工具**" "  >  **项目设置**" "  >  **常规**  >  **转换**"，然后在 "**杂项**" 下，将 "**省略扩展属性**" 设置的值更新为 **"是"**。

![省略扩展属性设置](../db2/media/ssma-omit-extended-properties.png)

此外，SSMA for DB2 现在提供：

* 用于转换使用默认参数值的函数的修补程序。
* 改进了函数的 `PARAMETER` 子句分析。
* 转换语句的能力 `LEAVE` 。

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v85"></a>SSMA 8。5

通过对 SQL server 中的 JSON 功能 Azure Active Directory authentication 和 basic 支持以及旨在提高可用性和性能的目标修补程序集，增强了用于 DB2 的 SSMA 的版本8.5。

此外，SSMA for DB2 已通过以下方式增强：

* 支持添加用于语句的转换 `GET DIAGNOSTICS` `ROW_NUMBER` 。
* 修复了与对象名称开头处空格相关的错误。

> [!IMPORTANT]
> 对于 SSMA 的8.5，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v84"></a>SSMA v2。0

SSMA for DB2 的 v2.0 版本使用旨在解决辅助功能问题的目标修补程序进行了增强，并修复了与最大索引列相关的 bug (允许32而不是 16) 用于 SQL Server 2016 及更高版本。

> [!IMPORTANT]
> 对于 SSMA 版本7.4，8.4，.NET 4.5.2 是必备组件。

## <a name="ssma-v83"></a>SSMA v 8。3

SSMA for DB2 的 v 8.3 版本通过旨在改进质量和转换指标的目标修补程序进行了增强。 此外，此版本的 SSMA for DB2 还提供了以下修补程序：

* 解决辅助功能问题。
* 为 SQL Server 中的类型添加基本支持 `hierarchyid` 。
* 将 z/OS 发现查询中的 TRIM 函数用法替换为 `RTRIM` / `LTRIM` 。
* 允许用户在 "标准模式" 下连接时指定包集合 (默认为 `NULLID`) 。
* 添加的转换 `CREATE TABLE AS SELECT` 。
* 改进全局临时表的转换。
* 解决了对象唯一检查顺序的问题，以便在名称发生冲突的情况下为表设置优先级。
* 解决为 `DATE` z/OS 加载和的默认列值的问题 `TIMESTAMP` 。
* 支持 Unicode 换行符 (也称为 `NEL`) 。
* 解决带有缺少子句的游标转换问题 `RETURN TO` 。
* 添加对标签和的支持 `GOTO` 。

## <a name="ssma-v82"></a>SSMA

的 SSMA for DB2 的7.4 版已通过 SSMA 控制台工具解决了与 Azure SQL 数据库连接有关的问题，并且在转换过程中缺少视图声明中的 COUNT_BIG 列。 此外，此版本还包括一组旨在提高质量和转换指标的目标修补程序，以及对的修复：

* 数据迁移后禁用的非聚集索引的问题。
* 在无提示安装期间检测 .NET Framework。
* 下载新版本时出现间歇性崩溃。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA v4.0 到 v4.0 8.2 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v81"></a>SSMA 8。1

SSMA for DB2 的 "7.4 版" 版本得到了增强，可提供旨在提高质量和转换指标的目标修补程序。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA 8.0 到 app-v 8.1 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v80"></a>SSMA 8。0

SSMA for DB2 的 v2.0 版本得到了增强，可提供旨在提高质量和转换指标的目标修补程序。 此版本还提供以下新增功能：

* 支持作为目标的 **AZURE SQL 托管实例** 。 你现在可以创建面向 Azure SQL 托管实例的新项目：

  ![SQL MI 项目](../media/ssma-newproject-sqldbmi.png)

* 转换后 **修补顾问**。 [在此处](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/)了解详细信息。

* 初步的数据库/架构选择。

  连接到源时，用户现在可以选择数据库/感兴趣的架构。 仅选择你计划迁移的架构将在初始连接期间节省时间，并提高总体 SSMA 性能。

  ![SSMA 筛选器对象](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA

用于 DB2 的 SSMA 的版本7.10 版本包含以下更改：

* 旨在提供附加安全和隐私保护以满足全局要求更改的目标修补程序。
* 用于转换块的修补程序 `BEGIN-END` 。

## <a name="ssma-v79"></a>SSMA v 7。9

适用于 DB2 的 SSMA 的 v 7.9 版本包含以下更改：

* 改进质量和转换指标的目标修补程序。
* 支持在 SSMA 命令行中更改数据类型映射和项目首选项。
* 支持使用 SQL Server Integration Services (SSIS) 迁移数据。 转换架构后，可以使用右键单击上下文菜单选项创建 SSIS 包。
* 还更改了 SSMA 中的 Azure SQL 数据库连接对话框以指定完全限定的服务器名称。 在以前版本的 SSMA 中，必须在项目设置中显式提到 Azure SQL 数据库前缀。

## <a name="ssma-v78"></a>SSMA v 7。8

适用于 DB2 的 SSMA 的版本7.8 版本包含以下更改：

* 更改 *项目设置* 中突出显示的类型映射。
* 允许用户禁用遥测数据。

## <a name="ssma-v77"></a>SSMA v4。0

用于 DB2 的 SSMA 的 v4.0 版本包含以下更改：

* 改进质量和转换指标的目标修补程序。
* 基于常用需求，SSMA for DB2 的32位版本已恢复。 与) v 7.4 之前的实现 (相比，有两个安装包，但不能并行安装。 因此，你必须根据你拥有的连接组件选择最适合的版本。 如果可能，始终最好使用64位版本。

## <a name="ssma-v76"></a>SSMA v 7。6

SSMA for DB2 的版本7.6 版本通过目标修补程序增强，这些修补程序可提高质量和转换指标，并支持 SQL Server 2017 (公共预览) 。 对 Windows 和 Linux 上的 SQL Server 2017 的支持是公共预览版，不应用于生产迁移。

## <a name="ssma-v75"></a>SSMA 7。5

SSMA for DB2 的版本7.5 版本增强了几项改进，以确保为残障人士提供更好的辅助功能。

## <a name="ssma-v74"></a>SSMA 7。4

用于 DB2 的 SSMA 的7.4 版本包含以下更改：

* 在源和目标中的架构对象发现期间，现在可以使用 " **查询超时** " 选项。

  ![query timeout 选项](../media/query-timeout_red.png)

* 根据客户的反馈，改进了质量和转换指标。

  > [!IMPORTANT]
  > .NET 4.5.2 是安装 SSMA 7.4 的必备组件。 此外，从7.4 版开始，32位版本的 SSMA 已不再使用。

## <a name="ssma-v73"></a>SSMA 7。3

适用于 DB2 的 SSMA 的7.3 版本包含以下更改：

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

SSMA for DB2 的 v2.0 版本包含以下更改：

* 根据客户的反馈，改进了质量和转换指标与目标修复。
* 遥测增强功能可提供更好的数据点以解决客户问题并提高 SSMA 的转换率。

## <a name="ssma-v71"></a>SSMA 7。1

适用于 DB2 的 SSMA 7.1 版本包含以下更改：

* Windows 和 Linux 上的 SQL Server 2017 现在是支持迁移的目标平台。 此功能在 technical preview 中，允许将架构和数据移动到目标 SQL server。
* 支持自动更新，以下载最新版本的 SSMA （如果可用）。
* SSMA 可安装二进制文件现在通过 Windows Installer 包文件 ( .msi) 提供。

## <a name="may-2016"></a>2016 年 5 月

DB2 的 SSMA 2016 版包含以下更改：

* 添加了对 SQL Server 2016 的支持。
* 添加了 DB2 内存中和常规表的转换，以便 SQL Server 内存中和 hekaton 的功能。
* 添加了 DB2 访问控制的转换，以便为 DB2)  (行级别安全性 SQL Server 策略对象。
* 添加了 DB2 系统版本控制表到 SQL Server 临时表的转换。
* 改进了 DB2 分析器和解析程序。
* 移除了 .NET 2.0 的安装程序检查。
* 从 Db2 安装程序删除了不需要的 \* .dll。
* 修复 `save-project` `open-project` 了和用于 SSMA 控制台的命令。
* 修复 `securepassword` 了 SSMA 控制台命令。
* 修复了初始加载的对象计数。
* 修复了全局设置中的 bug。
  
## <a name="march-2016"></a>2016 年 3 月

SSMA 的2016年3月预览版本增加了对迁移到 SQL Server 2016 的支持。

## <a name="january-2016"></a>2016 年 1 月

SSMA 的2016年1月维护版本包含以下更改：
  
* 添加了对许多标准函数的支持。
* 修复了 DB2 分析器错误。
* 修复了 DB2 v9.x zOS (RFC 5690920) 支持。
* 修复了转换期间的 DB2 未解决标识符错误。
* 已将 "查看日志" 菜单项添加到 SSMA (RFC 5706203) 。
* 添加了遥测。
  
## <a name="november-2014"></a>2014 年 11 月

用于 DB2 的 SSMA 11 月2014版是初始版本。
