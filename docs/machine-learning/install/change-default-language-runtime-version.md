---
title: 更改默认 R 或 Python 语言运行时版本
description: 了解如何通过 SQL Server 2017 机器学习服务或 SQL Server R Services 更改 SQL 实例使用的 R 或 Python 运行时的默认版本。
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/14/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 8ba60de81e880c03db50031c8489850b28a77018
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471208"
---
# <a name="change-the-default-r-or-python-language-runtime-version"></a>更改默认 R 或 Python 语言运行时版本

[!INCLUDE[SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

本文介绍如何更改 [SQL Server 2016 R Services](../r/sql-server-r-services.md) 或 [SQL Server 2017 机器学习服务](../sql-server-machine-learning-services.md)中使用的 R 或 Python 的默认版本。

下面列出了不同的 SQL Server 版本中包含的 R 和 Python 运行时版本。

| SQL Server 版本 | 服务 | 累计更新 | R 运行时版本 | Python 运行时版本 |
|-|-|-|-|-|
| SQL Server 2016 | R 服务 | RTM - SP2 CU13 | 3.2.2 | 不可用 |
| SQL Server 2016 | R 服务 | SP2 CU14 及更高版本 | 3.2.2 和 3.5.2 | 不可用 |
| SQL Server 2017 | 机器学习服务 | RTM - CU21 | 3.3.3 | 3.5.2 |
| SQL Server 2017 | 机器学习服务 | CU22 及更高版本 | 3.3.3 和 3.5.2 | 3.5.2 和 3.7.2 |

## <a name="prerequisites"></a>必备知识

需要安装累积更新 (CU) 才能更改默认 R 或 Python 语言运行时版本：

- **SQL Server 2016：** Services Pack (SP) 2 累积更新 (CU) 14 或更高版本
- SQL Server 2017：累积更新 (CU) 22 或更高版本

若要下载最新的累积更新，请参阅 [Microsoft SQL Server的最新更新](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

> [!NOTE]
> 如果使用 SQL Server 的全新安装补充累积更新，则将仅安装最新版本的 R 和 Python 运行时。

## <a name="change-r-runtime-version"></a>更改 R 运行时版本

如果已安装 SQL Server 2016 或 2017 的上述累积更新之一，则 SQL 实例中可能有 R 的多个版本。 每个版本包含在名称为 `R_SERVICES.`&lt;major&gt;.&lt;minor&gt; 的实例文件夹的子文件夹中（原始安装中的文件夹可能没有附加到文件夹名称的版本号）。

如果安装包含 R 3.5 的 CU，则新的 `R_SERVICES` 文件夹为：

- SQL Server 2016：`C:\Program Files\Microsoft SQL Server\MSSQL13.<INSTANCE_NAME>\R_SERVICES.3.5`
- SQL Server 2017：`C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\R_SERVICES.3.5`

每个 SQL 实例使用其中一个版本作为 R 的默认版本。可以使用 RegisterRext.exe 命令行实用工具来更改默认版本。 该实用工具位于每个 SQL 实例中的 R 文件夹下：

&lt;SQL 实例路径&gt;\R_SERVICES.n.n\library\RevoScaleR\rxLibs\x64\RegisterRext.exe

> [!Note]
> 本文中所述的功能仅适用于 SQL CU 中包含的 RegisterRext.exe 的副本。 不要使用原始 SQL 安装附带的副本。

若要更改 R 运行时版本，请将以下命令行参数传递给 RegisterRext.exe：

- `/configure` - 必需，指定你配置的是默认 R 版本。

- `/instance:`&lt;实例名称&gt; - 可选，要配置的实例。 如果未指定，则将配置默认实例。

- `/rhome:`&lt;R_SERVICES[n.n] 文件夹的路径&gt; - 可选，要设置为默认 R 版本的运行时版本文件夹的路径。

  如果未指定 /rhome，则配置的路径为 RegisterRext.exe 所在的路径。

### <a name="examples"></a>示例

下面是有关如何更改 SQL Server 2016 和 2017 中的 R 运行时版本的示例。

#### <a name="change-r-runtime-version-in-sql-server-2016"></a>更改 SQL Server 2016 中的 R 运行时版本

例如，若要将 R 3.5 配置为 SQL Server 2016 上实例 MSSQLSERVER01 的 R 的默认版本，请执行以下操作：

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

#### <a name="change-r-runtime-version-in-sql-server-2017"></a>更改 SQL Server 2017 中的 R 运行时版本

例如，若要将 R 3.5 配置为 SQL Server 2017 上实例 MSSQLSERVER01 的 R 的默认版本，请执行以下操作：

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

在这些示例中，无需包含 `/rhome` 参数，因为指定的文件夹为 RegisterRext.exe 所在的文件夹。

## <a name="change-python-runtime-version"></a>更改 Python 运行时版本

如果已安装 SQL Server 2017 的 CU22 或更高版本，则 SQL 实例中可能有 Python 的多个版本。 每个版本包含在名称为 `PYTHON_SERVICES.`&lt;major&gt;.&lt;minor&gt; 的实例文件夹的子文件夹中（原始安装中的文件夹可能没有附加到文件夹名称的版本号）。

例如，如果安装包含 Python 3.7 的 CU，则会创建新的 `PYTHON_SERVICES` 文件夹：

`C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\PYTHON_SERVICES.3.7`  

每个 SQL 实例使用其中一个版本作为 Python 的默认版本。 可以使用 RegisterRExt.exe 命令行实用工具来更改默认版本。 该实用工具位于每个 SQL 实例中的 Python 文件夹下：

&lt;SQL 实例路径&gt;`\PYTHON_SERVICES.n.n\Lib\site-packages\revoscalepy\rxLibs\RegisterRExt.exe`

> [!Note]
> 本文中所述的功能仅适用于 SQL CU 中包含的 RegisterRExt.exe 的副本。 不要使用原始 SQL 安装附带的副本。

若要更改 Python 运行时版本，请将以下命令行参数传递给 RegisterRext.exe：

- `/configure` - 必需，指定你配置的是默认 Python 版本。

- `/python` - 指定你配置的是默认 Python 版本。 如果指定 `/pythonhome`，则为可选。

- `/instance:`&lt;实例名称&gt; - 可选，要配置的实例。 如果未指定，则将配置默认实例。

- `/pythonhome:`&lt;PYTHON_SERVICES[n.n] 文件夹的路径&gt; - 可选，要设置为默认 Python 版本的运行时版本文件夹的路径。

  如果未指定 /pythonhome，则配置的路径为 RegisterRExt.exe 所在的路径。

### <a name="example"></a>示例

例如，若要将 Python 3.7 配置为 SQL Server 2017 上实例 MSSQLSERVER01 的 Python 的默认版本，请执行以下操作：

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7\Lib\site-packages\revoscalepy\rxLibs"

.\RegisterRext.exe /configure /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

在此示例中，无需包含 `/pythonhome` 参数，因为指定的文件夹为 RegisterRext.exe 所在的文件夹。

## <a name="remove-a-runtime-version"></a>删除运行时版本

若要删除 R 或 Python 的版本，请使用前面所述的相同 `/rhome`、`/pythonhome` 和 `/instance` 参数将 RegisterRExt.exe 与 `/cleanup` 命令行参数结合使用。

例如，若要从实例 MSSQLSERVER01 中删除 R 3.2 文件夹，请执行以下操作：

```cmd
.\RegisterRext.exe /cleanup /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES" /instance:MSSQLSERVER01
```

例如，若要从实例 MSSQLSERVER01 中删除 Python 3.7 文件夹，请执行以下操作：

```cmd
.\RegisterRExt.exe /cleanup /python /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

RegisterRext.exe 将要求你确认已指定 R 运行时的清除：

> 是否确定要永久删除给定运行时以及该运行时上安装的所有包?\[是(Y)/否(N)/默认值(是)\]：

若要确认，请回复 `Y` 或按 Enter。 此外，也可以通过在 `/cleanup` 选项中传入 `/y` 或 `/Yes` 来跳过此提示。

> [!NOTE]
> 仅当某个版本未配置为默认版本且当前未用于运行 RegisterRext.exe 时，才可将其删除。

## <a name="next-steps"></a>后续步骤

- [获取 R 包信息](../package-management/r-package-information.md)
- [获取 Python 包信息](../package-management/python-package-information.md)
- [使用 R 工具安装包](../package-management/install-r-packages-standard-tools.md)
- [使用 Python 工具来安装包](../package-management/install-python-packages-standard-tools.md)
