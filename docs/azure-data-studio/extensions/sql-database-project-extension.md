---
title: SQL 数据库项目扩展
description: 安装和使用 Azure Data Studio 的 SQL 数据库项目扩展
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 10/22/2020
ms.openlocfilehash: bd361913ac7f094e217b6b75163a0dd96d97d7e2
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496738"
---
# <a name="sql-database-projects-extension-preview"></a>SQL 数据库项目扩展（预览版）

SQL 数据库项目扩展（预览版）是在基于项目的开发环境中开发 SQL 数据库的扩展。 


## <a name="features"></a>功能

1. 从连接的数据库创建项目。
2. 创建新的空白项目。
3. 打开以前在 [Azure Data Studio](sql-database-project-extension-getting-started.md) 或 [SQL Server Data Tools](../../ssdt/sql-server-data-tools.md) 中创建的项目。
4. 通过在项目中添加或删除表、视图、存储过程或自定义脚本来编辑项目。
5. 组织文件夹中的文件/脚本。
6. 添加对系统数据库或用户 dacpac 的引用。
7. 生成单个项目。
8. 部署单个项目。
9. 从部署配置文件加载连接详细信息（SQL Windows 身份验证）和 SQLCMD 变量。

请观看这个 10 分钟的简短视频，了解 Azure Data Studio 中的 SQL 数据库项目扩展：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Build-SQL-Database-Projects-Easily-in-Azure-Data-Studio/player?WT.mc_id=dataexposed-c9-niner]

## <a name="install-the-sql-database-projects-extension"></a>安装 SQL 数据库项目扩展

1. 打开扩展管理器以访问可用的扩展。  为此，请选择扩展图标，也可以在“视图”菜单中选择“扩展”。
2. 通过在扩展搜索框中键入全部或部分名称来找到“SQL 数据库项目”扩展。 选择某个可用扩展以查看其详细信息。

   ![安装扩展](media/sql-database-projects-extension/install-database-projects.png)

3. 选择所需的扩展并“安装”它。
4. 选择“重新加载”以启用扩展（仅在第一次安装扩展时是必需的）。
5.  从活动栏中选择文件图标，或从“视图”菜单中选择“资源管理器”。 “项目”的新 viewlet 现已可用。

   > [!NOTE]
   > 项目生成功能需要 .NET Core SDK，如果扩展无法检测到 .NET Core SDK，系统将提示你安装它。  .NET Core SDK（v3.1 或更高版本）可以从 [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) 下载和安装。

   > [!NOTE]
   > 建议随 SQL 数据库项目扩展安装[架构比较扩展](schema-compare-extension.md)，以获得完整功能。

## <a name="known-limitations"></a>已知的限制

1. 当前不支持在 Azure Data Studio viewlet 中添加项目引用和加载现有项目引用。
2. 目前 Azure Data Studio viewlet 不支持以链接的形式加载文件，但会在树的顶层加载文件，生成会按预期合并这些文件。
3. 目前还不支持在 viewlet 中添加和加载部署前后的脚本，但如果在项目中手动添加这些文件，它们将在生成时得到执行。
4. .NET Core 版本的 DacFx 不支持项目中的 SQLCLR 对象。
5. 任务（生成/发布）不是用户定义的。
6. 发布 DacFx 定义的目标。
7. 源代码管理集成和新项目创建不会自动创建 .gitignore 文件。
8. WSL 环境支持受限。

## <a name="next-steps"></a>后续步骤

- [SQL 数据库项目扩展入门](sql-database-project-extension-getting-started.md)
- [使用 Azure Data Studio 的 SQL 数据库项目扩展生成并发布项目](sql-database-project-extension-build.md)
