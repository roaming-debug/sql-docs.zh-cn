---
title: 设置跟踪表的最大表大小
titleSuffix: SQL Server Profiler
description: 了解如何限制跟踪表的大小。 使用 SQL Server Profiler 指定表可以拥有的最大行数。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: d0ae83e5-1c88-4a2e-be05-2c341280b978
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 563e01d364c82a4d0c7a58e909ff0e31e875da36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345765"
---
# <a name="set-a-maximum-table-size-for-a-trace-table-sql-server-profiler"></a>设置跟踪表的最大表大小 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]设置跟踪表的最大表大小。  
  
### <a name="to-set-a-maximum-table-size-for-a-trace-table"></a>设置跟踪表的最大表大小  
  
1.  在 **“文件”** 菜单上，单击 **“新建跟踪”** ，再连接到 SQL Server 实例。  
  
     将出现“跟踪属性”对话框。 **“跟踪属性”** 对话框。  
  
    > [!NOTE]  
    >  如果选择“建立连接后立即开始跟踪”，则不会显示“跟踪属性”对话框，而是开始跟踪。 若要关闭此设置，请在“工具”菜单上，单击“选项”，然后清除“建立连接后立即开始跟踪”复选框。  
  
2.  在 **“跟踪名称”** 框中，键入跟踪的名称。  
  
3.  在“模板名称”列表中，选择跟踪模板。  
  
4.  选中“保存到表”复选框。  
  
5.  连接到要存储跟踪的服务器。  
  
     将显示 **“目标表”** 对话框。  
  
6.  从 **“数据库”** 列表中，为跟踪选择数据库。  
  
7.  在 **“表”** 框中，键入或选择表名。  
  
8.  选中 **“设置最大行数”** 复选框，然后指定跟踪表的最大行数。  
  
    > [!NOTE]  
    >  表中的行数超过指定的最大值时，将不再记录跟踪事件。 但是，仍继续跟踪。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 事件探查器](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
