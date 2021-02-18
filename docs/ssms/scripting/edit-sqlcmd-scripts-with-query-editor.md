---
title: 使用查询编辑器编辑 SQLCMD 脚本
description: 当需要在同一脚本中处理 Windows 系统命令和 Transact-SQL 语句时，请使用 SQLCMD 脚本。 了解如何使用数据库引擎查询编辑器来编写和编辑 SQLCMD 脚本。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], SQLCMD scripts
- SQLCMD scripts
- modifying scripts
- Query Editor [Database Engine], SQLCMD scripts
- scripts [SQL Server], SQL Server Management Studio
ms.assetid: f77b866d-c330-47c9-9e74-0b8d8dff4b31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60c88bce82f6eb13d1441ded4cb815cecd0842ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354674"
---
# <a name="edit-sqlcmd-scripts-with-query-editor"></a>使用查询编辑器编辑 SQLCMD 脚本

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中的 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 查询编辑器，可以将查询作为 SQLCMD 脚本来进行编写和编辑。 当必须处理同一脚本中的 Windows 系统命令和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时，使用的是 SQLCMD 脚本。  
  
## <a name="sqlcmd-mode"></a>SQLCMD 模式  
 若要使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器编写或编辑 SQLCMD 脚本，您必须启用 SQLCMD 脚本撰写模式。 默认情况下，查询编辑器中将不启用 SQLCMD 模式。 可以通过在工具栏中单击 **“SQLCMD 模式”** 图标或从 **“查询”** 菜单中选择 **“SQLCMD 模式”** 来启用脚本撰写模式。  
  
> [!NOTE]  
>  启用 SQLCMD 模式将关闭 IntelliSense 和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器中的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 调试器。  
  
 查询编辑器中的 SQLCMD 脚本可以使用所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本能够使用的功能。 这些功能包括：  
  
-   颜色编码  
  
-   执行脚本  
  
-   源代码管理  
  
-   分析脚本  
  
-   Showplan  
  
## <a name="enable-sqlcmd-scripting-in-query-editor"></a>在查询编辑器中启用 SQLCMD 脚本撰写  
 若要为活动的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口启用 SQLCMD 脚本撰写，请使用以下步骤。  
  
#### <a name="to-switch-a-database-engine-query-editor-window-to-sqlcmd-mode"></a>将数据库引擎查询编辑器窗口切换到 SQLCMD 模式  
  
1.  在“对象资源管理器”中，右键单击服务器，再单击“新建查询”以打开新的[!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口。  
  
2.  在 **“查询”** 菜单中，单击 **“SQLCMD 模式”** 。  
  
     查询编辑器将在其上下文中执行 **sqlcmd** 语句。  
  
3.  在 **“SQL 编辑器”** 工具栏的 **“可用数据库”** 列表中，选择 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]。  
  
4.  在“查询编辑器”窗口中，键入以下两个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和 `!!DIR` **sqlcmd** 语句：  
  
    ```  
    SELECT DISTINCT Type FROM Sales.SpecialOffer;  
    GO  
    !!DIR  
    GO  
    SELECT ProductCategoryID, Name FROM Production.ProductCategory;  
    GO  
    ```  
  
5.  按 F5 执行混合了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和 MS-DOS 语句的整个部分。  
  
     请注意第一个和第三个语句产生的两个 SQL 结果窗格。  
  
6.  在 **“结果”** 窗格中，单击 **“消息”** 选项卡可以查看所有三个语句产生的消息：  
  
    -   （6 行受影响）  
  
    -   \<The directory information>  
  
    -   (4 row(s) affected)  
  
> [!IMPORTANT]  
>  从命令行执行 **sqlcmd** 实用工具时，该工具允许与操作系统完全交互。 在 **“SQLCMD 模式”** 下使用查询编辑器时，必须注意不要执行交互语句。 查询编辑器无法响应操作系统提示。  
  
 有关如何运行 SQLCMD 的详细信息，请参阅 [sqlcmd Utility](../../tools/sqlcmd-utility.md)或学习 SQLCMD 教程。  
  
## <a name="enable-sqlcmd-scripting-by-default"></a>默认启用 SQLCMD 脚本撰写  
 若要默认启用 SQLCMD 脚本撰写，请在 **“工具”** 菜单中选择 **“选项”**，展开 **“查询执行”** 和 **SQL Server**，单击 **“常规”** 页面，然后选中 **“默认情况下，在 SQLCMD 模式下打开新查询”** 框。  
  
