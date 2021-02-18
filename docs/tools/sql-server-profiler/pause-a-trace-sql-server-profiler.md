---
title: 暂停跟踪
titleSuffix: SQL Server Profiler
description: 了解如何暂停跟踪以便 SQL Server Profiler 停止捕获事件数据，并了解在跟踪暂停时可以更改的属性。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 432b9b0c-b5e7-47f3-a71b-310fb3bf2445
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 4c87c6d92edfaab6da1dac5d912db8a82fbfb4cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353398"
---
# <a name="pause-a-trace-sql-server-profiler"></a>暂停跟踪 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
暂停跟踪可防止捕获更多的事件数据，直到重新启动该跟踪。  
  
 暂停跟踪可以防止捕获事件数据，直到重新启动跟踪。 重新启动跟踪将恢复跟踪操作。 重新启动后以前捕获的数据不会丢失。 重新启动跟踪时，将从启动的那一点恢复数据捕获。 暂停跟踪时，可以更改名称、事件、列和筛选器。 但是不能更改要将跟踪数据发送到的目标和服务器连接。  
  
### <a name="to-pause-a-trace"></a>暂停跟踪  
  
1.  选择一个包含正在运行的跟踪的窗口。  
  
2.  在 **“文件”** 菜单上，单击 **“暂停跟踪”** 。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
