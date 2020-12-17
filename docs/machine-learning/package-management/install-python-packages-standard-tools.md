---
title: 使用 Python 工具安装包
description: 了解如何使用标准 Python 工具将新的 Python 包安装到 SQL Server 机器学习服务的实例。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/21/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: f9c5d221c2deec5506b8d6b80f1000f315e6a352
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471068"
---
# <a name="install-packages-with-python-tools-on-sql-server"></a>使用 Python 工具在 SQL Server 上安装包
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

本文介绍了如何使用标准 Python 工具在 SQL Server 机器学习服务的实例上安装新的 Python 包。 通常情况下，安装新包的流程与在标准 Python 环境中安装类似。 不过，如果服务器未连接到 Internet，还必须执行其他一些步骤。

有关包位置和安装路径的详细信息，请参阅[获取 Python 包信息](python-package-information.md)。

## <a name="prerequisites"></a>必备条件

+ 必须使用 Python 语言选项安装 [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)。

### <a name="other-considerations"></a>其他注意事项

+ 包必须与 Python 3.5 兼容并在 Windows 上运行。

+ Python 包库位于 SQL Server 实例的“程序文件”文件夹中，默认情况下，在此文件夹中安装需要管理员权限。 有关详细信息，请参阅[包库位置](../package-management/python-package-information.md#default-python-library-location)。

+ 根据每个实例进行包安装。 如果有多个机器学习服务实例，则必须将包添加到每个实例中。

+ 数据库服务器经常被锁定。 在许多情况下，Internet 访问完全受阻。 对于有一长串依赖项列表的包，需要提前标识这些依赖项，并准备好手动安装每个依赖项。

+ 在添加包之前，请考虑该包是否适合 SQL Server 环境。

  + 建议将数据库内的 Python 用于那些受益于与数据库引擎紧密集成的任务（如机器学习），而不是简单地查询数据库的任务。

  + 如果添加的包给服务器带来了太多的计算压力，那么性能将受到影响。

  + 在强化的 SQL Server 环境中，可能希望避免使用以下包：
    + 需要网络访问权限的包
    + 需要提升的文件系统访问权限的包
    + 用于 Web 开发或不受益于在 SQL Server 内部运行的其他任务的包

## <a name="add-a-python-package-on-sql-server"></a>在 SQL Server 上添加 Python 包

若要让安装的新 Python 包可以在 SQL Server 上的脚本中使用，请在机器学习服务的实例中安装包。 如果有多个机器学习服务实例，则必须将包添加到每个实例中。

下面各示例中安装的包是 [CNTK](/cognitive-toolkit/)，这是 Microsoft 提供的深度学习框架，可支持自定义、定型和共享不同类型的神经网络。

### <a name="for-offline-install-download-the-python-package"></a>下载 Python 包以供脱机安装

若要在未连接到 Internet 的服务器上安装 Python 包，必须先从连接到 Internet 的计算机下载 WHL 文件，再将此文件复制到服务器。

例如，在连接到 Internet 的计算机上，可以先从站点 `cntk-2.1-cp35-cp35m-win_amd64.whl`[https://cntk.ai/PythonWheel/CPU-Only 下载文件 ](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl)，再将此文件复制到 SQL Server 计算机上的本地文件夹。

> [!IMPORTANT]
> 请务必获取 Windows 版本的包。 如果文件以 .gz 结尾，表明版本可能不正确。

若要详细了解如何下载适用于多个平台和多个版本 Python 的 CNTK 框架，请参阅[在计算机上安装 CNTK](/cognitive-toolkit/Setup-CNTK-on-your-machine)。

### <a name="locate-the-python-library"></a>查找 Python 库

查找 SQL Server 使用的默认 Python 库位置。 如果已安装多个实例，请查找要向其中添加包的实例的 `PYTHON_SERVICES` 文件夹。

例如，如果机器学习服务已使用默认设置进行安装，且默认实例已启用机器学习，那么路径为：

```console
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

> [!TIP]
> 为了方便将来进行调试和测试，不妨设置实例库专用的 Python 环境。

### <a name="install-the-package-using-pip"></a>使用 pip 安装包

使用 pip  安装程序来安装新包。 可以在 `pip.exe` 文件夹的 `Scripts` 子文件夹中找到 `PYTHON_SERVICES`。 由于 SQL Server 安装程序没有将 `Scripts` 子文件夹添加到系统路径，因此你必须指定完整路径，也可以将 Scripts 文件夹添加到 Windows 中的 PATH 变量。

> [!NOTE]
> 若要结合使用 Visual Studio 2017 或 Visual Studio 2015 与 Python 扩展，可以从“Python 环境”`pip install`**窗口中运行** 。 单击“包”  ，然后在文本框中输入要安装的包的名称或位置。 无需键入 `pip install`；系统自动为你填充它。

+ 如果计算机连接到 Internet，请提供包的名称：

  ```console
  scripts\pip.exe install cntk
  ```
  也可以指定包含特定包和版本的 URL，例如：

  ```console
  scripts\pip.exe install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

+ 如果计算机未连接到 Internet，请指定先前下载的 WHL 文件。 例如：

  ```console
  scripts\pip.exe install C:\Downloads\cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

系统可能会提示你提升权限来完成安装。
随着安装的进行，可以在命令提示符窗口中看到状态消息。

### <a name="load-the-package-or-its-functions-as-part-of-your-script"></a>将包或其函数作为脚本的一部分加载

安装完成后，可以立即开始在 SQL Server 上的 Python 脚本中使用包。

若要在脚本中使用包中的函数，请在脚本的初始行中插入标准 `import <package_name>` 语句：

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
import cntk
# Python statements ...
'
```

## <a name="see-also"></a>另请参阅

+ [获取 Python 包信息](python-package-information.md)
+ [SQL Server 机器学习服务的 Python 教程](../tutorials/python-tutorials.md)
+ [用于 CNTK 的 Python API](https://cntk.ai/pythondocs/tutorials.html)。