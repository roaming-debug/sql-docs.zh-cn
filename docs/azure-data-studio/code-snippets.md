---
title: 创建可重用代码片段
description: 了解如何创建和使用 Azure Data Studio SQL 代码片段，以便轻松创建数据库和数据库对象。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: e2ea3a950a548cae6ad23f40055acc09b2e9534f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048687"
---
# <a name="create-and-use-code-snippets-to-quickly-create-transact-sql-t-sql-scripts-in-azure-data-studio"></a>创建和使用代码片段，以便在 Azure Data Studio 中快速创建 Transact-SQL (T-SQL) 脚本

Azure Data Studio 中的代码片段是可轻松创建数据库和数据库对象的模板。 

Azure Data Studio 提供几个 T-SQL 片段，以帮助快速生成正确的语法。 

还可以创建用户定义的代码片段。

## <a name="using-built-in-t-sql-code-snippets"></a>使用内置 T-SQL 代码片段

1. 若要访问可用片段，请在查询编辑器中键入 sql 以打开列表：

   ![代码段](media/code-snippets/sql-snippets.png)

2. 选择要使用的片段，生成 T-SQL 脚本。 例如，选择 sqlCreateTable：

   ![创建表片段](media/code-snippets/create-table.png)

3. 使用特定值更新突出显示的字段。 例如，将 TableName 和 Schema 替换为数据库的值 ：

   ![代码片段中的表](media/code-snippets/table-from-snippet.png)

   如果要更改的字段不再突出显示（在编辑器周围移动光标时会出现这种情况），请右键单击要更改的字，然后选择“更改所有匹配项”：

   ![全部更改](media/code-snippets/change-all.png)

4. 更新或添加所选片段所需的任何其他 T-SQL。 例如，更新 Column1、Column2，并添加更多列 。

## <a name="creating-sql-code-snippets"></a>创建 SQL 代码片段

可以定义自己的片段。 打开需要编辑的 SQL 片段文件：

1. 请打开“命令面板”(Shift+Ctrl+P)，并键入 snip，然后选择“先决条件: 打开用户片段”：

   ![用户代码片段](media/code-snippets/user-snippets.png)

2. 选择 SQL：

   > [!NOTE]
   > Azure Data Studio 从 Visual Studio Code 继承其代码片段功能，本文专门介绍了如何使用 SQL 片段。 有关详细信息，请参阅 Visual Studio Code 文档中的[创建自己的片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)。 

   ![选择 SQL](media/code-snippets/select-sql.png)

3. 将以下代码粘贴到 sql.json：

    ```sql
    {
     "Select top 5": {
    "prefix": "sqlSelectTop5",
    "body": "SELECT TOP 5 * FROM ${1:TableName}",
    "description": "User-defined snippet example 1"
    },
    "Create Table snippet":{
    "prefix": "sqlCreateTable2",
    "body": [
    "-- Create a new table called '${1:TableName}' in schema '${2:SchemaName}'",
    "-- Drop the table if it already exists",
    "IF OBJECT_ID('$2.$1', 'U') IS NOT NULL",
    "DROP TABLE $2.$1",
    "GO",
    "-- Create the table in the specified schema",
    "CREATE TABLE $2.$1",
    "(",
    "$1Id INT NOT NULL PRIMARY KEY, -- primary key column",
    "Column1 [NVARCHAR](50) NOT NULL,",
    "Column2 [NVARCHAR](50) NOT NULL",
    "-- specify more columns here",
    ");",
    "GO"
    ],
       "description": "User-defined snippet example 2"
       }
       }
    ```

4. 保存 sql json 文件。

5. 单击 Ctrl+N，打开新的查询编辑器窗口。

6. 键入 sql，将看到刚刚添加的两个用户片段：sqlCreateTable2 和 sqlSelectTop5 。

选择其中一个新片段，并测试运行!

## <a name="next-steps"></a>后续步骤

有关 SQL 编辑器的信息，请参阅[代码编辑器教程](tutorial-sql-editor.md)。
