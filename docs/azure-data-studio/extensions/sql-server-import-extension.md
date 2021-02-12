---
title: SQL Server 导入扩展
description: 了解如何安装和使用 Azure Data Studio 的 SQL Server 导入扩展，这是将 .txt 和 .csv 文件转换为 SQL 表的向导。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: a1a4c9a83aca18ef41960f3c497b75f6de210109
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048377"
---
# <a name="sql-server-import-extension"></a>SQL Server 导入扩展

SQL Server 导入扩展会将 .txt 和 .csv 文件转换为 SQL 表。 此向导使用名为 [Program Synthesis using Examples (PROSE)](https://microsoft.github.io/prose/) 的 Microsoft 研究框架，以使用最少的用户输入来智能分析文件。 这是一种功能强大的框架，适用于数据整理，它是在 Microsoft Excel 中实现快速填充的相同技术

若要了解有关此功能的 SSMS 版本的详细信息，请阅读[本文](../../relational-databases/import-export/import-flat-file-wizard.md)。

## <a name="install-the-sql-server-import-extension"></a>安装 SQL Server 导入扩展

1. 若要打开扩展管理器并访问可用扩展，请选择扩展图标，或在“视图”菜单中选择“扩展”   。
2. 选择某个可用扩展以查看其详细信息。

   ![导入扩展管理器](media/sql-server-import-extension/import-wizard-install.png)

3. 选择所需的扩展并“安装”它  。
4. 选择“重新加载”以启用扩展（仅在第一次安装扩展时是必需的）  。

## <a name="start-import-wizard"></a>启动导入向导

1. 若要启动 SQL Server 导入，请先在“服务器”选项卡中建立与服务器的连接。
2. 建立连接后，向下钻取到想要将文件导入 SQL 表的目标数据库。
3. 右键单击数据库，然后选择“导入向导”。

    ![导入向导](media/sql-server-import-extension/open-import-wizard.png)

## <a name="importing-a-file"></a>导入文件

1. 右键单击以启动向导时，服务器和数据库已自动填充。 如果存在其他活动连接，则可以在下拉列表中进行选择。 

    选择“浏览”以选择文件。 它应该根据文件名自动填充表名，但也可以自行更改。

    默认情况下，架构将是 dbo，但可进行更改。 选择“下一步”继续。

    ![输入文件](media/sql-server-import-extension/import-wizard-input-file.png)

2. 向导将根据前 50 行生成预览。 除验证数据是否准确外，此页上没有其他操作。 选择“下一步”继续。

    ![预览数据](media/sql-server-import-extension/import-wizard-preview-data.png)

3. 在此页上，可以更改列名、数据类型、是否为主键或是否允许 null 值。 可以根据需要执行任意数量的更改。 选择“导入数据”继续操作。

    ![修改列](media/sql-server-import-extension/import-wizard-modify-columns.png)

4. 此页提供所选操作的摘要。 还可以查看表是否成功插入。

    如果需要进行更改，可选择“完成，上一步”，或者单击“导入新文件”以快速导入另一个文件 。

    ![总结](media/sql-server-import-extension/import-wizard-summary.png)

5. 通过刷新目标数据库或对表名运行 SELECT 查询来验证表是否已成功导入。

## <a name="next-steps"></a>后续步骤

- 若要了解有关导入向导的详细信息，请阅读[博客文章](https://cloudblogs.microsoft.com/sqlserver/2018/08/30/the-august-release-of-sql-operations-studio-is-now-available/)。
- 若要了解有关 PROSE 的详细信息，请阅读[文档](https://microsoft.github.io/prose/)。