## <a name="writing-and-editing-sqlcmd-scripts"></a>编写和编辑 SQLCMD 脚本  
 启用脚本撰写模式后，可以编写 SQLCMD 命令和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 下列规则适用：  
  
-   SQLCMD 命令必须是一行中的第一个语句。  
  
-   每行只允许使用一个 SQLCMD 命令。  
  
-   SQLCMD 命令前可以添加注释或空格。  
  
-   注释字符内的 SQLCMD 命令不会执行。  
  
-   单行注释字符是两个连字符 (`--)` ，必须位于一行的开头。  
  
-   操作系统命令前面必须具有两个感叹号 (`!!`)。 两个感叹号命令使其后的语句通过 `cmd.exe` 命令处理器来执行。 `!!` 后面的文本作为一个参数传递到 `cmd.exe`，因此最终的命令行将如此执行： `"%SystemRoot%\system32\cmd.exe /c <text after !!>"`。  
  
-   为了清楚地区分 SQLCMD 命令和 [!INCLUDE[tsql](../../includes/tsql-md.md)]，所有的 SQLCMD 命令都需要在前面添加冒号 (`:`)。  
  
-   `GO` 命令可以直接使用，无需在其前面加 `!!:`  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器支持环境变量和定义为 SQLCMD 脚本的一部分的变量，但不支持内置的 SQLCMD 或 **osql** 变量。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 处理的 SQLCMD 变量区分大小写。 例如，PRINT '$(COMPUTERNAME)' 会生成正确的结果，而 PRINT '$(ComputerName)' 则返回错误。  
  
> [!CAUTION]
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在常规模式和 SQLCMD 模式下，使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]SqlClient 来执行。 从命令行运行时，SQLCMD 将使用 OLE DB 访问接口。 由于可以应用不同的默认选项，因此在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] SQLCMD 模式下以及在 SQLCMD 实用工具中执行相同的查询时，可能会获得不同的行为。  
  
## <a name="supported-sqlcmd-syntax"></a>支持的 SQLCMD 语法  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器支持以下 SQLCMD 脚本关键字：  
  
 `[!!:]GO[count]`  
  
 `!! <command>`  
  
 `:exit(statement)`  
  
 `:Quit`  
  
 `:r <filename>`  
  
 `:setvar <var> <value>`  
  
 `:connect server[\instance] [-l login_timeout] [-U user [-P password]]`  
  
 `:on error [ignore|exit]`  
  
 `:error <filename>|stderr|stdout`  
  
 `:out <filename>|stderr|stdout`  
  
> [!NOTE]  
>  对于 `:error` 和 `:out`， `stderr` 和 `stdout` 将向消息选项卡发送输出。  
  
 查询编辑器不支持上面未列出的 SQLCMD 命令。 执行包含不支持的 SQLCMD 关键字的脚本时，查询编辑器会为每个不支持的关键字向目标发送一条“忽略命令 *\<ignored command*>”消息。 脚本将成功执行，但同时忽略不支持的命令。  
  
> [!CAUTION]  
>  因为不是从命令行启动 SQLCMD，所以在 SQLCMD 模式下运行查询编辑器时会有一些限制。 不能传入命令行参数（如变量），而且，由于查询编辑器无法响应操作系统提示，因此必须注意不要执行交互语句。  
  
## <a name="color-coding-in-sqlcmd-scripts"></a>SQLCMD 脚本中的颜色编码  
 启用 SQLCMD 脚本撰写后，脚本将进行颜色编码。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 关键字的颜色编码将保持不变。 SQLCMD 命令用阴影背景来表示。  
  
## <a name="example"></a>示例  
 下面的示例使用 **sqlcmd** 语句创建名为 testoutput.txt 的输出文件，并将两个 [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT 语句与一个操作系统命令一起执行（以输出当前目录）。 结果文件包含 `DIR` 语句的消息输出，后面跟有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的结果输出。  
  
```  
:out C:\testoutput.txt  
SELECT @@VERSION As 'Server Version'  
!!DIR  
!!:GO  
SELECT @@SERVERNAME AS 'Server Name'  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)  
  
  
