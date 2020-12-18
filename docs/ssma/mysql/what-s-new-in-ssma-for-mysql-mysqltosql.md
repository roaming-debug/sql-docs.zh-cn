---
title: SSMA for MySQL 中的新增功能 (MySQLToSql) |Microsoft Docs
description: 了解每个版本的 SQL Server 迁移助手 (SSMA) for MySQL (MySQLToSQL) 的更改。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
ms.author: alexiva
ms.openlocfilehash: b97f27f2e6c1fbed9109abbde012d7a8cf97935a
ms.sourcegitcommit: a16b98d3bf3eeb58f5d2aeece2464f8a96e2b4a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2020
ms.locfileid: "97665828"
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>SSMA for MySQL 中的新增功能 (MySQLToSql)

本文列出了每个版本中的 MySQL 更改 SQL Server 迁移助手 (SSMA) 。

## <a name="ssma-v816"></a>SSMA v 8.16

SSMA for MySQL 的8.16 版本包含以下更改：

* 添加对计算列的支持
* 修复 `INSERT` 具有唯一约束和主键的表的转换语句时的问题
* 更新分析器以与 `ANSI_QUOTES` `NO_BACKSLASH_ESCAPES` 服务器模式相关
* 删除对旧分析器的支持
* 修复了对象未从数据库刷新的问题

## <a name="ssma-v815"></a>SSMA v 8.15

除了多项辅助功能改进之外，8.15 版本的 SSMA for MySQL 还包含以下更改：

* 在新式浏览器中使用的说服评估报表
* 使用数据库提供的授权进行 Azure AD 身份验证
* 改善从文件中加载的语句的命名

## <a name="ssma-v814"></a>SSMA v 8.14

除了多项改进以确保为残障人士提供更好的辅助功能，8.14 版本的 SSMA for MySQL 还需要项目升级，因为它现在将完整的源/目标服务器版本存储在项目元数据中。

## <a name="ssma-v813"></a>SSMA v 8.13

SSMA for MySQL 的8.13 版本包含以下更改：

* 转换过程和函数调用时，请考虑隐式类型强制转换
* 改进源连接字符串的日志记录，以帮助解决连接问题

## <a name="ssma-v812"></a>SSMA v 8.12

SSMA for MySQL 的8.12 版本包含以下更改：

* 临时表 DDL 的转换

## <a name="ssma-v811"></a>SSMA v 8.11

SSMA for MySQL 的8.11 版本包含以下更改：

* 使用 MSAL.NET 库进行交互式 Azure Active Directory 身份验证

## <a name="ssma-v810"></a>SSMA 8.10

SSMA for MySQL 的 v2.0 版本包含少许性能改进和 bug 修复。

## <a name="ssma-v89"></a>SSMA v 8。9

SSMA for MySQL 的6.x 版包含以下更改：

* 修复空间类型的数据迁移
* 解决项目名称中含有特殊字符的问题

## <a name="ssma-v88"></a>SSMA v 8。8

SSMA for MySQL 的 v2.0 版本包括：

* SQL Server 对象同步稳定性改进
* 评估和转换期间的 GUI 性能改进

## <a name="ssma-v87"></a>SSMA v 8。7

SSMA for MySQL 的 v4.0 版本在图形用户界面中提供了细微的修复和性能改进。

此外，SSMA for MySQL 现在提供 `LIMIT` 面向 AZURE SQL 时的转换 for 子句。

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v86"></a>SSMA v 8。6

除了旨在提高可用性和性能的目标修补集外，通过添加使用户能够在转换后的代码中省略 SSMA 扩展属性的设置，增强了 SSMA for MySQL 的 v 8.6 版本。

若要利用此设置，请在 SSMA for MySQL 中导航到 "**工具**" "  >  **项目设置**" "  >  **常规**  >  **转换**"，然后在 "**杂项**" 下，将 "**省略扩展属性**" 设置的值更新为 **"是"**。

