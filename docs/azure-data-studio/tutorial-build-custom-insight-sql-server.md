---
title: 生成自定义见解小组件
description: 本教程演示如何生成自定义见解小组件，并将其添加到 Azure Data Studio 中的数据库和服务器仪表板。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 08/26/2020
ms.openlocfilehash: bd4d3c12ea7a56ceddec0ad23f7276a4be4b9f40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039957"
---
# <a name="tutorial-build-a-custom-insight-widget"></a>教程：生成自定义见解小组件

本教程演示如何使用自己的见解查询来生成自定义见解小组件。

在本教程中，你将学习如何执行以下操作：
> [!div class="checklist"]
> * 运行自己的查询并在图表中进行查看
> * 从图表生成自定义见解小组件
> * 将图表添加到服务器或数据库仪表板
> * 将详细信息添加到自定义见解小组件

## <a name="prerequisites"></a>先决条件

本教程需要使用 SQL Server 或 Azure SQL 数据库 TutorialDB  。 若要创建 TutorialDB  数据库，请完成以下其中一项快速入门：

- [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 SQL Server](quickstart-sql-server.md)
- [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 Azure SQL 数据库](quickstart-sql-database.md)


## <a name="run-your-own-query-and-view-the-result-in-a-chart-view"></a>运行自己的查询并在图表视图中查看结果
在此步骤中，运行 sql 脚本以查询当前的活动会话。

1. 若要打开新编辑器，请按 Ctrl+N  。 

2. 将连接上下文更改为 TutorialDB  。

3. 将以下查询粘贴到查询编辑器中：

   ```sql
   SELECT count(session_id) as [Active Sessions]
   FROM sys.dm_exec_sessions
   WHERE status = 'running'
   ```

4. 将编辑器中的查询保存到 \*.sql 文件中。 对于本教程，请将脚本保存为 activeSession.sql  。

5. 若要执行此查询，请按 F5  。

6. 显示查询结果后，单击“以图表形式查看”，然后单击“图表查看器”选项卡   。

7. 将“图表类型”更改为“计数”   。 这些设置呈现计数图表。

## <a name="add-the-custom-insight-to-the-database-dashboard"></a>将自定义见解添加到数据库仪表板

1. 若要打开见解小组件配置，请单击“图表查看器”上的“创建见解”   ：

   ![配置](./media/tutorial-build-custom-insight-sql-server/create-insight.png)
   
2. 复制见解配置（JSON 数据）。 

3. 按“Ctrl+逗号”打开“用户设置”   。

4. 在“搜索设置”中键入“仪表板”   。

5. 对 dashboard.database.widgets 单击“编辑”   。

   ![仪表板设置](./media/tutorial-build-custom-insight-sql-server/dashboard-settings.png)

6. 将见解配置 JSON 粘贴到 dashboard.database.widgets 中  。 数据库仪表板设置如下所示：

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql"
                }
            }
        }
    ]
   ```

7. 保存“用户设置”文件，并打开“TutorialDB”数据库仪表板以查看活动会话小组件   ：

   ![activesession 见解仪表板](./media/tutorial-build-custom-insight-sql-server/insight-activesession-dashboard.png)

## <a name="add-details-to-custom-insight"></a>将详细信息添加到自定义见解

1. 若要打开新编辑器，请按 Ctrl+N  。

2. 将连接上下文更改为 TutorialDB  。

3. 将以下查询粘贴到查询编辑器中：

   ```sql
    SELECT session_id AS [SID], login_time AS [Login Time], host_name AS [Host Name], program_name AS [Program Name], login_name AS [Login Name]
    FROM sys.dm_exec_sessions
    WHERE status = 'running'
   ```

4. 将编辑器中的查询保存到 \*.sql 文件中。 对于本教程，请将脚本保存为 activeSessionDetail.sql  。

5. 按“Ctrl+逗号”打开“用户设置”   。

6. 编辑设置文件中现有的 dashboard.database.widgets 节点  ：

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql",
                    "details": {
                        "queryFile": "{your file folder}/activeSessionDetail.sql",
                        "label": "SID",
                        "value": "Login Name"
                    }
                }
            }
        }
    ]
   ```

7. 保存“用户设置”文件，并打开“TutorialDB”数据库仪表板   。 单击“我的小组件”旁边的省略号 (...) 按钮以显示详细信息  ：

    ![activesession 见解详细信息](./media/tutorial-build-custom-insight-sql-server/insight-activesession-detail.png)

## <a name="next-steps"></a>后续步骤
在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> * 运行自己的查询并在图表中进行查看
> * 从图表生成自定义见解小组件
> * 将图表添加到服务器或数据库仪表板
> * 将详细信息添加到自定义见解小组件

若要了解如何备份和还原数据库，请完成下一教程：

> [!div class="nextstepaction"]
> [备份和还原数据库](tutorial-backup-restore-sql-server.md)。
