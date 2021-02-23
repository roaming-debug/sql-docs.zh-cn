---
title: 使用 Visual Studio Code mssql 扩展
description: 为 Visual Studio Code 使用 mssql 扩展来编辑和运行用于 Linux 上 SQL Server 的 Transact-SQL 脚本。
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340538"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>使用 Visual Studio Code 创建并运行 Transact SQL 脚本

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

本文介绍如何为 Visual Studio Code 使用“mssql”扩展来开发 SQL Server 数据库。 由于 Visual Studio Code 是跨平台的，因此可以在 Linux、macOS 和 Windows 上使用“mssql”扩展。

## <a name="install-and-start-visual-studio-code"></a>安装并启动 Visual Studio Code

Visual Studio Code 是支持扩展的跨平台图形代码编辑器。

1. 在计算机上[下载并安装 Visual Studio Code](https://code.visualstudio.com/)。

2. 启动 Visual Studio Code。

    >[!NOTE]
    >如果通过 xrdp 远程桌面会话连接时 Visual Studio Code 无法启动，请参阅[使用 XRDP 连接时 VS Code 无法在 Ubuntu 上运行](https://github.com/Microsoft/vscode/issues/3451)。

## <a name="install-the-mssql-extension"></a>安装 mssql 扩展

[适用于 Visual Studio Code 的 mssql 扩展](https://aka.ms/mssql-marketplace)允许你连接到 SQL Server，使用 Transact-SQL (T-SQL) 进行查询，并查看结果。

1. 在 Visual Studio Code 中，选择“查看” > “命令面板”，或按“Ctrl”+“Shift”+“P”，或按“F1”打开“命令面板”      。

2. 在“命令面板”中，从下拉列表中选择“扩展: 安装扩展” 。

3. 在“扩展”窗格中，键入“mssql”。

4. 选择“SQL Server (mssql)”扩展，然后选择“安装”。

   ![安装 mssql 扩展](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. 安装完成后，选择“重新加载”以启用扩展。

## <a name="create-or-open-a-sql-file"></a>创建或打开 SQL 文件

当语言模式设置为“SQL”时，mssql 扩展将在代码编辑器中启用 mssql 命令和 T-SQL IntelliSense。

1. 选择“文件” > “新建文件”或按“Ctrl”+“N”   。 默认情况下，Visual Studio Code 将打开一个新的“纯文本”文件。 

2. 在下方状态栏上选择“纯文本”，或按“Ctrl”+“K” > “M”，然后从“语言”下拉列表中选择“SQL”。 

   ![SQL 语言模式](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > 如果这是你第一次使用该扩展，则该扩展会安装支持性的 SQL Server 工具。

如果打开一个文件扩展名为 .sql 的现有文件，语言模式会自动设置为 SQL。  

## <a name="connect-to-sql-server"></a>连接到 SQL Server

请按照以下步骤创建连接配置文件并连接到 SQL Server。

1. 按“Ctrl”+“Shift”+“P”或“F1”打开“命令面板”    。 

2. 键入 sql 可显示 mssql 命令，或键入 sqlcon，然后从下拉列表中选择“MS SQL: 连接” 。

   ![mssql 命令](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >代码编辑器中的焦点须位于 SQL 文件（例如创建的空 SQL 文件），才能执行 mssql 命令。

3. 选择“MS SQL: 管理连接配置文件”命令。

4. 然后选择“创建”为 SQL Server 创建新的连接配置文件。

5. 按照提示为新连接配置文件指定属性。 指定每个值后，按“Enter”继续。

   | 连接属性 | 说明 |
   |---|---|
   | **服务器名称或 ADO 连接字符串** | 指定 SQL Server 实例名称。 使用 localhost 连接到本地计算机上的 SQL Server 实例。 如果要连接到远程 SQL Server，请输入目标 SQL Server 的名称，或它的 IP 地址。 若要连接到 SQL Server 容器，请指定容器主机的 IP 地址。 如果需要指定端口，请使用逗号将其与名称分开。 例如，对于侦听端口 1401 的服务器，请输入 `<servername or IP>,1401`。<br/><br/>或者，可以在此处输入数据库的 ADO 连接字符串。 |
   | “数据库名称”（可选） | 要使用的数据库。 若要连接到默认数据库，请不要在此处指定数据库名称。 |
   | **身份验证类型** | 选择“集成”或“SQL 登录”。 |
   | **用户名** | 如果选择了“SQL 登录”，则输入拥有访问服务器上数据库权限的用户名。 |
   | **密码** | 输入指定用户的密码。 |
   | **保存密码** | 按“Enter”选择“是”并保存密码。 选择“否”，系统将在每次使用连接配置文件时提示输入密码。 |
   | “配置文件名称”（可选） | 键入连接配置文件的名称，例如 localhost 配置文件。 |

   输入所有值并选择“Enter”后，Visual Studio Code 将创建连接配置文件并连接到 SQL Server。

   > [!TIP]
   > 如果连接失败，请尝试通过 Visual Studio Code “输出”面板中的错误消息来诊断问题。 要打开“输出”面板，请选择“查看” > “输出”。 另外，请查看[连接故障排除建议](../../linux/sql-server-linux-troubleshooting-guide.md)。

6. 在下方的状态栏中验证连接。

   ![连接状态](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

作为前面步骤的替代方法，还可以在“用户设置”文件 (settings.json) 中创建和编辑连接配置文件。 若要打开设置文件，请选择“文件” > “首选项” > “设置”  。 有关详细信息，请参阅[管理连接配置文件](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles)。

## <a name="create-a-sql-database"></a>创建 SQL 数据库

1. 在先前启动的新 SQL 文件中，键入 sql 以显示可编辑的代码段的列表。

   ![SQL 代码段](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. 选择“sqlCreateDatabase”。

3. 在代码段中，键入 `TutorialDB` 以替换“DatabaseName”：

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

4. 按“Ctrl”+“Shift”+“E”执行 Transact - SQL 命令  。 在查询窗口中查看结果。

    ![创建数据库消息](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > 可以自定义 mssql 命令的快捷键。 请参阅[自定义快捷键](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts)。

## <a name="create-a-table"></a>创建表

1. 删除代码编辑器窗口中的内容。

2. 按“Ctrl”+“Shift”+“P”或“F1”打开“命令面板”    。

3. 键入 sql 可显示 mssql 命令，或键入 sqluse，然后选择“MS SQL: 使用数据库”命令 。

4. 选择新的“TutorialDB”数据库。

   ![使用数据库](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. 在代码编辑器中，键入 sql 以显示片段，选择“sqlCreate Table”后按“Enter”。

6. 在片段中，键入表名 `Employees`。

7. 按“Tab”键转到下一个字段，然后键入 `dbo` 作为架构名称。

8. 使用以下列替换列定义：

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. 按“Ctrl”+“Shift”+“E”可创建表  。

## <a name="insert-and-query"></a>插入和查询

1. 添加下列语句，将四行插入“Employees”表。

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   键入时，T-SQL IntelliSense 可帮助你完成语句：

   ![T-SQL IntelliSense](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > mssql 扩展还包含可帮助创建 INSERT 和 SELECT 语句的命令。 前面的示例中未使用这些命令。

2. 按“Ctrl”+“Shift”+“E”执行命令  。 将在“结果”窗口中显示两个结果集。

   ![结果](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>查看并保存结果

1. 选择“视图” > “编辑器布局” > “翻转布局”以切换为垂直拆分或水平拆分的布局  。

2. 选择“结果”和“消息”面板标头来折叠和展开面板。

   ![切换标头](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > 可以自定义 mssql 扩展的默认行为。 请参阅[自定义扩展选项](https://github.com/Microsoft/vscode-mssql/wiki/customize-options)。

3. 选择第二个结果网格上的最大化网格图标以放大这些结果。

   ![最大化网格](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > T-SQL 脚本产生两个或多个结果网格时，会出现最大化图标。

4. 右键单击网格，打开网格上下文菜单。

   ![上下文菜单](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. 选择“全选”。

6. 再次打开网格上下文菜单，选择“另存为 JSON”将结果保存到“.json”文件。

7. 为 JSON 文件指定文件名。

8. 验证 JSON 文件是否在 Visual Studio Code 中保存和打开。

   ![另存为 JSON](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

如果以后需要保存和运行 SQL 脚本，用于管理用途或更大的开发项目，请使用 .sql 扩展名保存脚本。

## <a name="next-steps"></a>后续步骤

如果不熟悉 T-SQL，请参阅[教程：编写 Transact-SQL 语句](../../t-sql/tutorial-writing-transact-sql-statements.md)和 [Transact-SQL 引用（数据库引擎）](../../t-sql/language-reference.md)。

有关使用 mssql 扩展或贡献相关内容的详细信息，请参阅 [mssql 扩展项目 Wiki](https://github.com/Microsoft/vscode-mssql/wiki)。

有关使用 Visual Studio Code 的详细信息，请参阅 [Visual Studio Code 文档](https://code.visualstudio.com/docs)。