---
title: 下载并安装 Azure Data Studio
description: 下载并安装适用于 Windows、macOS 或 Linux 的 Azure Data Studio。 本文提供发布日期、版本号、系统要求和下载链接。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 3/17/2021
ms.openlocfilehash: 0a88fcbdd027215b8ce1bcd6242f48833ec0c6b2
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610816"
---
# <a name="download-and-install-azure-data-studio"></a>下载并安装 Azure Data Studio

Azure Data Studio 是一种跨平台的数据库工具，适合在 Windows、macOS 和 Linux 上使用本地和云数据平台的数据专业人员。

Azure Data Studio 利用 IntelliSense、代码片段、源代码管理集成和集成终端提供新式编辑器体验。 它在设计时考虑了数据平台用户，带有内置查询结果集图表和可自定义的仪表板。 有关 Azure Data Studio 的详细信息，请访问[什么是 Azure Data Studio](what-is-azure-data-studio.md)。

## <a name="download-the-latest-release"></a>下载最新版本

| 平台 | 下载 | 发布日期 | 版本 |
|----------|----------|--------------|---------|
| Windows | [用户安装程序（推荐）](https://go.microsoft.com/fwlink/?linkid=2157460)<br>[系统安装程序](https://go.microsoft.com/fwlink/?linkid=2157459)<br>[.zip](https://go.microsoft.com/fwlink/?linkid=2157458) | 2021 年 3 月 17 日 | 1.27.0 |
| macOS | [.zip](https://go.microsoft.com/fwlink/?linkid=2157456) | 2021 年 3 月 17 日 | 1.27.0 |
| Linux | [.deb](https://go.microsoft.com/fwlink/?linkid=2157352)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2157248)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2157353) | 2021 年 3 月 17 日 | 1.27.0 |

有关最新版本的详细信息，请参阅[发行说明](./release-notes-azure-data-studio.md)。

## <a name="get-azure-data-studio-for-windows"></a>获取适用于 Windows 的 Azure Data Studio

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

此版本的 Azure Data Studio 包含标准 Windows Installer 体验和 .zip 文件。

建议使用用户安装程序，因为它不需要管理员权限，从而简化安装和升级。 用户安装程序不需要管理员权限，因为它位于用户本地的 AppData (LOCALAPPDATA) 文件夹下。 用户安装程序还提供更流畅的后台更新体验。 有关详细信息，请参阅 [Windows 用户设置](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows)。

**用户安装程序**（推荐）

1. 下载并运行[适用于 Windows 的 Azure Data Studio 用户安装程序](https://go.microsoft.com/fwlink/?linkid=2157460)。
2. 启动 Azure Data Studio 应用。

**系统安装程序**

1. 下载并运行[适用于 Windows 的 Azure Data Studio 系统安装程序](https://go.microsoft.com/fwlink/?linkid=2157459)。
2. 启动 Azure Data Studio 应用。

**zip 文件**

1. 下载[适用于 Windows 的 Azure Data Studio .zip](https://go.microsoft.com/fwlink/?linkid=2157458)。
2. 浏览到下载文件并将其解压缩。
3. `\azuredatastudio-windows\azuredatastudio.exe`运行

## <a name="get-azure-data-studio-for-macos"></a>获取适用于 macOS 的 Azure Data Studio

1. 下载[适用于 macOS 的 Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2157456)。
2. 若要展开 zip 的内容，请双击。
3. 若要使 Azure Data Studio 在启动板中可用，请将 Azure Data Studio.app 拖到“Applications”文件夹中  。

## <a name="get-azure-data-studio-for-linux"></a>获取适用于 Linux 的 Azure Data Studio

1. 通过使用安装程序之一或 tar.gz 存档来下载适用于 Linux 的 Azure Data Studio：
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2157352)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2157248)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2157353)
1. 若要提取文件并启动 Azure Data Studio，请打开一个新的终端窗口并键入以下命令：

   **Debian 安装：**

   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **rpm 安装：**

   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **tar.gz 安装：**

   ```bash
   cd ~
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc
   azuredatastudio
   ```

   > [!NOTE]
   > 在 Debian、Redhat 和 Ubuntu 上，可能缺少依赖项。 使用以下命令安装这些依赖项，具体取决于你的 Linux 版本：

   **Debian：**

   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat：**

   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu：**

   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```

## <a name="download-insiders-build-of-azure-data-studio"></a>下载 Azure Data Studio 的预览体验内部版本

通常，用户应下载上述 Azure Data Studio 的稳定版本。 但是，如果希望试用 Beta 版功能并发送反馈，可以下载 [Azure Data Studio 的预览体验内部版本。](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-main)

## <a name="supported-operating-systems"></a>受支持的操作系统

Azure Data Studio 在 Windows、macOS 和 Linux 上运行，并在以下平台上受支持：

### <a name="windows"></a>Windows

- Windows 10（64 位）
- Windows 8.1（64 位）
- Windows 8（64 位）
- Windows 7 (SP1)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2（64 位）
- Windows Server 2012（64 位）
- Windows Server 2008 R2（64 位）

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra
- macOS 10.12 Sierra
- macOS 11.1 Big Sur

### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>建议的系统要求

| 推荐/最低 | CPU 内核数 | 内存/RAM |
|---------------------|-----------|------------|
|     建议     |     4     |   8 GB     |
|     最小值         |     2     |   4 GB     |

## <a name="check-for-updates"></a>检查更新

若要检查是否有最新更新，请选择窗口左下角的齿轮图标，然后选择“检查是否有更新”。

脱机环境可以通过直接在以前安装的版本上[安装最新版本](#download-and-install-azure-data-studio)来应用更新。 不需要卸载 Azure Data Studio 的先前版本。 安装程序将更新当前安装的应用程序（如果有）。

## <a name="supported-sql-offerings"></a>支持的 SQL 产品/服务

- 此版本的 Azure Data Studio 适用于所有[受支持版本的 [!INCLUDE [sssql14-md](../includes/sssql14-md.md)] - [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](https://support.microsoft.com/lifecycle?C2=1044)，并且支持与 Azure SQL 数据库和 Azure Synapse Analytics 中的最新云功能配合使用。 Azure Data Studio 还提供对 Azure SQL 托管实例的预览支持。

## <a name="move-user-settings"></a>移动用户设置

如果要将 SQL Operations Studio 更新为 Azure Data Studio 且希望保留设置、键盘快捷方式或代码片段，请按照以下步骤操作。

如果已经拥有 Azure Data Studio，或从未安装或自定义过 SQL Operations Studio，可忽略此部分。

1. 选择左下方的齿轮，然后选择“设置”，以打开设置。

   ![编辑 Azure Data Studio 中的设置](./media/download/open-settings.png)

2. 右键单击顶部的“用户设置”选项卡，然后选择“在资源管理器中显示” 

   ![启动资源管理器，它将转到本地文件系统](./media/download/reveal-in-explorer.png)

3. 复制此文件夹中的所有文件，并将其保存在本地驱动器上易于查找的位置，例如文档文件夹。

   ![使用这些文件并将其复制到你的位置](./media/download/copy-settings.png)

4. 在新版 Azure Data Studio 中，执行步骤 1-2，然后在步骤 3 中将保存的内容粘贴到文件夹。 也可以在其各自位置上手动复制设置、键绑定或代码片段。

5. 若要覆盖现有安装，请在安装之前删除旧安装目录，以避免连接到资源浏览器的 Azure 帐户时出错。

## <a name="unattended-install-for-windows"></a>适用于 Windows 的无人参与安装

还可以使用命令提示符脚本安装 Azure Data Studio。

如果你使用 Windows 平台，并且想要在没有 GUI 提示的情况下在后台安装 Azure Data Studio，请执行以下步骤。

1. 使用提升的权限启动命令提示符。

2. 在命令提示符下键入以下命令。

    ```console
    <path where the azuredatastudio-windows-user-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    ```

    示例：

    ```console
    %systemdrive%\azuredatastudio-windows-user-setup-1.24.0.exe /VERYSILENT /MERGETASKS=!runcode
    ```

    > [!Note]
    > 该示例还适用于系统安装程序文件。
    > 
    > ```console
    > <path where the azuredatastudio-windows-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    > ```

    你也可以传递 /SILENT 而不是 /VERYSILENT 来查看设置 UI 。

3. 如果一切顺利，便可以看到 Azure Data Studio 安装成功。

## <a name="uninstall-azure-data-studio"></a>卸载 Azure Data Studio

如果使用 Windows 安装程序安装了 Azure Data Studio，则卸载方式与删除任何 Windows 应用程序相同。

如果使用 .zip 或其他存档安装了 Azure Data Studio，则删除文件。

## <a name="next-steps"></a>后续步骤

请参阅以下快速入门以开始使用：

- [什么是 Azure Data Studio](what-is-azure-data-studio.md)
- [Azure Data Studio 发行说明](release-notes-azure-data-studio.md)
- [连接并查询 SQL Server](quickstart-sql-server.md)
- [连接并查询 Azure SQL 数据库](quickstart-sql-database.md)
- [连接和查询 Azure Synapse Analytics](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Microsoft 隐私声明](https://go.microsoft.com/fwlink/?LinkId=521839)和[使用情况数据收集](usage-data-collection.md)。
