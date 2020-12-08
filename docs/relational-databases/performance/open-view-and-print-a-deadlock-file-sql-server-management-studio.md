---
title: 打开、查看和打印死锁文件 (SSMS)
description: 了解如何捕获 SQL Server Profiler 生成的死锁信息并在 SQL Server Management Studio 中查看该信息。
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- deadlocks [SQL Server], printing files
- deadlocks [SQL Server], opening files
- opening deadlock files
- printing deadlock files
ms.assetid: 5061b13f-2cb7-457a-b8d0-fbd437b510ab
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5d778526336ce26a4cfb7932971a1be275a5dda
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505111"
---
# <a name="open-view-and-print-a-deadlock-file-in-sql-server-management-studio-ssms"></a>在 SQL Server Management Studio (SSMS) 中打开、查看和打印死锁文件

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 生成死锁时，您可以捕获死锁信息并将死锁信息保存到文件。 保存死锁文件后，可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中将其打开以进行查看或打印。  
  
## <a name="open-and-view-a-deadlock-file"></a>打开和查看死锁文件  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“文件”菜单上，指向“打开”，然后选择“文件”。  
  
2. 在 **“打开文件”** 对话框的 **“文件类型”** 框中选择 .xdl 文件类型。 现在就获得了一个筛选为只包含死锁文件的列表。  
  
## <a name="print-a-deadlock-file"></a>打印死锁文件  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“文件”菜单上，指向“打开”，然后选择“文件”。  
  
2. 在 **“打开文件”** 对话框的 **“文件类型”** 框中选择 .xdl 文件类型。 现在就获得了一个筛选为只包含死锁文件的列表。  
  
3. 选择要打印的死锁文件，然后选择“打开”。  
  
4. 在“文件”菜单上，选择“打印”。  
  
## <a name="see-also"></a>另请参阅  
 [保存死锁图形 (SQL Server Profiler)](../../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md)  
  
  
