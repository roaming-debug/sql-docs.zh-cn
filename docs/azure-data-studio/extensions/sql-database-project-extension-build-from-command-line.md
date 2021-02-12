---
title: 从命令行生成项目
description: 从命令行生成 SQL Server 数据库项目
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 08/07/2020
ms.openlocfilehash: 8f8fd9745391d4fb85a12fa2136e4d3db0b797de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040322"
---
# <a name="build-a-database-project-from-command-line"></a>从命令行生成数据库项目

尽管 Azure Data Studio 的 SQL 数据库项目扩展（预览版）提供了图形用户界面来[生成数据库项目](sql-database-project-extension-build.md)，但 Windows、macOS 和 Linux 环境也提供了命令行生成体验。 本文概述了从命令行中将 SQL 项目生成到 dacpac 所需的先决条件和语法。

## <a name="prerequisites"></a>先决条件

1. 安装并配置 [Azure Data Studio 的 SQL 数据库项目扩展](sql-database-project-extension.md)。

2. 若要从 SQL 数据库项目的 Azure Data Studio 扩展支持的所有平台通过命令行生成 SQL 数据库项目，需要以下 .NET Core dll 和目标文件 `Microsoft.Data.Tools.Schema.SqlTasks.targets`。 在 Azure Data Studio 接口中完成的第一个生成过程中，扩展会创建这些文件，并将其置于 `BuildDirectory` 下的扩展文件夹中。  例如，在 Linux 上，这些文件位于 `~\.azuredatastudio\extensions\microsoft.sql-database-projects-x.x.x\BuildDirectory\`。  将这 10 个文件复制到一个新的可访问的文件夹中，或记下其位置。  本文档中，该位置称为 `DotNet Core build folder`。

    - Microsoft.Data.Tools.Schema.Sql.dll
    - Microsoft.Data.Tools.Schema.Tasks.Sql.dll
    - Microsoft.Data.Tools.Utilities.dll
    - System.Io.Packaging.dll
    - Microsoft.SqlServer.Dac.dll
    - Microsoft.SqlServer.Dac.Extensions.dll
    - Microsoft.SqlServer.TransactSql.ScriptDom.dll
    - Microsoft.SqlServer.Types.dll
    - Microsoft.Data.Tools.Schema.SqlTasks.targets
    - System.ComponentModel.Composition.dll
    - Microsoft.Data.SqlClient.dll

3. 如果项目是在 Azure Data Studio 中创建的，请跳到[从命令行生成项目](#build-the-project-from-the-command-line)。 如果项目是在 SQL Server Data Tools (SSDT) 中创建的，请在 Azure Data Studio SQL 数据库项目扩展中打开该项目。  在 Azure Data Studio 中打开项目会自动通过三次编辑更新 `sqlproj` 文件，请注意以下信息：

    1. 导入条件

    ```console
    <Import Condition="'$(NetCoreBuild)' == 'true'" Project="$(NETCoreTargetsPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/> 
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' != ''" Project="$(SQLDBExtensionsRefPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' == ''" Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    ```

    2. 包参考

    ```console
    <ItemGroup>
        <PackageReference Condition="'$(NetCoreBuild)' == 'true'" Include="Microsoft.NETFramework.ReferenceAssemblies" Version="1.0.0" PrivateAssets="All"/>
    </ItemGroup>
    ```

    3. 清理目标，这是在 SQL Server Data Tools (SSDT) 和 Azure Data Studio 中支持双重编辑所必需的

        ```console
        <Target Name="AfterClean">
            <Delete Files="$(BaseIntermediateOutputPath)\project.assets.json"/>
        </Target>
        ```

## <a name="build-the-project-from-the-command-line"></a>从命令行生成项目

从完整的 .NET 文件夹中，使用以下命令：

```console
dotnet build "<sqlproj file path>" /p:NetCoreBuild=true /p:NETCoreTargetsPath="<DotNet Core build folder>"
```

例如，通过 Linux 上的 `/usr/share/dotnet`：

```console
dotnet build "/home/myuser/Documents/DatabaseProject1/DatabaseProject1.sqlproj" /p:NetCoreBuild=true /p:NETCoreTargetsPath="/home/myuser/.azuredatastudio-insiders/extensions/microsoft.sql-database-projects-0.1.2/BuildDirectory"  
```

## <a name="next-steps"></a>后续步骤

- [Azure Data Studio 的 SQL 数据库项目扩展](sql-database-project-extension.md)
- [发布 SQL 数据库项目](sql-database-project-extension-build.md#publish-a-database-project)
