---
title: 安装 Microsoft ODBC Driver for SQL Server (macOS)
description: 了解如何在 macOS 客户端上安装 Microsoft ODBC Driver for SQL Server 来启用数据库连接。
ms.date: 09/08/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a07f2760503e5df9897f4a4ef065621bd5852523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042917"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-macos"></a>安装 Microsoft ODBC Driver for SQL Server (macOS)

本文介绍如何在 macOS 上安装 Microsoft ODBC Driver for SQL Server。 本文还包括有关 SQL Server 的可选命令行工具（`bcp` 和 `sqlcmd`）以及 unixODBC 开发标头的说明。

本文提供了用于从 bash shell 安装 ODBC 驱动程序的命令。 如果要直接下载包，请参阅[下载 ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md)。

## <a name="microsoft-odbc-17"></a>Microsoft ODBC 17

若要在 macOS 上安装 Microsoft ODBC Driver 17 for SQL Server，请运行以下命令：

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=Y brew install msodbcsql17 mssql-tools
```

> [!IMPORTANT]
> 如果安装了暂时可用的 v17 `msodbcsql` 包，应先删除它，再安装 `msodbcsql17` 包。 这样可避免冲突。 `msodbcsql17` 包可以与 `msodbcsql` v13 包并行安装。

## <a name="previous-versions"></a>以前的版本

以下各节提供了有关在 macOS 上安装 Microsoft ODBC 驱动程序的早期版本的说明。

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

使用以下命令在 OS X 10.11 (El Capitan) 和 macOS 10.12 (Sierra) 上安装 Microsoft ODBC Driver 13.1 for SQL Server：

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="driver-files"></a>驱动程序文件

macOS 上的 ODBC 驱动程序由以下组件构成：

|组件|说明|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib 或 libmsodbcsql.13.dylib|包含该驱动程序所有功能的动态库 (`dylib`) 文件。 此文件安装在 `/usr/local/lib/` 中。|  
|`msodbcsqlr17.rll` 或 `msodbcsqlr13.rll`|驱动程序库的附带资源文件。 此文件安装在 ODBC Driver 17 的 `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` 中和 ODBC Driver 13 的 `[driver .dylib directory]../share/msodbcsql/resources/en_US/` 中。 | 
|msodbcsql.h|头文件，它包含使用驱动程序所需的所有新定义。<br /><br /> **注意：** 无法在同一个程序中引用 msodbcsql.h 和 odbcss.h。<br /><br /> msodbcsql.h 安装在 ODBC Driver 17 的 `/usr/local/include/msodbcsql17/` 中和 ODBC Driver 13 的 `/usr/local/include/msodbcsql/` 中。 |
|LICENSE.txt|包含最终用户许可协议条款的文本文件。 此文件位于 ODBC Driver 17 的 `/usr/local/share/doc/msodbcsql17/` 中和 ODBC Driver 13 的 `/usr/local/share/doc/msodbcsql/` 中。 |
|RELEASE_NOTES|包含发行说明的文本文件。 此文件位于 ODBC Driver 17 的 `/usr/local/share/doc/msodbcsql17/` 中和 ODBC Driver 13 的 `/usr/local/share/doc/msodbcsql/` 中。 |

## <a name="resource-file-loading"></a>资源文件加载

驱动程序需要加载资源文件才能正常运行。 此文件称为 `msodbcsqlr17.rll` 或 `msodbcsqlr13.rll`，具体取决于驱动程序版本。 `.rll` 文件的位置与驱动程序本身的位置（`so` 或 `dylib`）相对，如上表中所述。 自版本 17.1 开始，如果从相对路径加载失败，驱动程序还将尝试从默认目录加载 `.rll`。 macOS 上的默认资源文件路径是 `/usr/local/share/msodbcsql17/resources/en_US/`

## <a name="troubleshooting"></a>疑难解答

在安装 ODBC 驱动程序后尝试进行连接时，某些用户会遇到问题，并收到如下所示的错误：`"[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 17 for SQL Server' : file not found (0) (SQLDriverConnect)"`。 这可能是由于未正确配置 unixODBC ，无法找到已注册的驱动程序。 在这种情况下，创建几个符号链接可以解决此问题。

```bash
sudo ln -s /usr/local/etc/odbcinst.ini /etc/odbcinst.ini
sudo ln -s /usr/local/etc/odbc.ini /etc/odbc.ini
```

对于无法使用 ODBC 驱动程序与 SQL Server 建立连接的其他情况，请参阅[解决连接问题](known-issues-in-this-version-of-the-driver.md#connectivity)上的“已知问题”一文。

## <a name="next-steps"></a>后续步骤

安装驱动程序后，可以尝试 [C++ ODBC 示例应用程序](../../odbc/cpp-code-example-app-connect-access-sql-db.md)。 有关开发 ODBC 应用程序的详细信息，请参阅[开发应用程序](../../../odbc/reference/develop-app/developing-applications.md)。

有关详细信息，请参阅 ODBC 驱动程序[发行说明](release-notes-odbc-sql-server-linux-mac.md)和[系统要求](system-requirements.md)。
