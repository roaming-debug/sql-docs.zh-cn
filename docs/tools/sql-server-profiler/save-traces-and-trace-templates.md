---
title: 保存跟踪和跟踪模板
titleSuffix: SQL Server Profiler
description: 了解如何将捕获的事件数据保存到 SQL Server Profiler 中的跟踪文件或数据库表中，以及如何保存用户定义的跟踪模板。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 957e6bf8-e7a3-4a59-a1cd-0a41538a8158
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: c75f865ed31e43f2fbdec96cfc65fcbd9619c1d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345775"
---
# <a name="save-traces-and-trace-templates"></a>保存跟踪和跟踪模板

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

区分保存跟踪文件和保存跟踪模板很重要。 保存跟踪文件是指将捕获的事件数据保存到指定位置。 保存跟踪模板是指保存跟踪定义，例如指定的数据列、事件类或筛选器。  
  
## <a name="saving-traces"></a>保存跟踪  
 如果需要在以后分析或重播捕获数据，请将捕获的事件数据保存到文件或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。 使用跟踪文件可以执行下列操作：  
  
-   使用跟踪文件或跟踪表可以创建工作负荷，作为数据库引擎优化顾问的输入。  
  
-   使用跟踪文件可以捕获事件，并将跟踪文件发送给支持提供商供其分析。  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的查询处理工具可以访问数据或在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中查看数据。 只有 **sysadmin** 固定服务器角色的成员或表的创建者可以直接访问跟踪表。  
  
> [!NOTE]  
>  将跟踪数据捕获到表比将跟踪数据捕获到文件慢。 因此可以将跟踪数据捕获到文件，打开该跟踪文件，然后将跟踪另存为跟踪表。  
  
 如果你使用跟踪文件， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 会将捕获的事件数据（而非跟踪定义）保存到 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 跟踪文件 (\*.trc) 中。 保存跟踪文件时自动在文件名的末尾加上该扩展名，不管是否指定任何其他扩展名。 例如，如果指定一个名为 **Trace.dat** 的跟踪文件，则创建的文件名为 **Trace.dat.trc**。  
  
> [!IMPORTANT]  
>  拥有 SHOWPLAN、ALTER TRACE 或 VIEW SERVER STATE 权限的用户可以对显示计划输出中捕获的查询进行查看。 这些查询可能包含敏感信息，例如密码。 因此，建议仅将这些权限授予有权查看敏感信息的一类用户，例如 **db_owner** 固定数据库角色的成员或 **sysadmin** 固定服务器角色的成员。 此外，建议您最好将包含显示计划相关事件的显示计划文件或跟踪文件保存到使用 NTFS 文件系统的某个位置，并且只允许有权查看敏感信息的用户对之进行访问。  
  
## <a name="saving-templates"></a>保存模板  
 跟踪的模板定义包括事件类、数据列、筛选器和用于创建跟踪的所有其他属性（捕获的事件数据除外）。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 提供针对常见跟踪任务和特定任务的预定义系统模板，如创建可供数据库引擎优化顾问用来优化物理数据库设计的工作负荷。 还可以创建和保存用户定义模板。  
  
### <a name="importing-and-exporting-templates"></a>导入和导出模板  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 允许在服务器之间导入和导出模板。 导出模板会将现有模板的副本移到指定目录。 导入模板会复制指定的模板。 在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 中查看这些模板时，可以通过模板名称后的术语“(user)”使它们区别于系统模板。 您无法覆盖或直接修改预定义系统模板。  
  
### <a name="analyzing-performance-with-templates"></a>使用模板分析性能  
 如果您经常监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，请使用模板分析性能。 模板每次都捕获相同的事件数据，并使用同一跟踪定义监视相同的事件。 您不需要在每次创建跟踪时都定义事件类和数据列。 此外，可以将模板提供给其他用户，供其监视特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。 例如，支持提供商可以向客户提供一个模板。 客户使用该模板捕获所需的事件数据，然后将数据发送给支持提供商供其分析。  
  
 **将跟踪保存到文件**  
  
 [将跟踪结果保存到文件 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
 [sp_trace_create (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [将跟踪结果保存到表 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)   
 [创建跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [从正在运行的跟踪中派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [从跟踪文件或跟踪表派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)   
 [导出跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [导入跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
  
