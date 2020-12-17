---
title: DTA 中的基本导航
description: 数据库引擎优化顾问 (DTA) 提供一种基于图形用户界面 (GUI) 的方式来查看优化会话和优化建议报表。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
ms.assetid: ad49b2e0-a5e3-49d2-80fd-9f4eaa3652cb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 735176a4fbb92d91d446f47492b2ddaa11098b2e
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489265"
---
# <a name="lesson-1-basic-navigation-in-database-engine-tuning-advisor-dta"></a>第 1 课：数据库引擎优化顾问 (DTA) 基本导览

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

数据库引擎优化顾问提供了一种基于图形用户界面 (GUI) 的方式来查看优化会话和优化建议报表。 本课程将介绍如何启动工具以及如何配置显示。 在本课程的结尾，您将了解启动工具的各种方式以及如何配置其显示，从而支持定期执行的优化任务。  

## <a name="prerequisites"></a>先决条件 

若要完成本教程，需要 SQL Server Management Studio、针对运行 SQL Server 的服务器的访问权限以及 AdventureWorks 数据库。

- 安装 [SQL Server Management Studio。](../../ssms/download-sql-server-management-studio-ssms.md)
- 安装 [SQL Server 2017 Developer Edition。](https://www.microsoft.com/sql-server/sql-server-downloads)
- 下载 [AdventureWorks2017 示例数据库。](../../samples/adventureworks-install-configure.md)


此处提供在 SSMS 中还原数据库的说明：[还原数据库。](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > 本教程适用于熟悉如何使用 SQL Server Management Studio 和基本数据库管理任务的用户。 
  

## <a name="launch-database-tuning-advisor"></a>启动数据库优化顾问 
开始前，请先打开数据库引擎优化顾问 (DTA) 图形用户界面 (GUI)。 第一次使用时，必须由 **sysadmin** 固定服务器角色的成员来启动数据库引擎优化顾问，以初始化应用程序。 初始化后， **db_owner** 固定数据库角色的成员便可使用数据库引擎优化顾问来优化他们拥有的数据库。 有关初始化数据库引擎优化顾问的详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
1. 启动 SQL Server Management Studio (SSMS)。 在 Windows“开始”  菜单上，指向“所有程序”  ，并找到 SQL Server Management Studio  。 
2. 打开 SSMS 后，选择“工具”  菜单，然后选择“数据库优化顾问”  。 

  ![从 SSMS 启动 DTA](media/dta-tutorials/launch-dta.png)

3. 数据库优化顾问将启动，并打开“连接到服务器”  对话框。 验证默认设置，然后选择“连接”  连接到 SQL Server。  
  
默认情况下，数据库引擎优化顾问将打开下图所示的配置：  
  
![数据库引擎优化顾问默认窗口](media/dta-tutorials/dta-default-gui.png)
  
> [!NOTE]  
> “会话监视器”  选项卡显示会话名称，该名称是连接的用户名称和当前日期。 
  
第一次打开时，数据库引擎优化顾问 GUI 中将显示两个主窗格。  
  
-   左窗格包含会话监视器，其中列出已对此 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例执行的所有优化会话。 打开数据库引擎优化顾问时，在窗格顶部将显示一个新会话。 可在相邻窗格中对此会话命名。 最初，仅列出默认会话。 这是数据库引擎优化顾问为您自动创建的默认会话。 对数据库进行优化后，您所连接的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的所有优化会话都将在新会话下面列出。 可右键单击优化会话以对其重命名、关闭、删除或克隆。 如果在列表中单击右键，则可按照名称、状态或创建时间对会话排序，或创建新会话。 在此窗格的底部将显示选定优化会话的详细信息。 可以选择使用“按类别显示”按钮，以显示按类别分组的详细信息；也可使用“按字母显示”按钮，在按字母排序的列表中显示详细信息。   也可以通过将右窗格边框拖动到窗口的左侧来隐藏会话监视器。 若要再次查看，请将窗格边框重新拖动回右侧。 利用会话监视器可以查看以前的优化会话，或使用这些会话来创建具有类似定义的新会话。 还可以使用会话监视器来评估优化建议。 有关详细信息，请参阅 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)。 使用浏览器中的“后退”按钮可返回到本教程。   
  
-   右窗格包含“常规”和“优化选项”选项卡。   在此可以定义数据库引擎优化会话。 在“常规”选项卡中，键入优化会话的名称，指定要使用的工作负荷文件或表，并选择要在该会话中优化的数据库和表。  工作负荷是对要优化的一个或多个数据库执行的一组 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 优化数据库时，数据库引擎优化顾问使用跟踪文件、跟踪表、 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本或 XML 文件作为工作负荷输入。 在“优化选项”选项卡上，可以选择希望数据库引擎优化顾问在分析过程中考虑的物理数据库设计结构（索引或索引视图）和分区策略。  在此选项卡上，还可以指定数据库引擎优化顾问优化工作负载使用的最大时间。 默认情况下，数据库引擎优化顾问优化工作负荷的时间为一个小时。  
  
> [!NOTE]
> 从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器中导入 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 脚本时，数据库引擎优化顾问可接受 XML 文件作为输入。 有关详细信息，请参阅 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 启动并使用数据库引擎优化顾问 [中的“从](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)查询编辑器中启动数据库引擎优化顾问”一节。  
  
## <a name="configure-tool-options-and-layout"></a>配置工具选项和布局 

1.  在“工具”  菜单上，单击“选项”  。  

   ![DTA 选项](media/dta-tutorials/dta-settings.png) 
  
2.  在“选项”  对话框中，查看下列选项：  
  
    -   展开“启动时”  列表，查看数据库引擎优化顾问在启动时可显示的内容。 默认情况下，选择“显示新会话”  。  
  
    -   单击“更改字体”  ，查看在“常规”  选项卡中可以为数据库和表的列表选择的字体。执行优化后，为此选项选择的字体也将用于数据库引擎优化顾问的建议网格和报表中。 默认情况下，数据库引擎优化顾问使用系统字体。  
  
    -   “最近使用的列表中的项数”  可设置为 **1** 到 **10** 之间的数字。 此选项设置在单击“文件”  菜单上的“最近使用的会话”  或“最近使用的文件”  时，列表中可以显示的最大项数。 默认情况下，此选项设置为 **4**。  
  
    -   选中“记住我上次的优化选项”  时，默认情况下，数据库引擎优化顾问会将为上一优化会话指定的优化选项用于下一优化会话中。 清除此复选框，以便使用数据库引擎优化顾问优化选项的默认设置。 默认情况下，将选中此选项。  
  
    -   默认情况下，将选中“在永久删除会话之前询问”  ，避免意外删除优化会话。  
  
    -   默认情况下，将选中“在停止会话分析之前询问”  ，避免在数据库引擎优化顾问完成工作负载分析之前意外停止优化会话。  
  
## <a name="next-lesson"></a>下一课  
[第 2 课：使用数据库引擎优化顾问](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
  
  
