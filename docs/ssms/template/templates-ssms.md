---
title: 在 SQL Server Management Studio 中使用模板
description: 了解 SQL Server Management Studio (SSMS) 中提供的预建 Transact-SQL (T-SQL) 模板。
keywords: SQL Server, SSMS, SQL Server Management Studio, 模板
author: MashaMSFT
ms.author: mathoma
ms.topic: tutorial
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
helpviewer_keywords:
- templates [SQL Server], SQL Server Management Studio
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: b3c5c26ddf8e343b2cf63bc0dde337056eb2fe74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017227"
---
# <a name="use-templates-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中使用模板

本教程将介绍 SQL Server Management Studio (SSMS) 中提供的预建 Transact-SQL (T-SQL) 模板。 在本文中，学习如何：

## <a name="prerequisites"></a>必备条件

要完成本教程，需要 SQL Server Management Studio 并访问 SQL Server。

* 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。

* 安装 [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。

## <a name="use-template-browser"></a>使用模板浏览器

本部分指导如何查找和使用“模板浏览器”。

1. 打开 SQL Server Management Studio。

2. 在“视图”菜单中选择“模板浏览器”(Ctrl+Alt+T)   ：

    ![打开“模板浏览器”](media/templates-ssms/templatebrowser.png)

    可在“模板浏览器”底部查看最近使用的模板。

3. 展开你感兴趣的节点。 右键单击该模板，然后选择“打开”  ：

    ![打开模板](media/templates-ssms/opentemplate.png)

    也可双击模板名称将其打开。

4. “新建查询”窗口随即打开。 已填充 T-SQL 脚本。

5. 根据需要修改模板，然后选择“执行”以运行查询  ：

    ![创建 DB 模板](media/templates-ssms/createdbtemplate.png)

## <a name="edit-an-existing-template"></a>编辑现有模板

还可以在“模板浏览器”中编辑现有模板。  

1. 在“模板浏览器”中，转到要使用的模板。

2. 右键单击该模板，然后选择“编辑”  ：

    ![编辑模板](media/templates-ssms/edittemplate.png)

3. 在打开的“查询”窗口中，进行要进行的更改。

4. 要保存模板，请选择“文件” > “保存”(Ctrl+S)。

5. 关闭查询窗口。

6. 重新打开模板。 应显示编辑内容。

## <a name="locate-templates-on-disk"></a>在磁盘上找到模板

当模板打开时，可以找到磁盘上的模板。

1. 在模板浏览器中，选择一个模板，然后选择“编辑”  。

2. 右键单击“查询标题”，然后选择“打开所在的文件夹”   。 资源管理器应打开磁盘上的模板存储位置： 

   ![磁盘上的模板](media/templates-ssms/templatesondisk.png)
  
## <a name="create-a-new-template"></a>创建新模板

也可在模板浏览器中创建新模板。 以下步骤介绍如何创建一个新文件夹，然后在该文件夹中创建新模板。 还可使用这些步骤在现有文件夹中创建自定义模板。 

1. 打开“模板浏览器”。

2. 右键单击“SQL Server 模板”，然后选择“新建” > “文件夹”。

3. 将此文件夹命名为“自定义模板”  ：

    ![创建自定义模板文件夹](media/templates-ssms/creatingcustomtemplate.png)

4. 右键单击新创建的“自定义模板”文件夹，然后选择“新建” > “模板”。 为模板输入名称：

    ![创建自定义模板](media/templates-ssms/createnewtemplate.png)

5. 右键单击创建的模板，然后选择“编辑”  。 “新建查询”窗口随即打开。

6. 输入要保存的 T-SQL 文本。

7. 在“文件”菜单上，选择“保存”   。

8. 关闭现有“查询窗口”然后打开新的自定义模板。

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些教程  和操作说明  文章可帮助你使用 SSMS 的各种功能。  这些文章教你如何管理 SSMS 组件，以及如何查找常用功能。

* [连接到实例和查询实例](../quickstarts/ssms-connect-query-sql-server.md)
* [脚本](../tutorials/scripting-ssms.md)
* [SSMS 配置](../tutorials/ssms-configuration.md)
* [使用 SSMS 的其他提示和技巧](../tutorials/ssms-tricks.md)