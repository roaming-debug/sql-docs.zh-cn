---
title: 开发和部署 Linux 上的 SQL Server 数据库 | Microsoft Docs
description: 使用 Visual Studio 的 SQL Server 数据工具是一个强大的开发环境和数据库生命周期管理环境，适用于 Linux 上的 SQL Server。
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1e924704-e07c-4a8b-b243-8c1dd8cff0d3
ms.openlocfilehash: a7add65dbc62679a6a1183c7a3f6998de7a18cb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273088"
---
# <a name="use-visual-studio-to-create-databases-for-sql-server-on-linux"></a>使用 Visual Studio 为 Linux 上的 SQL Server 创建数据库

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server Data Tools (SSDT) 可将 Visual Studio 转变为一种强大的开发环境和数据库生命周期管理 (DLM) 环境，转变后的环境适用于 Linux 上的 SQL Server。 从源代码管理的项目开发、生成、测试和发布数据库。 就像开发应用程序代码一样。

## <a name="install-visual-studio-and-sql-server-data-tools"></a>安装 Visual Studio 和 SQL Server Data Tools

1. 如果 Windows 计算机上尚未安装 Visual Studio，请[下载并安装 Visual Studio](https://visualstudio.microsoft.com/downloads/)。 如果缺少 Visual Studio 许可证，可使用免费的 Visual Studio Community 版本，该版本是一款适用于学生、开放源代码和个体开发人员的全功能型 IDE。

2. 在 Visual Studio 的安装过程中，在“选择安装类型”选项下，选择“自定义”   。 点击“下一步” 

3. 依次选择“Microsoft SQL Server Data Tools”和“Git for Windows”，然后从功能选择列表中选择“适用于 Visual Studio 的 GitHub 扩展”    。

   <img src="./media/sql-server-linux-develop-use-ssdt/ssdt-setup.png" alt="ssdt setup" style="width: 400px;"/>

4. 继续完成 Visual Studio 安装。 这可能需要几分钟。

## <a name="upgrade-sql-server-data-tools-to-ssdt-170-rc-release"></a>将 SQL Server Data Tools 升级到 SSDT 17.0 RC 版本

SSDT 版本 17.0 RC 及更高版本支持 Linux 上的 SQL Server。

* [下载并安装 SSDT 17.0 RC2](https://go.microsoft.com/fwlink/?linkid=837939)。

## <a name="create-a-new-database-project-in-source-control"></a>在源代码管理中创建新数据库项目

1. 启动 Visual Studio。

2. 在“视图”菜单上，选择“团队资源管理器”   。 

3. 在“连接”页的“本地 Git 存储库”部分，单击“新建”    。

   <img src="./media/sql-server-linux-develop-use-ssdt/git-repository.png" alt="Screenshot of the Local Git Repository section with the New option called out." style="width: 300px;"/>

4. 单击“创建”。  创建本地 Git 存储库后，双击“SSDTRepo”  。

5. 单击“解决方案”部分的“新建”   。 选择“新建项目”对话框中“其他语言”节点下的“SQL Server”    。

   <img src="./media/sql-server-linux-develop-use-ssdt/new-project.png" alt="Screenshot of the Solutions section with the New option and SQL Server option called out." style="width: 480px;"/>

6. 键入 TutorialDB 作为名称，单击“确定”创建新数据库项目   。

## <a name="create-a-new-table-in-the-database-project"></a>在数据库项目中创建新表

1. 在“视图”菜单上，选择“解决方案资源管理器”   。

2. 右键单击“解决方案资源管理器”中的“TutorialDB”，打开数据库项目菜单  。

3. 选择“添加”中的“表”   。

   <img src="./media/sql-server-linux-develop-use-ssdt/create-table.png" alt="create table" style="width: 480px;"/>

4. 使用表设计器添加两列，即名称 `nvarchar(50)` 和位置 `nvarchar(50)`，如下图所示。 在设计器中添加列时，SSDT 将生成 `CREATE TABLE` 脚本。

   <img src="./media/sql-server-linux-develop-use-ssdt/add-columns.png" alt="Screenshot of the table designer with the Name and Location values called out." style="width: 480px;"/>

5. 保存 Table1.sql 文件  。

## <a name="build-and-validate-the-database"></a>生成和验证数据库

1. 在“TutorialDB”中打开数据库项目菜单，并选择“生成”   。 SSDT 将在项目中编译 .sql 源代码文件，并生成数据层应用程序包 (dacpac) 文件。 这适用于将数据库发布到 Linux 上的 SQL Server 实例。 

   <img src="./media/sql-server-linux-develop-use-ssdt/build.png" alt="Screenshot showing the TutorialDB with the Build option called out." style="width: 400px;"/>

2. 在 Visual Studio 的“输出”窗口中检查生成成功的消息  。 

## <a name="publish-the-database-to-sql-server-instance-on-linux"></a>将数据库发布到 Linux 上的 SQL Server 实例

1. 在“TutorialDB”中打开数据库项目菜单，并选择“发布”   。

2. 单击“编辑”，选择 Linux 上的 SQL Server 实例  。

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-dialog.png" alt="publish dialog" style="width: 480px;"/>

3. 在“连接”对话框中，键入 IP 地址或 Linux 上的 SQL Server 实例的主机名、用户名和密码。

   <img src="./media/sql-server-linux-develop-use-ssdt/connection-dialog.png" alt="connection dialog" style="width: 400px;"/>

4. 单击“发布”对话框上的“发布”按钮  。

5. 检查“Data Tools 操作”窗口中的发布状态  。

6. 单击“查看结果”或“查看脚本”，查看 Linux 上的 SQL Server 的数据库发布结果详细信息   。

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-result.png" alt="publish result" style="width: 480px;"/>

你已成功在 Linux 上的 SQL Server 实例上创建新数据库，并了解了使用源代码管理的数据库项目进行数据库开发的基本知识。

## <a name="next-steps"></a>后续步骤

如果不熟悉 T-SQL，请参阅[教程：编写 Transact-SQL 语句](../t-sql/tutorial-writing-transact-sql-statements.md)。

有关使用 SQL Data Tools 开发数据库的详细信息，请参阅以下文章。

* [下载并安装 Visual Studio](https://www.visualstudio.com/downloads/)
* [下载并安装 SSDT](../ssdt/download-sql-server-data-tools-ssdt.md)
* [SSDT MSDN 文档](/previous-versions/sql/sql-server-data-tools/hh272686(v=vs.103))
* [教程：编写 Transact-SQL 语句](../t-sql/tutorial-writing-transact-sql-statements.md)
* [Transact-SQL 参考（数据库引擎）](../t-sql/language-reference.md)