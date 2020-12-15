---
description: 跟踪和重播事件
title: 跟踪和重播事件 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- replaying events
- traces [SMO]
- events [SMO], replaying
- events [SMO], tracing
ms.assetid: f41b3f85-2f6c-4c3e-9776-8c73d2cc7a53
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8280f8891d4ccd57b4496125a62bd96ba03eb49f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475428"
---
# <a name="tracing-and-replaying-events"></a>跟踪和重播事件
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 SMO 中，命名空间中的 **跟踪** 和 **重播** 对象 <xref:Microsoft.SqlServer.Management.Trace> 提供对 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] 功能（用于监视或的实例）的编程访问 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 。 您可以捕获有关每个事件的数据并将其保存到文件或表中供以后分析。 例如，可以监视生产环境，了解哪些过程由于执行速度太慢影响了性能。  
  
 **跟踪** 和 **重播** 对象提供一组对象，这些对象可用于在实例上创建跟踪 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 可以在您自己的应用程序中使用这些对象为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 手动创建跟踪。 此外，SMO **Trace** 对象可用于读取通过监视 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 或 DTS 日志记录创建的 SQL 跟踪文件和表。  
  
 SMO **跟踪** 对象允许您执行以下功能：  
  
-   创建跟踪。  
  
-   设置跟踪筛选器。  
  
-   设置正在跟踪的事件。  
  
-   停止或启动跟踪。  
  
-   读取跟踪文件和跟踪表。  
  
-   获取有关跟踪中的事件的信息。  
  
-   获取有关跟踪中的筛选器的信息。  
  
-   以编程方式操作跟踪数据。  
  
-   写入跟踪表和跟踪文件。  
  
-   重播跟踪文件或跟踪表。  
  
 应用程序可以使用来自 **跟踪** 和 **重播** 对象的跟踪数据，也可以使用 [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md)手动检查这些数据。 跟踪数据还与同时提供跟踪功能的 [SQL 跟踪](../../../relational-databases/sql-trace/sql-trace.md) 存储过程兼容。  
  
 SMO 跟踪对象驻留在 <xref:Microsoft.SqlServer.Management.Trace> 命名空间中，该命名空间要求引用 Microsoft.SQLServer.ConnectionInfo.dll 文件。  
  
 **跟踪** 和 **重播** 对象需要一个 [microsoft.sqlserver.management.common.serverconnection>](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A> 对象，以便与实例建立连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 [Microsoft.sqlserver.management.common.serverconnection>](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120))对象位于命名空间中[，该命名](/previous-versions/sql/sql-server-2014/ms212673(v=sql.120))空间需要对 Microsoft.SQLServer.ConnectionInfo.dll 文件的引用。  
  
> [!NOTE]  
>  64位平台上不支持 **跟踪** 和 **重播** 对象。  
  
