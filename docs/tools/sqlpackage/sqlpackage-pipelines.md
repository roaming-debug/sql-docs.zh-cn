---
title: 开发管道中的 SqlPackage
description: 了解如何利用 SqlPackage.exe 通过查看安装的生成号对数据库开发管道进行故障排除。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 002d145328ca101fee467428e5b7c8b0ff1fdd95
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577793"
---
# <a name="sqlpackage-in-development-pipelines"></a>开发管道中的 SqlPackage

SqlPackage.exe 是一个命令行实用工具，可自动处理多个数据库开发任务，并可合并到 CI/CD 管道中。

## <a name="managed-virtual-environments"></a>托管虚拟环境

用于 GitHub Actions 托管的运行程序和 Azure Pipelines VM 映像的虚拟环境在[虚拟环境](https://github.com/actions/virtual-environments) GitHub 存储库中进行托管。  SqlPackage 包含在 `windows-latest` 环境中，在每次 SqlPackage 发布后的几个星期内，都会对映像进行更新。

## <a name="checking-the-sqlpackage-version"></a>查看 SqlPackage 版本

在故障排除过程中，必须了解所使用的 SqlPackage 版本。  若要获得此信息，可以在管道中添加一个步骤，以使用 `/version` 参数运行 SqlPackage。  下面提供了基于 Microsoft 和 GitHub 托管环境的示例，自承载环境的工作目录可能具有不同的安装路径。

### <a name="azure-pipelines"></a>Azure Pipelines

通过利用 Azure 管道中的 [script](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema#script) 关键字，可以向 Azure 管道添加一个步骤，以输出 SqlPackage 版本号。

```yaml
- script: sqlpackage.exe /version
  workingDirectory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  displayName: 'get sqlpackage version'
```

### <a name="github-actions"></a>GitHub 操作

通过利用 GitHub 操作工作流中的 [run](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) 关键字，可以向 GitHub 操作添加一个步骤，以输出 SqlPackage 版本号。

```yaml
- name: get sqlpackage version
  working-directory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  run: ./sqlpackage.exe /version
```

:::image type="content" source="media/sqlpackage-pipelines-github-action.png" alt-text="显示生成号 15.0.4897.1 的 GitHub 操作输出":::

## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息
