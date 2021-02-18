---
title: 安装 SQL Server 文档以脱机查看
description: 了解如何安装 SQL Server 2019、2017、2016、2014 和 2012 版本的脱机文档。 使用 SQL Server Management Studio (SSMS) 查看脱机内容。
ms.prod: sql
ms.technology: install
ms.topic: conceptual
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
author: markingmyname
ms.author: maghan
ms.date: 08/12/2020
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: ab5ee55bf91e1a9287f22584da83ca7112dadece
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336777"
---
# <a name="install-sql-server-documentation-to-view-offline-in-ssms"></a>安装 SQL Server 文档以在 SSMS 中进行脱机查看

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

本文介绍如何在 [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 中下载和查看 SQL Server 脱机内容。 下载脱机内容后，便可在没有 Internet 连接的情况下访问文档（尽管最初下载时还是需要 Internet 连接）。

SQL Server 2012 和更高版本都提供了脱机文档。 虽然可以[联机查看早期版本的内容](/previous-versions/sql/)，但在访问早期内容时，脱机方式非常便捷。

- [SQL Server 2016 及更高版本](#sql-server-2016-and-later-offline-content)
- [SQL Server 2014](#sql-server-2014-offline-content)
- [SQL Server 2012](#sql-server-2012-offline-content)

## <a name="sql-server-2016-and-later-offline-content"></a>SQL Server 2016 及更高版本的脱机内容

以下步骤说明如何加载 SQL Server 2016 及更高版本的脱机内容。

1. 在 SSMS 中，选择“帮助”菜单上的“添加和删除帮助内容”。

   ![添加和删除帮助内容](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   帮助查看器随即打开“管理内容”选项卡。

2. 如需查找 SQL Server 2016 和更高版本的最新帮助内容，请在“管理内容”选项卡下，选择“安装源”下的“联机”，然后在搜索栏中键入“sql server”。

   ![SQL Server 丛书搜索](../sql-server/media/sql-server-offline-documentation/sql-online-search.png)

   > [!Note]
   > “管理内容”选项卡上的“本地存储路径”显示了内容在本地计算机上的安装位置。 如需更改位置，请选择“移动”，在“到”字段中输入其他文件夹路径，然后选择“确定”  。 如果在更改本地存储路径后帮助安装失败，请关闭帮助查看器再重新打开。 确保本地存储路径中显示了新位置，然后重试安装。

3. 如需安装 SQL Server 2016 和更高版本的最新帮助内容，请选择要安装的每个内容包（丛书）旁的“添加”，然后选择右下方的“更新”。

   ![SQL Server 联机丛书的“添加”和“更新”](../sql-server/media/sql-server-offline-documentation/sql-add-update.png)

   > [!NOTE]
   > 如果帮助查看器在添加内容时冻结（挂起），请将 %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings 或 HlpViewer_VisualStudiox_en-US.settings 文件中的 Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" 行更改为将来的某个日期。 有关此问题的详细信息，请参阅 [《Visual Studio 帮助查看器冻结》](/visualstudio/welcome-to-visual-studio)。

4. 可以在左侧内容窗格下搜索“sql server 2016”，验证是否已加载 SQL Server 2016 和更高版本的内容。

   ![SQL Server 2016 丛书已自动更新](../sql-server/media/sql-server-offline-documentation/sql-2016-content.png)

## <a name="sql-server-2014-offline-content"></a>SQL Server 2014 脱机内容

> [!IMPORTANT]
> SQL 2014 Transact-SQL 内容只能脱机使用。

以下步骤说明如何加载 SQL Server 2014 的脱机内容。

1. 从下载中心下载[适用于防火墙和代理受限环境的 Microsoft SQL Server 2014 产品文档](https://www.microsoft.com/download/details.aspx?id=42557)内容，并将它保存到文件夹中。

2. 解压缩文件以查看 .msha 文件。

   ![SQL Server 2014 帮助文档安装程序文件](../sql-server/media/sql-server-offline-documentation/sql-2014-help-content-setup-msha.png)

3. 在 SSMS 中，选择“帮助”菜单上的“添加和删除帮助内容”。

   ![帮助查看器中的“添加和删除内容”](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   帮助查看器随即打开“管理内容”选项卡。

4. 如需安装最新的帮助内容，请选择“安装源”下的“磁盘”，然后选择省略号 (...)。

   ![帮助查看器上的“管理内容”>“磁盘”源](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > “管理内容”选项卡上的“本地存储路径”显示了内容在本地计算机上的位置。 如需更改位置，请选择“移动”，在“到”字段中输入其他文件夹路径，然后选择“确定”  。
   如果在更改本地存储路径后帮助安装失败，请关闭并重新打开 Help Viewer。 确保本地存储路径中显示了新位置，然后重试安装。

5. 找到你用于解压缩内容的文件夹。 选择文件夹中的“HelpContentSetup.msha”文件，然后选择“打开”。

   ![打开 SQL Server 2014 的“Help Content Setup.msha”文件](../sql-server/media/sql-server-offline-documentation/sql-2014-open-msha.png)

6. 在搜索栏中键入“sql server 2014”。 看到 SQL Server 2014 的内容后，选择要安装到帮助查看器的每个内容包（丛书）旁边的“添加”，然后选择“更新”。

   ![帮助查看器中的 SQL Server 2014 丛书搜索屏幕](../sql-server/media/sql-server-offline-documentation/sql-2014-search.png)

   ![帮助查看器中的 SQL Server 2014 丛书添加和更新操作](../sql-server/media/sql-server-offline-documentation/sql-2014-add-update.png)

    > [!NOTE]
    > 如果帮助查看器在添加内容时冻结（挂起），请将 %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings 或 HlpViewer_VisualStudiox_en-US.settings 文件中的 Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" 行更改为将来的某个日期。 有关此问题的详细信息，请参阅 [《Visual Studio 帮助查看器冻结》](/visualstudio/welcome-to-visual-studio)。

7. 可以在左侧的内容窗格下搜索“sql server 2014”，验证是否已安装 SQL Server 2014 内容。

   ![SQL Server 2014 丛书已自动更新](../sql-server/media/sql-server-offline-documentation/sql-2014-content.png)

## <a name="sql-server-2012-offline-content"></a>SQL Server 2012 脱机内容

以下步骤说明如何加载 SQL Server 2012 的脱机内容

1. 从下载中心下载[适用于防火墙和代理受限环境的 Microsoft SQL Server 2012 产品文档](https://www.microsoft.com/download/details.aspx?id=35750)内容，并将它保存到文件夹中。

2. 解压缩文件以查看 .msha 文件。

   ![SQL Server 2012 帮助内容安装程序文件](../sql-server/media/sql-server-offline-documentation/sql-2012-help-content-setup-msha.png)

3. 在 SSMS 中，选择“帮助”菜单上的“添加和删除帮助内容”。

   ![帮助查看器中的“添加和删除内容”](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   帮助查看器随即打开“管理内容”选项卡。

4. 如需安装最新的帮助内容，请选择“安装源”下的“磁盘”，然后选择省略号 (...)。

   ![帮助查看器上的“管理内容”>“磁盘”源](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > “管理内容”选项卡上的“本地存储路径”显示了内容在本地计算机上的位置。 如需更改位置，请选择“移动”，在“到”字段中输入其他文件夹路径，然后选择“确定”  。
   如果在更改本地存储路径后帮助安装失败，请关闭并重新打开 Help Viewer。 确保本地存储路径中显示了新位置，然后重试安装。

5. 找到你用于解压缩内容的文件夹。 选择文件夹中的“HelpContentSetup.msha”文件，然后选择“打开”。

   ![打开 SQL Server 2012 的“Help Content Setup.msha”文件](../sql-server/media/sql-server-offline-documentation/sql-2012-open-msha.png)

6. 在搜索栏中键入“sql server 2012”。 看到 SQL Server 2012 的内容后，选择要安装到帮助查看器的每个内容包（丛书）旁边的“添加”，然后选择“更新”。

   ![帮助查看器中的 SQL Server 2012 丛书搜索屏幕](../sql-server/media/sql-server-offline-documentation/sql-2012-search.png)

   ![帮助查看器中的 SQL Server 2012 丛书添加和更新操作](../sql-server/media/sql-server-offline-documentation/sql-2012-add-update.png)

    > [!NOTE]
    > 如果帮助查看器在添加内容时冻结（挂起），请将 %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings 或 HlpViewer_VisualStudiox_en-US.settings 文件中的 Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" 行更改为将来的某个日期。 有关此问题的详细信息，请参阅 [《Visual Studio 帮助查看器冻结》](/visualstudio/welcome-to-visual-studio)。

7. 可以在左侧的内容窗格下搜索“sql server 2012”，验证是否已加载 SQL Server 2012 内容。

   ![SQL Server 2012 文档已自动更新](../sql-server/media/sql-server-offline-documentation/sql-2012-content.png)

## <a name="view-offline-documentation"></a>查看脱机文档

可以使用最新版本的 [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 中的“帮助”菜单来查看 SQL Server 帮助内容。

### <a name="view-offline-help-content-in-ssms"></a>在 SSMS 中查看脱机帮助内容

如需在 SSMS 中查看已安装的帮助内容，请从“帮助”菜单中选择“在帮助查看器中启动”，即可启动帮助查看器。

   ![在帮助查看器中启动](../sql-server/media/sql-server-offline-documentation/helpviewer-view-offline.png)  

帮助查看器随即打开“管理内容”选项卡，并在左窗格中显示已安装的帮助目录。 选择目录中的文章，便可在右窗格显示文章内容。

> [!Important]
> 如果看不到目录窗格，请在左侧边距上选择“目录”。 选择图钉图标，可使目录窗格保持打开状态。  

   ![显示内容的帮助查看器](../sql-server/media/sql-server-offline-documentation/view-offline-all.png)

## <a name="life-cycle-policy"></a>生命周期策略

请参阅“Microsoft 产品生命周期”，了解具体产品、服务或技术的支持情况：

- [Microsoft 生命周期策略](https://support.microsoft.com/lifecycle/selectindex)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>后续步骤

如需详细了解存档内容和帮助查看器，请参考下面的链接。

- [SQL Server 联机文档](../sql-server/index.yml?view=sql-server-2016&preserve-view=true)
- [SQL Server 2014 联机文档](/previous-versions/sql/2014)
- [SQL Server 早期版本联机文档](previous-versions-sql-server.md)
- [SQL 文档的版本控制系统](../sql-server/versioning-system-monikers-ui-sql-server.md?view=sql-server-2016&preserve-view=true)