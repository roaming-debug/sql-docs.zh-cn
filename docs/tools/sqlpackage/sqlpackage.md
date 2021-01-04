---
title: SqlPackage.exe
description: 了解如何通过 SqlPackage.exe 自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577786"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

SqlPackage.exe 是一个命令行实用工具，可自动处理以下数据库开发任务  ：  
  
- [版本](#version)：返回 SqlPackage 应用程序的生成号。  在版本 18.6 中添加。

- [提取](sqlpackage-extract.md)：从活动的 SQL Server 或 Azure SQL 数据库创建数据库快照 (.dacpac) 文件。  
  
- [发布](sqlpackage-publish.md)：增量更新数据库架构以便匹配源 .dacpac 文件的架构。 如果该数据库在服务器上不存在，则发布操作将创建它。 否则，将更新现有数据库。  
  
- [导出](sqlpackage-export.md)：将活动数据库（包括数据库架构和用户数据）从 SQL Server 或 Azure SQL 数据库导出到 BACPAC 包（.bacpac 文件）。  
  
- [导入](sqlpackage-import.md)：将架构和表数据从 BACPAC 包导入到 SQL Server 或 Azure SQL 数据库的实例中的新用户数据库中。  
  
- [DeployReport](sqlpackage-deploy-drift-report.md)：创建将由发布操作完成的更改的 XML 报表。  
  
- [DriftReport](sqlpackage-deploy-drift-report.md)：创建自注册数据库注册以来已对其做出的更改的 XML 报表。  
  
- [脚本](sqlpackage-script.md)：创建 Transact-SQL 增量更新脚本，该脚本可更新目标的架构以匹配源的架构。  
  
通过 SqlPackage.exe 命令行，可以指定这些操作以及特定于操作的参数和属性  。  

[下载最新版本](sqlpackage-download.md)  。 有关最新版本的详细信息，请参阅[发行说明](release-notes-sqlpackage.md)。
  
## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>用法示例

**使用带有 SQL 脚本输出的 .dacpac 文件生成数据库之间的比较结果**

首先，创建包含最新数据库更改的 .dacpac 文件：

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
创建包含数据库目标（没有更改）的 .dacpac 文件：

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

创建一个 SQL 脚本，用于生成两个 .dacpac 文件之间的差异：

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>版本

将 sqlpackage 版本显示为生成号。  可在交互提示和[自动管道](sqlpackage-pipelines.md)中使用。

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>退出代码

用于返回以下退出代码的命令：

- 0 = 成功
- 非零 = 失败


## <a name="next-steps"></a>后续步骤

- 了解有关 [SqlPackage 提取](sqlpackage-extract.md)的详细信息
- 了解有关 [SqlPackage 发布](sqlpackage-publish.md)的详细信息
- 了解有关 [SqlPackage 导出](sqlpackage-export.md)的详细信息
- 了解有关 [SqlPackage 导入](sqlpackage-import.md)的详细信息
