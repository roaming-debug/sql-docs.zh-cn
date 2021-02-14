---
title: Azure Data Studio 中使用 SQL 内核的笔记本
description: 本教程介绍如何创建和运行 SQL Server 笔记本。
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray, alayu
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: d2d04be4d571b14445abfb638f070b3666ce9984
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040277"
---
# <a name="create-and-run-a-sql-server-notebook"></a>创建和运行 SQL Server 笔记本

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何使用 SQL Server 在 Azure Data Studio 中创建和运行笔记本。

## <a name="prerequisites"></a>先决条件

- [已安装 Azure Data Studio](../download-azure-data-studio.md)
- 已安装 SQL Server
  - [Windows](../../database-engine/install-windows/install-sql-server.md)
  - [Linux](../../linux/sql-server-linux-setup.md)

## <a name="create-a--notebook"></a>创建笔记本

以下步骤演示如何在 Azure Data Studio 中创建笔记本文件：

1. 在 Azure Data Studio 中，连接到你的 SQL Server。

2. 在“服务器”窗口中的“连接”下进行选择   。 然后选择“新建笔记本”  。

3. 等待要填充的 Kernel 和目标上下文（“附加到”）   。 确认“内核”设置为“SQL”，并设置 SQL Server 的“附加到”（在本示例中为 localhost）  。

   ![设置 Kernel 和“附加到”](media/notebooks-sql-kernel/set-kernel-and-attach-to.png)

可使用“文件”菜单中的“保存”或“另存为…”命令保存笔记本  。

若要打开笔记本，可以使用“文件”菜单中的“打开文件…”命令，选择“欢迎”页上的“打开文件”，或使用命令面板中的“文件：    打开”命令。

## <a name="change-the-sql-connection"></a>更改 SQL 连接

更改笔记本的 SQL 连接：

1. 选择笔记本工具栏中的“附加到”菜单，然后选择“更改连接”   。

   ![选择笔记本工具栏中的“附加到”菜单](./media/notebooks-sql-kernel/select-attach-to-1.png)

2. 现在你可以选择一个最近的连接服务器，也可以输入新的连接详细信息进行连接。

## <a name="run-a-code-cell"></a>运行代码单元格

可单击单元格左侧的“运行单元格”按钮（圆形黑色箭头），创建包含可就地运行的 SQL 代码的单元格。 单元格完成运行后，结果会显示在笔记本中。

例如：

1. 选择工具栏中的“+代码”命令，添加一个新的代码单元格  。

   ![笔记本工具栏](media/notebooks-guidance/notebook-toolbar.png)

2. 将以下示例复制并粘贴到单元格，然后单击“运行单元格”。 此示例将创建一个新的数据库。

   ```sql
   USE master
   GO

   -- Drop the database if it already exists
   IF  EXISTS (
           SELECT name
           FROM sys.databases
           WHERE name = N'TestNotebookDB'
      )
   DROP DATABASE TestNotebookDB
   GO

   -- Create the database
   CREATE DATABASE TestNotebookDB
   GO
   ```

   ![运行单元格](media/notebooks-sql-kernel/run-notebook-cell.png)

## <a name="save-the-result"></a>保存结果

如果运行返回结果的脚本，则可以使用结果上方显示的工具栏将结果保存为不同的格式。

- 另存为 CSV
- 另存为 Excel
- 另存为 JSON
- 另存为 XML

例如，下面的代码将返回结果 [PI](../../t-sql/functions/pi-transact-sql.md)。

```sql
SELECT PI() AS PI;
GO
```

![保存结果](media/notebooks-sql-kernel/run-notebook-cell-2.png)

## <a name="next-steps"></a>后续步骤

了解有关笔记本的详细信息：

- [如何使用 Azure Data Studio 中的笔记本](./notebooks-guidance.md)
- [创建和运行 Python 笔记本](././notebooks-python-kernel.md)
- [使用 Spark 运行示例笔记本](../../big-data-cluster/notebooks-tutorial-spark.md)