---
title: SSMS 脚本对象
description: 为 SQL Server Management Studio (SSMS) 中找到的各种对象生成 Transact-SQL (T-SQL) 脚本。
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio, 脚本, 编写脚本
helpviewer_keywords:
- projects [SQL Server Management Studio], tutorials
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- solutions [SQL Server Management Studio], tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/13/2018
ms.openlocfilehash: 2607b029389d0a0d3f4b59e55e031b768baf4aac
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597254"
---
# <a name="script-objects-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中编写对象脚本

本教程将指导如何为 SQL Server Management Studio (SSMS) 中找到的各种对象生成 Transact-SQL (T-SQL) 脚本。 本教程提供如何编写以下对象的脚本的示例：

> [!div class="checklist"]
> * 在 GUI 中执行操作时的查询
> * 两种不同方式（“脚本编写为”和“生成脚本”）的数据库
> * 表
> * 存储过程
> * 扩展的事件

要为“对象资源管理器”  中的任何对象编写脚本，请右键单击它并选择“编写对象脚本为”  选项。 本教程将介绍该过程。

## <a name="prerequisites"></a>必备条件

若要完成本教程，需要 SQL Server Management Studio、针对运行 SQL Server 的服务器的访问权限以及 AdventureWorks 数据库。

* 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。
* 安装 [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
* 下载 [AdventureWorks2016 示例数据库](https://github.com/Microsoft/sql-server-samples/releases)。

此处提供在 SSMS 中还原数据库的说明：[还原数据库](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)。 

## <a name="script-queries-from-the-gui"></a>从 GUI 编写查询脚本

无论何时在 SSMS 中使用 GUI 来完成任务，都可以为任务生成关联的 T-SQL 代码。 以下示例演示如何在备份数据库和收缩事务日志时执行此操作。 可以向通过 GUI 完成的任何操作应用这些相同的步骤。

### <a name="script-t-sql-when-you-back-up-a-database"></a>备份数据库时编写 T-SQL 脚本

1. 连接到运行 SQL Server 的服务器。

2. 展开 **“数据库”** 节点。

3. 右键单击数据库“Adventureworks2016” > “任务” > “备份”：

    ![备份数据库](media/scripting-ssms/backupdb.png)

4. 按照所需方式配置备份。 对于本教程，所有内容均保留默认值。 但是，在窗口中进行的任何更改都会反映在脚本中。 

5. 选择  “脚本” >   “将操作脚本保存到‘新建查询’窗口”：

    ![脚本数据库备份--脚本操作](media/scripting-ssms/scriptdbbackup.PNG)
6. 查看查询窗口中填充的 T-SQL。

    ![脚本数据库备份--查看 T-SQL](media/scripting-ssms/dbbackupscript.PNG)
7. 选择“执行”以执行查询，以便通过 T-SQL 备份数据库  。 

### <a name="script-t-sql-when-you-shrink-the-transaction-log"></a>在收缩事务日志时编写 T-SQL 脚本

1. 右键单击数据库“AdventureWorks2016” > “任务” > “收缩” > “文件”：

     ![收缩文件](media/scripting-ssms/shrinkfiles.png)

2. 从“文件类型”下拉列表框中选择“日志”：

    ![收缩事务日志](media/scripting-ssms/shrinktlog.png)

3. 选择“脚本”和“将操作脚本保存到剪贴板”   ：

    ![将脚本保存到剪贴板](media/scripting-ssms/scriptactiontoclipboard.png)

4. 打开一个“新建查询”窗口并粘贴  。 （在窗口中右键单击。 然后选择“粘贴”  。）

    ![粘贴脚本](media/scripting-ssms/paste.png)

5. 选择“执行”以执行查询和收缩事务日志  。

## <a name="script-databases"></a>编写数据库脚本

以下部分介绍如何使用“脚本编写为”选项和“生成脚本”选项编写数据库脚本   。 “脚本编写为”选项将重新创建数据库及其配置选项  。 通过使用“生成脚本”选项，可编写架构和数据的脚本  。 在本部分中，你将创建两个新数据库。 可以使用“脚本编写为”  选项来创建 AdventureWorks2016a  。 可以使用“生成脚本”  选项来创建 AdventureWorks2016b  。

### <a name="script-a-database-by-using-the-script-option"></a>使用脚本选项为数据库编写脚本

1. 连接到运行 SQL Server 的服务器。

2. 展开 **“数据库”** 节点。

3. 右键单击数据库“AdventureWorks2016” > “编写数据库脚本为” > “创建到” > “新建查询窗口”：

    ![编写数据库脚本](media/scripting-ssms/scriptdb.png)

4. 在窗口中查看数据库创建查询：

    ![脚本化数据库](media/scripting-ssms/scriptedoutdb.png)：此选项仅脚本化数据库配置选项。

5. 在键盘上选择 Ctrl+F 以打开  “查找”对话框。 选择向下箭头以打开“替换”  选项。 在顶部的“查找”行中键入“AdventureWorks2016”，在底部的“替换”行中键入“AdventureWorks2016a”   。

6. 选择“全部替换”，将所有 AdventureWorks2016 实例替换为 AdventureWorks2016a    。 

    ![查找和替换](media/scripting-ssms/findandreplace.png)

7. 选择“执行”以执行查询并创建新的 AdventureWorks2016a 数据库  。 

### <a name="script-a-database-by-using-the-generate-scripts-option"></a>使用生成脚本选项为数据库编写脚本

1. 连接到运行 SQL Server 的服务器。

2. 展开 **“数据库”** 节点。

3. 右键单击  AdventureWorks2016 >   “任务” >   “生成脚本”：

    ![生成数据库的脚本](media/scripting-ssms/generatescriptsfordb.png)

4. “简介”页随即打开  。 选择“下一步”以打开“选择对象”页面   。 可选择整个数据库或数据库中的特定对象。 选择“编写整个数据库及所有数据库对象的脚本”  。

    ![生成对象的脚本](media/scripting-ssms/scriptobjects.png)

5. 选择“下一步”以打开“设置脚本编写选项”页面   。 在此，你可以配置保存脚本的位置以及一些其他高级选项。 

    a. 选择“保存到新建查询窗口”  。

    b.保留“数据库类型”设置，即设置为“共享”。 选择“高级”并确保已设置以下选项  ：

      * “编写统计信息脚本”设置为“编写统计信息脚本”   。
      * “要编写脚本的数据的类型”设置为“仅限架构”   。
      * “编写索引脚本”设置为“True”   。

   ![编写对象脚本](media/scripting-ssms/advancedscripts.png)

   > [!NOTE]
   > 如果为“要编写脚本的数据的类型”选项选择“架构和数据”，可以为数据库的数据编写脚本   。 但是，这对于大型数据库来说并不是理想之选。 它可能需要比 SSMS 分配更多的内存。 这个限制也适用于小型数据库。 如果要移动大型数据库的数据，请使用[导入和导出向导](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。

6. 选择“确定”  ，然后选择“下一步”  。

7. 在“摘要”页上，请选择“下一步”   。 然后再次选择“下一步”，以将脚本生成到“新建查询”窗口   。

8. 在键盘上打开  “查找”对话框 (Ctrl+F)。 选择向下箭头以打开“替换”  选项。 在顶部  的“查找”行，输入 AdventureWorks2016  。 在底部的  “替换”行，输入 AdventureWorks2016b  。

9. 选择“全部替换”以将所有 AdventureWorks2016 实例替换为 AdventureWorks2016b    。

    ![AdventureWorks 2016](media/scripting-ssms/adventureworks2016b.png)

10. 选择“执行”以执行查询并创建新的 AdventureWorks2016b 数据库  。

## <a name="script-tables"></a>编写表脚本

本部分介绍如何编写数据库中表的脚本。 使用此选项可以创建表或删除和创建表。 通过此选项，还可以编写与修改表相关的 T-SQL 脚本。 例如，插入或更新表。 在此部分中，将删除表，然后重新创建它。 

1. 连接到运行 SQL Server 的服务器。

2. 展开“数据库”节点  。

3. 展开“AdventureWorks2016”数据库节点  。 

4. 展开“表”节点  。

5. 右键单击“dbo.ErrorLog” > “编写表脚本为” > “删除并创建到” > “新建查询编辑器窗口”：

    ![编写表脚本](media/scripting-ssms/scripttable.png)

6. 选择“执行”以执行该查询  。 此操作将删除  Errorlog 表并重新创建它。 

    >[!NOTE]
    > 默认情况下，AdventureWorks2016 数据库中的  “错误日志”表为空。 所以，删除表并不会丢失任何数据。 但是，对包含数据的表执行这些步骤会导致数据丢失。

## <a name="script-stored-procedures"></a>编写存储过程脚本

此部分介绍如何删除并创建存储过程。  

1. 连接到运行 SQL Server 的服务器。

2. 展开“数据库”节点  。

3. 展开“可编程性”节点  。 

4. 展开“存储过程”节点  。

5. 右键单击存储过程“dbo.uspGetBillOfMaterials” > “编写存储过程脚本为” > “删除并创建到” > “新建查询编辑器窗口”：

    ![编写存储过程脚本](media/scripting-ssms/script-stored-procedure.png)

## <a name="script-extended-events"></a>编写扩展事件脚本

本部分介绍如何编写[扩展事件](../../relational-databases/extended-events/extended-events.md)脚本。

1. 连接到运行 SQL Server 的服务器。

2. 展开“管理”节点  。

3. 展开“扩展事件”节点  。

4. 展开“会话”节点  。

5. 右键单击感兴趣的扩展会话 >“编写会话脚本为” > “CREATE 到” > “新建查询编辑器窗口”：

    ![扩展的新建查询编辑器窗口会话](media/scripting-ssms/scriptxevents.png)

6. 在“新建查询编辑器窗口”中，将新的会话名称从 system_health 修改为 system_health2    。 选择“执行”以执行该查询  。

7. 在“对象资源管理器”中右键单击“会话”   。 选择  “刷新”以查看新的扩展事件会话。 会话旁边的绿色图标表示会话正在运行。 红色图标表示会话已停止。

    ![新的扩展事件会话](media/scripting-ssms/newxevent.png)

    >[!NOTE]
    > 可通过右键单击会话并选择“开始”来开始会话  。 但是，由于这是已经在运行的 system_health 会话的副本，可跳过此步骤  。 可以删除扩展事件会话的副本：右键单击扩展事件会话副本并选择“删除”  。

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些教程  和操作说明  文章可帮助你使用 SSMS 的各种功能。 这些文章教你如何管理 SSMS 组件，以及如何查找常用功能。

* [连接到实例和查询实例](../quickstarts/ssms-connect-query-sql-server.md)
* [在 SSMS 中使用模板](../template/templates-ssms.md)
* [SSMS 配置](ssms-configuration.md)
* [使用 SSMS 的其他提示和技巧](ssms-tricks.md)