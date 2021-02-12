---
title: 备份和还原数据库
description: 请按照本教程的说明操作，了解如何使用 Azure Data Studio 备份和还原数据库。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 11/04/2019
ms.openlocfilehash: 851fbdf950656f9edfba76e3d5a0aae54e8e3d99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048167"
---
# <a name="tutorial-back-up-and-restore-databases-using-azure-data-studio"></a>教程：使用 Azure Data Studio 备份和还原数据库

在本教程中，将了解如何使用 Azure Data Studio 执行以下操作：
> [!div class="checklist"]
> * 备份数据库。
> * 查看备份状态。
> * 生成用于执行备份的脚本。
> * 还原数据库。
> * 查看还原任务的状态。

## <a name="prerequisites"></a>先决条件

本教程需要 SQL Server TutorialDB。 若要创建 TutorialDB 数据库，请完成以下快速入门：

* [使用 Azure Data Studio 连接和查询 SQL Server](quickstart-sql-server.md)

本教程需要连接到 SQL Server 数据库。 Azure SQL 数据库已自动备份，因此 Azure Data Studio 不执行 Azure SQL 数据库备份和还原。 有关详细信息，请参阅[了解 SQL 数据库自动备份](/azure/sql-database/sql-database-automated-backups)。

## <a name="back-up-a-database"></a>备份数据库

1. 打开“服务器”边栏，以打开“TutorialDB 数据库”仪表板。 然后按 Ctrl+G，展开“数据库”，右键单击“TutorialDB”，然后选择“管理”   。

1. 通过在“任务”小组件上选择“备份”，打开“备份数据库”对话框  。

   ![显示“任务”小组件的屏幕截图。](./media/tutorial-backup-restore-sql-server/tasks.png)

1. 本教程使用默认备份选项，因此请选择“备份”。

   ![显示“备份”对话框的屏幕截图。](./media/tutorial-backup-restore-sql-server/backup-dialog.png)

选择“备份”后，“备份数据库”对话框将会消失，备份过程开始 。

## <a name="view-the-backup-status-and-the-backup-script"></a>查看备份状态和备份脚本

1. “任务历史记录”窗格将出现，否则请按 CTRL+T 打开该窗格 。

   ![显示“任务历史记录”窗格的屏幕截图。](./media/tutorial-backup-restore-sql-server/task-history.png)

1. 要在编辑器中查看备份脚本，请右键单击“已成功备份的数据库”，然后选择“脚本” 。

   ![显示备份脚本的屏幕截图。](./media/tutorial-backup-restore-sql-server/task-script.png)

## <a name="restore-a-database-from-a-backup-file"></a>从备份文件中还原数据库

1. 按 Ctrl+G 打开“服务器”边栏 。 然后右键单击服务器，并选择“管理”。

1. 通过在“任务”小组件上选择“还原”打开“还原数据库”对话框  。

   ![显示任务还原的屏幕截图。](media/tutorial-backup-restore-sql-server/tasks-restore.png)

1. 在“还原来源”框中选择“备份文件” 。

1. 选择“备份文件路径”框中的省略号 (…)，然后选择 TutorialDB 的最新备份文件。

1. 在“目标”部分的“目标数据库”框中，输入“TutorialDB_Restored”以将备份文件还原到新数据库  。 然后选择“还原”。

   ![显示还原备份的屏幕截图。](./media/tutorial-backup-restore-sql-server/restore.png)

1. 要查看还原操作的状态，请按 Ctrl+T 打开“任务历史记录” 。

   ![显示历史记录任务还原的屏幕截图。](./media/tutorial-backup-restore-sql-server/task-history-restore.png)