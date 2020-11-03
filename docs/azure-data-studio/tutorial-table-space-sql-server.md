---
title: 启用表空间使用情况示例见解小组件
description: 本教程演示如何在 Azure Data Studio 数据库仪表板上启用表空间使用情况示例见解小组件。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/10/2019
ms.openlocfilehash: d0dd2b33c5f37b58e1442c4ba4cef2a4f38f293c
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439271"
---
# <a name="tutorial-enable-the-table-space-usage-sample-insight-widget-using-azure-data-studio"></a>教程：使用 Azure Data Studio 启用表空间使用情况示例见解小组件

本教程演示如何在数据库仪表板上启用见解小组件，从而提供有关数据库中所有表的空间使用情况的一览式视图。 在本教程中，你将学习如何执行以下操作：

> [!div class="checklist"]
> * 使用内置见解小组件示例快速打开见解小组件
> * 查看表空间使用情况的详细信息
> * 在见解图上筛选数据和查看标签详细信息

## <a name="prerequisites"></a>先决条件

本教程需要使用 SQL Server 或 Azure SQL 数据库 TutorialDB。 若要创建 TutorialDB 数据库，请完成以下其中一项快速入门：

* [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 SQL Server](quickstart-sql-server.md)
* [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 Azure SQL 数据库](quickstart-sql-database.md)

## <a name="turn-on-a-management-insight-on-azure-data-studios-database-dashboard"></a>在 Azure Data Studio 的数据库仪表板上打开管理见解

Azure Data Studio 具有内置的示例小组件，用于监视数据库中的表所使用的空间。

1. 按“Ctrl+Shift+P”打开“命令面板”，以打开“用户设置”。

2. 在搜索框中键入“设置”，然后选择“首选项:打开用户设置”。

3. 在“设置搜索”输入框中键入“仪表板”，然后找到“dashboard.database.widgets”。

4. 自定义“dashboard.database.widgets”设置时，需编辑“用户设置”部分中的“dashboard.database.widgets”条目  。

   ![屏幕截图显示了“用户设置”部分，其中突出显示了“仪表板”>“数据库小组件”部分。](media/tutorial-table-space-sql-server/search-settings.png)

   如果“用户设置”部分中没有“dashboard.database.widgets”，请将鼠标悬停在“默认设置”列中的“dashboard.database.widgets”文本上，单击显示在文本左侧的齿轮图标，然后单击“设置 JSON 时复制”  。 如果弹出窗口显示“在设置中替换”，请勿单击它！ 转到右侧的“用户设置”列，找到“dashboard.database.widgets”部分，然后进入下一步 。

5. 在“dashboard.database.widgets”部分中，添加以下行：

   ```json
        {
            "name": "Space Used by Tables",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "table-space-db-insight": null
            }
        },
    ```

   “dashboard.database.widgets”部分看起来应类似于下图：

    ![屏幕截图显示了 settings.json 文件，其中包含具有 dashboard.database.widgets 数组的第一个对象。](./media/tutorial-table-space-sql-server/insight-table-space.png)

6. 按“Ctrl+S”保存设置。

7. 右键单击“TutorialDB”打开数据库仪表板，然后单击“管理” 。

8. 查看 *表空间* 见解小组件，如下图所示：

   ![小组件](./media/tutorial-table-space-sql-server/insight-table-space-result.png)

## <a name="working-with-the-insight-chart"></a>使用见解图

Azure Data Studio 的见解图提供筛选和鼠标悬停详细信息。 尝试执行以下步骤：

1. 单击并切换图表上的 *row_count* 图例。 当你打开或关闭图例时，Azure Data Studio 会显示和隐藏数据系列。

2. 将鼠标指针悬停在图表上方。 Azure Data Studio 会显示有关数据系列标签及其值的详细信息，如以下屏幕截图所示。

   ![图表切换和图例](./media/tutorial-table-space-sql-server/insight-table-space-toggle.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> * 使用内置见解小组件示例快速打开见解小组件。
> * 查看表空间使用情况的详细信息。
> * 在见解图上筛选数据和查看标签详细信息

若要了解如何生成自定义见解小组件，请完成下一教程：

> [!div class="nextstepaction"]
> [生成自定义见解小组件](tutorial-build-custom-insight-sql-server.md)
