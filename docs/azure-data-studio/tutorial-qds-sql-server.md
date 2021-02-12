---
title: 启用五个最慢查询示例小组件
description: 本教程演示如何在数据库仪表板上启用五个最慢查询示例小组件。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu; sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 06/01/2020
ms.openlocfilehash: 6b8632f9ff5c07cb23a296b98a5933639abee4d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048137"
---
# <a name="tutorial-add-the-five-slowest-queries-sample-widget-to-the-database-dashboard"></a>教程：将 *五个最慢查询* 示例小组件添加到数据库仪表板

本教程演示了将 Azure Data Studio 内置示例小组件之一添加到数据库仪表板以快速查看数据库的五个最慢查询的过程。 还将学习如何使用 Azure Data Studio 功能查看慢速查询和查询计划的详细信息。 在本教程中，你将学习如何执行以下操作：

> [!div class="checklist"]
> * 在数据库上启用查询存储
> * 向数据库仪表板添加预生成的见解小组件
> * 查看有关数据库最慢查询的详细信息
> * 查看慢速查询的查询执行计划

Azure Data Studio 包含几个现成的见解小组件。 本教程介绍如何添加 *query-data-store-db-insight* 小组件，但所有小组件的添加步骤都基本相同。

## <a name="prerequisites"></a>先决条件

本教程需要使用 SQL Server 或 Azure SQL 数据库 TutorialDB。 若要创建 TutorialDB 数据库，请完成以下其中一项快速入门：

* [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 SQL Server](quickstart-sql-server.md)

* [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 Azure SQL 数据库](quickstart-sql-database.md)

## <a name="turn-on-query-store-for-your-database"></a>为数据库启用查询存储

本示例中的小组件需要启用 *查询存储*。

1. 右键单击“TutorialDB”数据库（在“服务器”侧栏中），然后选择“新建查询”  。

2. 在查询编辑器中粘贴以下 Transact-SQL (T-SQL) 语句，然后单击“运行”：

   ```sql
    ALTER DATABASE TutorialDB SET QUERY_STORE = ON
   ```

## <a name="add-the-slow-queries-widget-to-your-database-dashboard"></a>将慢速查询小组件添加到数据库仪表板

若要将 *慢速查询小组件* 添加到仪表板，请编辑 *用户设置* 文件中的 *dashboard.database.widgets* 设置。

1. 按“Ctrl+Shift+P”打开“命令面板”，以打开“用户设置”。

2. 在搜索框中键入“设置”，然后选择“首选项:打开用户设置”。

   ![打开用户设置命令](./media/tutorial-qds-sql-server/open-user-settings.png)

3. 在设置搜索框中键入“仪表板”，找到“dashboard.database.widgets”，然后单击“在 settings.json 中编辑”。

   ![搜索设置](./media/tutorial-qds-sql-server/search-settings.png)

4. 在 settings.json 中，添加以下代码：

   ```json
   "dashboard.database.widgets": [
       {
           "name": "slow queries widget",
           "gridItemConfig": {
               "sizex": 2,
               "sizey": 1
           },
           "widget": {
               "query-data-store-db-insight": null
           }
       },
       {
           "name": "Tasks",
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 1
           },
           "widget": {
               "tasks-widget": {}
           }
       },
       {
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 2
           },
           "widget": {
               "explorer-widget": {}
           }
       }
   ]
   ```

5. 按“Ctrl+S”保存修改后的“用户设置” 。

6. 通过导航到“服务器”侧栏中的“TutorialDB”打开 *数据库仪表板*，右键单击并选择“管理”  。

   ![打开仪表板](./media/tutorial-qds-sql-server/insight-open-dashboard.png)

7. 见解小组件显示在仪表板上：

   ![QDS 小组件](./media/tutorial-qds-sql-server/insight-qds-result.png)

## <a name="view-insight-details-for-more-information"></a>查看见解详细信息以了解更多信息

1. 若要查看见解小组件的其他信息，请单击右上角的省略号 ( **...** )，然后选择“显示详细信息”。

2. 若要显示某个项目的更多详细信息，请选择“图表数据”列表中的任意项目。

   ![见解详细信息对话框](./media/tutorial-qds-sql-server/insight-details-dialog.png)

3. 关闭“见解”窗格。

## <a name="view-the-query-plan"></a>查看查询计划

1. 右键单击“TutorialDB”数据库并选择“管理”

2. 在“慢查询小组件”中，若要查看见解小组件的其他信息，请单击右上角的省略号 (...)，然后选择“运行查询”。

    ![运行查询](media/tutorial-qds-sql-server/run-query.png)

3. 现在，应该会看到一个包含结果的新查询窗口。

    ![运行查询结果](media/tutorial-qds-sql-server/run-query-results.png)

4. 单击“说明”。

   ![见解 QDS 说明](./media/tutorial-qds-sql-server/insight-qds-explain.png)

5. 查看查询的执行计划：

   ![显示查询的执行计划的屏幕截图。](./media/tutorial-qds-sql-server/showplan.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> * 在数据库上启用查询存储
> * 向数据库仪表板添加见解小组件
> * 查看有关数据库最慢查询的详细信息
> * 查看慢速查询的查询执行计划

若要了解如何启用“表空间使用情况”示例见解，请完成下一教程：

> [!div class="nextstepaction"]
> [启用表空间示例见解小组件](tutorial-table-space-sql-server.md)