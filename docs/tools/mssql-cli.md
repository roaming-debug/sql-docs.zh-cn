---
title: mssql-cli
description: mssql-cli 是用于 SQL Server 的交互式命令行查询工具，在 Windows、macOS 或 Linux 上运行。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein, maghan
ms.custom: tools|mssql-cli
ms.date: 02/22/2018
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 1514c776944b66757a23baf06bb5169f66171f2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033517"
---
# <a name="mssql-cli-command-line-query-tool-for-sql-server-preview"></a>用于 SQL Server 的 mssql-cli 命令行查询工具（预览版）

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

mssql-cli 是用于查询 SQL Server 的交互式命令行工具，可在 Windows、macOS 或 Linux 上进行运行。

## <a name="install-mssql-cli"></a>安装 mssql-cli

有关安装说明的详细信息，请参阅[安装指南](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)。 下面汇总了最常见的安装方案。

### <a name="windows-and-macos-installation"></a>Windows 和 macOS 安装

使用 `pip` 在 Windows 和 macOS 上安装 mssql-cli：

```$ pip install mssql-cli```

有关更多详细说明，请参阅[安装指南](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)。

### <a name="linux-installation"></a>Linux 安装

注册 Microsoft 存储库后，可通过多个 Linux 分发上的包管理器来安装和升级 mssql-cli。

以下示例适用于 Ubuntu 18.04 (Bionic)，有关其他分发的详细信息和示例，请参阅[安装指南](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)。

```
# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Ubuntu repository
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod

# Update the list of products
sudo apt-get update

# Install mssql-cli
sudo apt-get install mssql-cli

# Install missing dependencies
sudo apt-get install -f
```

## <a name="mssql-cli-documentation"></a>mssql-cli 文档

mssql-cli 文档位于 [mssql-cli GitHub 存储库](https://github.com/dbcli/mssql-cli)中。

- [主页/自述文件](https://github.com/dbcli/mssql-cli)
- [安装指南](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)
- [使用指南](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)

其他文档位于 [doc 文件夹](https://github.com/dbcli/mssql-cli/tree/master/doc)中。