![省略扩展属性设置](../mysql/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> 对于 SSMA 的8.5 和更高版本，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v85"></a>SSMA 8。5

通过对 SQL server 中的 JSON 功能 Azure Active Directory authentication 和 basic 支持以及旨在提高可用性和性能的目标修补程序集，增强了的 SSMA for MySQL 的版本8.5。

> [!IMPORTANT]
> 对于 SSMA 的8.5，.NET 4.7.2 是必备组件。 如果需要安装此版本，可以从 [此处](https://dotnet.microsoft.com/download/dotnet-framework/net472)下载运行时文件。

## <a name="ssma-v84"></a>SSMA v2。0

SSMA for MySQL 的 v2.0 版本增强了目标修补程序，旨在解决辅助功能问题，并修复了与最大索引列相关的 bug (允许32而不是 16) 用于 SQL Server 2016 及更高版本。

> [!IMPORTANT]
> 对于 SSMA 版本7.4，8.4，.NET 4.5.2 是必备组件。

## <a name="ssma-v83"></a>SSMA v 8。3

SSMA for MySQL 的 v 8.3 版本利用旨在提高质量和转换指标的目标修补程序进行了增强。 此外，此版本的 SSMA for MySQL 提供了以下修补程序：

* 解决辅助功能问题。
* 为 SQL Server 中的类型添加基本支持 `hierarchyid` 。

## <a name="ssma-v82"></a>SSMA

SSMA for MySQL 的7.4 版是使用一组目标修补程序进行增强，旨在改进质量和转换度量，并为以下方面提供修复：

* 数据迁移后禁用的非聚集索引的问题。
* 在无提示安装期间检测 .NET Framework。
* 下载新版本时出现间歇性崩溃。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA v4.0 到 v4.0 8.2 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v81"></a>SSMA 8。1

SSMA for MySQL 的7.4 版是通过旨在提高质量和转换指标的目标修补程序进行增强。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA 8.0 到 app-v 8.1 的更新失败。 如果遇到此错误，请下载并手动安装新版本。

## <a name="ssma-v80"></a>SSMA 8。0

SSMA for MySQL 的 v2.0 版本增强了旨在提高质量和转换指标的目标修补程序。 此版本还提供以下新增功能：

* 支持作为目标的 **AZURE SQL 托管实例** 。 你现在可以创建面向 Azure SQL 托管实例的新项目：

  ![SQL MI 项目](../media/ssma-newproject-sqldbmi.png)

* 转换后 **修补顾问**。 [在此处](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/)了解详细信息。

* 初步的数据库/架构选择。

  连接到源时，用户现在可以选择数据库/感兴趣的架构。 仅选择你计划迁移的架构将在初始连接期间节省时间，并提高总体 SSMA 性能。

  ![SSMA 筛选器对象](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA

SSMA for MySQL 的版本7.10 版本包含以下更改：

* 旨在提供附加安全和隐私保护以满足全局要求更改的目标修补程序。
* 函数名称和参数列表之间的空格转换修复。

## <a name="ssma-v79"></a>SSMA v 7。9

SSMA for MySQL 的 v 7.9 版本包含以下更改：

* 改进质量和转换指标的目标修补程序。
* 部分支持将空间数据类型从 MySQL 迁移到 Azure SQL Database。
* 支持在 SSMA 命令行中更改数据类型映射和项目首选项。
* 支持使用 SQL Server Integration Services (SSIS) 迁移数据。 转换架构后，可以使用右键单击上下文菜单选项创建 SSIS 包。
* 还更改了 SSMA 中的 Azure SQL 数据库连接对话框以指定完全限定的服务器名称。 在以前版本的 SSMA 中，必须在项目设置中显式提到 Azure SQL 数据库前缀。

## <a name="ssma-v78"></a>SSMA v 7。8

SSMA for MySQL 的7.4 版版本包含以下更改：

* 更改项目设置中突出显示的类型映射。
* 允许用户禁用遥测数据。

## <a name="ssma-v77"></a>SSMA v4。0

SSMA for MySQL 的 v4.0 版本包含以下更改：

* SSMA for MySQL 已通过可提高质量和转换指标的目标修补程序进行了增强。
* 基于常用需求，SSMA for MySQL 的32位版本已恢复。 与上一次实现)  (v 7.4 之前的实现相比，有两个安装包，但不能并行安装。 因此，你必须根据你拥有的连接组件选择最适合的版本。 如果可能，始终最好使用64位版本。
* SSMA for MySQL 现在具有 ODBC 连接字符串连接模式，该模式允许你使用与 MySQL 兼容的任何第三方 ODBC 驱动程序。

## <a name="ssma-v76"></a>SSMA v 7。6

SSMA for MySQL 的版本7.6 版本已增强，可提高质量和转换指标，并支持 SQL Server 2017 (公共预览) 。 对 Windows 和 Linux 上的 SQL Server 2017 的支持是公共预览版，不应用于生产迁移。

## <a name="ssma-v75"></a>SSMA 7。5

SSMA for MySQL 的 v2.0 版本已经得到了增强，可确保为残障人士提供更好的辅助功能。

## <a name="ssma-v74"></a>SSMA 7。4

SSMA for MySQL 的7.4 版包含以下更改：

* 在源和目标中的架构对象发现期间，现在可以使用 " **查询超时** " 选项。

    ![query timeout 选项](../media/query-timeout_red.png)
* 根据客户的反馈，改进了质量和转换指标。

> [!IMPORTANT]
> .NET 4.5.2 是安装 SSMA 7.4 的必备组件。 此外，从7.4 版开始，32位版本的 SSMA 即将停止使用。

## <a name="ssma-v73"></a>SSMA 7。3

SSMA for MySQL 的7.3 版包含以下更改：

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

SSMA for MySQL 的 v2.0 版本包含以下更改：

* 根据客户的反馈，改进了质量和转换指标与目标修复。
* 遥测增强功能可提供更好的数据点以解决客户问题并提高 SSMA 的转换率。

## <a name="ssma-v71"></a>SSMA 7。1

SSMA for MySQL 的 v2.0 版本包含以下更改：

* Windows 和 Linux 上的 SQL Server 2017 现在是支持迁移的目标平台。 此功能在 technical preview 中，允许将架构和数据移动到目标 SQL server。
* SSMA 现在支持自动更新，以下载最新版本的 SSMA。
* SSMA 可安装二进制文件现在通过 Windows Installer 包文件 ( .msi) 提供。

## <a name="may-2016"></a>2016 年 5 月

MySQL 的 SSMA 2016 版包含以下更改：

* 添加了对 SQL Server 2016 的支持。
* 改进的分析器和解析程序。
* 移除了 .NET 2.0 的安装程序检查。
* 更新了从 .NET 3.5 到 .NET 4.0 的扩展包。
* 修复了 MySql 的默认 BigInt 类型映射。
* 修复 `save-project` `open-project` 了和用于 SSMA 控制台的命令。
* 修复 `securepassword` 了 SSMA 控制台命令。
* 修复了初始加载的对象计数。
* 修复了 MsSql 对象加载。
* 修复了全局设置中的 bug。

## <a name="march-2016"></a>2016 年 3 月

SSMA for MySQL 的2016年3月预览版本添加了对迁移到 SQL Server 2016 的支持。
  
## <a name="january-2016"></a>2016 年 1 月

SSMA for MySQL 的版本2016的维护版本包含以下更改：

* 已将 "查看日志" 菜单项添加到 SSMA (RFC 5706203) 。
* 添加了遥测。
  
## <a name="july-2014"></a>2014 年 7 月

SSMA for MySQL 的2014年7月发行版本包含以下更改：
  
* 改进了 Azure SQL 数据库代码转换。
* 扩展包功能已移动到架构以支持 Azure SQL 数据库。
* 针对具有超过10k 对象的数据库测试了性能改进。
* 用于处理大量对象的 UI 改进。
* 突出显示众所周知的 LOB 架构 (以便在转换) 时可以将其忽略。
* 转换速度得到改进。
* 在 UI 中显示对象计数。
* 报表大小减少25% 以上。
* 改进了未分析构造的错误消息。
  
## <a name="april-2014"></a>2014 年 4 月

SSMA for MySQL 的2014年4月版包含以下更改：
  
* 添加了对 SQL Server 2014 的支持。
* 修复了有关转换到 Azure 的 bug。
* 修复了 IE 10 中不可见报表页的相关 bug。
  
## <a name="july-2011"></a>2011 年 7 月

SSMA for MySQL 的2011年7月发行版本包含以下更改：
  
* 支持将转换为 `LIMIT` [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] `OFFSET` 。
* 在数据迁移过程中改进了错误报告。
  
## <a name="april-2011"></a>2011 年 4 月

SSMA for MySQL 的2011年4月版包含以下更改：
  
* 可单独安装 "SSMA for MySQL"，它支持 [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] 、 [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] 和 Azure SQL。
* 连接的能力 [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] 。
* 增强了客户端数据迁移引擎，支持并行数据迁移。
* 通过简单和大容量日志恢复模式改进了数据迁移性能。
* SSMA for MySQL 控制台版本支持向后兼容性。 可以打开先前版本的 SSMA v 5.0 创建的项目。
* SSMA for MySQL 5.0 版产品可与 SSMA 产品的旧版本并行安装 (SxS) 。
  
## <a name="july-2010"></a>2010 年 7 月

SSMA for MySQL 的2010年7月发行版包含以下功能：
  
**1. 改进用户界面：**

* MySQL 数据库对象的 "SQL 模式" 选项卡
* MySQL 数据库对象的 "设置" 选项卡
* MySQL 表的 "数据" 选项卡
* 转换和迁移页中更新了项目设置
* 表级的 "数据迁移设置"
  
**2. 用于连接 MySQL 和 SQL Server 的改进：**
  
* MySQL 中的 SSL 连接
* SQL Server 中的加密连接
  
**3. 改进 MySQL 元数据库资源管理器：**
  
* 加载所有 MySQL 数据库对象及其各自的选项卡。
  
**4. 对象转换的改进：**
  
* MySQL 元数据库对象的转换-过程、函数、视图、触发器和语句。
* 对表中空间数据类型的有限支持。
* 用于将 MySQL 函数转换为 SQL Server 存储过程的选项
* 对象转换期间应用 SQL 模式和字符集映射的选项
  
**5. 数据迁移改进：**  
  
* 支持使用 Server-Side 和 Client-Side 数据迁移引擎进行数据迁移
* 支持空间数据迁移
* 用于表的数据迁移的自定义 SQL
  
**6. SSMA for MySQL 控制台：**  
  
* 适用于 MySQL 的 SSMA 的支持控制台功能  
* 支持 Script-Level 接口  
  
## <a name="january-2010"></a>2010年1月

SSMA for MySQL 的2010年1月版是初始版本。 它包含以下功能：
  
* 添加了对本地 SQL Server 和 Azure SQL 迁移的支持。  
  
* **功能快照：** MySQL 表/索引/约束的架构和数据迁移。
