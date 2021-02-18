---
title: 创建跟踪模板
titleSuffix: SQL Server Profiler
description: 了解如何在 SQL Server Profiler 中创建新的跟踪模板。 了解如何向模板添加筛选器以及如何添加或删除事件和数据列。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 025868b0-3790-4cda-8757-5a58327bfba0
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 453989e908808e0444807bcfb9dd817b3f79c436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349358"
---
# <a name="create-a-trace-template-sql-server-profiler"></a>创建跟踪模板 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]创建新的跟踪模板。  
  
### <a name="to-create-a-trace-template"></a>创建跟踪模板  
  
1.  在 **“文件”** 菜单上，指向 **“模板”** ，然后单击 **“新建模板”** 。  
  
2.  在 **“跟踪模板属性”** 对话框中，从 **“选择服务器类型”** 列表中选择服务器类型。  
  
3.  在“ **新模板名称** ”框中，输入模板的名称。  
  
4.  或者，单击 **“使新模板基于现有模板”** ，然后从列表中选择模板。  
  
     所有事件、数据列和筛选器的初始设置都由选定模板指定。  
  
5.  或者，单击 **“用作所选服务器类型的默认模板”** 。  
  
6.  在 **“事件选择”** 选项卡上，添加、删除或修改事件、数据列或筛选器。  
  
7.  在 **“事件选择”** 选项卡上，使用网格控件来添加或删除跟踪文件中的事件和数据列，如下所示：  
  
    -   若要添加事件，请在 **“事件”** 列中展开相应的事件类别，再选择事件名称。  
  
    -   添加事件时，默认情况下将包含所有相关数据列。 若要从跟踪中删除某个事件的数据列，请清除此事件的该数据列中的复选框。  
  
    -   若要添加筛选器，请单击数据列的名称，然后在 **“编辑筛选器”** 对话框中指定筛选条件。 也可以右键单击数据列名称，再单击“编辑列筛选器”，以启动“编辑筛选器”对话框。 单击 **“确定”** 以添加筛选器。  
  
8.  单击“保存”。  
  
## <a name="see-also"></a>另请参阅  
 [指定跟踪文件的事件和数据列 (SQL Server Profiler)](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)   
 [从正在运行的跟踪中派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [从跟踪文件或跟踪表派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)   
 [SQL Server Profiler 模板和权限](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
