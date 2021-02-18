---
title: 查看筛选器信息
titleSuffix: SQL Server Profiler
description: 了解如何查看 SQL Server Profiler 当前正在应用于数据列的筛选器，以限制跟踪的事件。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8d002dea-376a-452c-b3ca-3e93656ed75f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c4031cd865c84b9bc6171b25d40de21ab29379ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338603"
---
# <a name="view-filter-information-sql-server-profiler"></a>查看筛选器信息 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]查看数据列的事件类筛选器。  
  
### <a name="to-view-filter-information"></a>查看筛选器信息  
  
1.  打开跟踪文件、跟踪表或 SQL 脚本，在 **“文件”** 菜单上，单击 **“属性”** 。 如果要编辑跟踪模板或创建新跟踪，请跳过此步骤。  
  
2.  在“事件选择”选项卡上，右键单击要查看的筛选器的数据列名称，然后单击“编辑列筛选器”。  
  
3.  在 **“编辑筛选器”** 对话框中，展开筛选器比较运算符以查看为指定的标准分配的值。 对要查看筛选器信息的所有列，重复步骤 2 和步骤 3。  
  
> [!NOTE]  
>  具有分配值的比较运算符显示为粗体格式。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
