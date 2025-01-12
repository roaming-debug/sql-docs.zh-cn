---
title: 安装 Python 自定义运行时
description: 了解如何使用语言扩展为 SQL Server 安装 Python 自定义运行时。 Python 自定义运行时可以运行机器学习脚本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 23f1bac7674002178602c0d8fa844531ea4f1a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072860"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>为 SQL Server 安装 Python 自定义运行时
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

了解如何在以下操作系统上安装 Python 自定义运行时，以便使用 SQL Server 运行外部 Python 脚本：

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

自定义运行时可以运行机器学习脚本，并使用 [SQL Server 语言扩展](../../language-extensions/language-extensions-overview.md)。

可以将自己的 Python 运行时版本与 SQL Server 一起使用，而不是使用随 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)安装的默认运行时版本。

::: zone pivot="platform-windows"
[!INCLUDE [Python custom runtime - Windows](includes/custom-runtime-python-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-python-linux-ubuntu.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - RHEL specific steps](includes/custom-runtime-python-linux-rhel.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - SLES specific steps](includes/custom-runtime-python-linux-sles.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>启用外部脚本

可以通过存储过程 [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行 Python 外部脚本。

若要启用外部脚本，请使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) 执行下面的语句。

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>验证安装

使用以下 SQL 脚本验证 Python 自定义运行时的安装和功能。

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>后续步骤

+ [为 SQL Server 安装 R 自定义运行时](custom-runtime-r.md)
+ [SQL Server 中的扩展性框架](../concepts/extensibility-framework.md)
+ [语言扩展概述](../../language-extensions/language-extensions-overview.md)
