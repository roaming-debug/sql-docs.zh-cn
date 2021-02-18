---
title: 将跟踪结果保存到表
titleSuffix: SQL Server Profiler
description: 了解如何使用 SQL Server Profiler 将跟踪结果保存到 SQL Server 数据库中的表。 了解如何指定要保存的最大行数。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: edbecf74-683b-4e43-a1ef-7a3d5f5e27f6
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: c73bfe20e57e0edf67cbcbf88e4c86de135a9b25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345826"
---
# <a name="save-trace-results-to-a-table-sql-server-profiler"></a>将跟踪结果保存到表 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]将跟踪结果保存到数据库表。  
  
## <a name="to-save-trace-results-to-a-table"></a>将跟踪结果保存到表
  
1.  在 **“文件”** 菜单上，单击 **“新建跟踪”** ，再连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。  
  
     将出现“跟踪属性”对话框。 **“跟踪属性”** 对话框。  
  
    > [!NOTE]  
    >  如果选择“建立连接后立即开始跟踪”，则不会显示“跟踪属性”对话框，而是开始跟踪。 若要关闭此设置，请在“工具”菜单上，单击“选项”，然后清除“建立连接后立即开始跟踪”复选框。  
  
2.  在 **“跟踪名称”** 框中，键入跟踪的名称，然后单击 **“保存到表”** 。  
  
3.  在 **“连接到服务器”** 对话框中，连接到将包含跟踪表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
4.  在“目标表”对话框中，从“数据库”列表中选择一个数据库。  
  
5.  在 **“所有者”** 列表中，选择此跟踪的所有者。  
  
6.  在 **“表”** 列表中，键入或选择跟踪结果的表名。 单击“确定” **。**  
  
7.  在“跟踪属性”对话框中，选中“设置最大行数(以千为单位)”复选框以指定要保存的最大行数。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
