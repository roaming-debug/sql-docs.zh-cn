---
description: 大容量插入任务
title: 批量插入任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.bulkinserttask.f1
- sql13.dts.designer.bulkinserttask.connection.f1
- sql13.dts.designer.bulkinserttask.general.f1
- sql13.dts.designer.bulkinserttask.options.f1
helpviewer_keywords:
- Bulk Insert task
- copying data [Integration Services]
ms.assetid: c5166156-6b4c-4369-81ed-27c4ce7040ae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 739086916d6b7bdea630e8327aa0ede0fb899c87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348974"
---
# <a name="bulk-insert-task"></a>大容量插入任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  大容量插入任务为将大量的数据复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或视图提供了有效的方法。 例如，假定贵公司在大型主机系统上存储了数百万行的产品列表，但公司的电子商务系统却使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来填充网页。 您必须每晚都用大型机的主产品列表更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品表。 若要更新表，请以制表符分隔格式保存产品列表，并使用大容量插入任务将数据直接复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。  
  
 为确保高速数据复制，在从源文件向表或视图移动数据时，请不要对数据执行转换。  
  
## <a name="usage-considerations"></a>使用注意事项  
 在使用大容量插入任务之前，请注意以下事项：  
  
-   大容量插入任务只能从文本文件将数据传输到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或视图中。 若要使用大容量插入任务从其他数据库管理系统 (DBMS) 传输数据，必须先将数据从源导出到文本文件，然后再将数据从文本文件导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或视图中。  
  
-   目标必须是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的表或视图。 如果目标表或视图已经包含数据，则在大容量插入任务运行时，新数据会追加到现有数据中。 如果希望替换数据，请在运行大容量插入任务前运行可执行 DELETE 或 TRUNCATE 语句的“执行 SQL”任务。 有关详细信息，请参阅 [Execute SQL Task](../../integration-services/control-flow/execute-sql-task.md)。  
  
-   可以在大容量插入任务对象中使用格式化文件。 如果已通过 **bcp** 实用工具创建了格式化文件，则可以在大容量插入任务中指定其路径。 大容量插入任务支持 XML 和非 XML 格式化文件。 有关格式化文件的详细信息，请参阅[导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。  
  
-   只有 sysadmin 固定服务器角色的成员才能够运行包含大容量插入任务的包。  
  
## <a name="bulk-insert-task-with-transactions"></a>使用事务的大容量插入任务  
 如果没有设置批大小，则整个大容量复制操作将被视为一个事务。 批大小为 **0** 表示在一个批中将数据插入。 如果设置了批大小，则每个批代表一个事务，当批运行完成时，该事务也就被提交。  
  
 由于大容量插入任务与事务相关，所以其行为取决于任务是否加入了包事务。 如果大容量插入任务未加入包事务，则在尝试处理下一个批之前，将每个未出错的批作为一个单元提交。 如果大容量插入任务加入了包事务，则在任务结束时，未出错的批会保留在事务中。 这些批要受到包的提交或回滚操作的影响。  
  
 大容量插入任务中的失败不自动回滚已成功加载的批；同样，若任务成功，批也不会自动提交。 只有在响应包和工作流属性设置时，才会发生提交和回滚操作。  
  
## <a name="source-and-destination"></a>源和目标  
 指定文本源文件的位置时，请注意下列事项：  
  
-   服务器必须具有访问文件和目标数据库的权限。  
  
-   服务器运行大容量插入任务。 因此，任务使用的所有格式化文件都必须位于服务器上。  
  
-   大容量插入任务加载的源文件可以和要向其插入数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库位于同一服务器上，也可以位于远程服务器上。 如果文件在远程服务器上，则必须在路径中使用通用命名约定 (UNC) 名称来指定该文件名。  
  
## <a name="performance-optimization"></a>性能优化  
 若要优化性能，请注意以下事项：  
  
-   如果文本文件与要向其插入数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库位于同一计算机上，则复制操作可以更快的速度进行，因为数据不是在网络上移动的。  
  
-   大容量插入任务不记录引发错误的行。 如果必须捕获此信息，请使用数据流组件的错误输出来将引发错误的行捕获到一个异常错误文件中。  
  
## <a name="custom-log-entries-available-on-the-bulk-insert-task"></a>大容量插入任务可用的自定义日志项  
 下表列出了大容量插入任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**BulkInsertTaskBegin**|指示大容量插入开始。|  
|**BulkInsertTaskEnd**|指示大容量插入完成。|  
|**BulkInsertTaskInfos**|提供有关任务的说明性信息。|  
  
## <a name="bulk-insert-task-configuration"></a>大容量插入任务配置  
 可以按照下列方法来配置大容量插入任务：  
  
-   指定 OLE DB 连接管理器，以便连接到目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库以及要向其插入数据的表或视图。 大容量插入任务仅支持目标数据库的 OLE DB 连接。  
  
-   指定文件或平面文件连接管理器来访问该源文件。 大容量插入任务仅将连接管理器用于源文件位置。 该任务将忽略在连接管理器编辑器中所选择的其他选项。  
  
-   通过使用格式化文件或通过定义源数据的列和行分隔符来定义大容量插入任务使用的格式。 如果使用格式化文件，请指定文件连接管理器来访问该格式化文件。  
  
-   指定在任务插入数据时要对目标表或视图执行的操作。 选项包括是否检查约束、启用标识插入、保留空值、激发触发器或锁定表。  
  
-   提供要插入的数据批的信息，如批大小、文件中要插入的第一行和最后一行、发生多少次插入错误后任务停止插入行以及要排序的列的名称。  
  
 如果大容量插入任务使用平面文件连接管理器访问源文件，则该任务不使用在平面文件连接管理器中指定的格式。 相反，大容量插入任务将使用在格式化文件中指定的格式，或者使用该任务的 RowDelimite 和 ColumnDelimiter 属性的值。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="programmatic-configuration-of-the-bulk-insert-task"></a>大容量插入任务的编程配置  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>相关内容  
  
-   support.microsoft.com 上的技术文章 [您可能会在支持 UAC 的系统上看到“无法准备 SSIS 大容量插入以插入数据”错误](https://go.microsoft.com/fwlink/?LinkId=233693)。  
  
-   msdn.microsoft.com 上的技术文章 [数据加载性能指南](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))。  
  
-   simple-talk.com 上的技术文章 [使用 SQL Server Integration Services 大容量加载数据](https://go.microsoft.com/fwlink/?LinkId=233701)。  
  
## <a name="bulk-insert-task-editor-connection-page"></a>大容量插入任务编辑器（“连接”页）
  可以使用 **“大容量插入任务编辑器”** 对话框的 **“连接”** 页，指定大容量插入操作的源和目标以及使用的格式。  
  
 若要了解大容量插入，请参阅[大容量插入任务](../../integration-services/control-flow/bulk-insert-task.md)和[用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。  
  
### <a name="options"></a>选项  
 **Connection**  
 从列表中选择一个 OLE DB 连接管理器，或单击“\<**New connection...**>创建新的连接。  
  
 **相关主题：** [OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **DestinationTable**  
 键入目标表或视图的名称，或在列表中选择表或视图。  
  
 **格式**  
 选择大容量插入任务的格式源。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**使用文件**|选择包含格式规范的文件。 选择此选项将显示动态选项 **FormatFile**。|  
|**指定**|指定格式。 选择此选项将显示动态选项 **RowDelimiter** 和 **ColumnDelimiter**。|  
  
 **File**  
 在列表中选择一个“文件连接管理器”或“平面文件连接管理器”，或单击“\<**New connection...**>”创建新的连接。  
  
 文件位置与在此任务的连接管理器中指定的 SQL Server 数据库引擎有关。 该文本文件必须可被服务器本地硬盘上的 SQL Server 数据库引擎访问，或可通过 SQL Server 的共享驱动器或映射的驱动器访问。 SSIS 运行时不访问该文件。  
  
 如果通过使用平面文件连接管理器来访问源文件，则大容量插入任务不会使用在平面文件连接管理器中指定的格式。 相反，大容量插入任务将使用在格式化文件中指定的格式，或者使用该任务的 RowDelimite 和 ColumnDelimiter 属性的值。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md) 
  
 **刷新表**  
 刷新表和视图的列表。  
  
### <a name="format-dynamic-options"></a>Format 动态选项  
  
#### <a name="format--use-file"></a>Format = 使用文件  
 **FormatFile**  
 键入格式化文件的路径，或单击省略号按钮“(…)”定位到该格式化文件  。  
  
#### <a name="format--specify"></a>Format = 指定  
 **RowDelimiter**  
 指定源文件中的行分隔符。 默认值为 **{CR}{LF}** 。  
  
 **ColumnDelimiter**  
 指定源文件中的列分隔符。 默认值为 **“制表符”** 。  
  
## <a name="bulk-insert-task-editor-general-page"></a>大容量插入任务编辑器（“常规”页）
  可以使用 **“大容量插入任务编辑器”** 对话框的 **“常规”** 页，对大容量插入任务进行命名和说明。  
  
### <a name="options"></a>选项  
 **名称**  
 为大容量插入任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对大容量插入任务的说明。  
 
## <a name="bulk-insert-task-editor-options-page"></a>大容量插入任务编辑器（“选项”页）
  使用 **“大容量插入任务编辑器”** 对话框的 **“选项”** 页，可以设置大容量插入操作的属性。 大容量插入任务可以将大量的数据复制到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或视图中。  
  
 若要了解如何使用大容量插入，请参阅[大容量插入任务](../../integration-services/control-flow/bulk-insert-task.md)和 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
### <a name="options"></a>选项  
 **CodePage**  
 指定数据文件中数据的代码页。  
  
 **DataFileType**  
 指定在加载操作中要使用的数据类型值。  
  
 **BatchSize**  
 指定每批中的行数。 默认设置为整个数据文件。 如果将 **BatchSize** 设置为零，则数据是在一批中加载的。  
  
 **LastRow**  
 指定要复制的最后一行。  
  
 **FirstRow**  
 指定要开始复制的第一行。  
  
 **选项**  
 |术语|定义|  
|----------|----------------|  
|**检查约束**|选择此项将检查表约束和列约束。|  
|**保留 Null**|选择此项将在大容量插入操作期间保留 Null 值，而不是为空列插入任意默认值。|  
|**启用标识插入**|选择此项可将现有值插入到标识列中。|  
|**表锁**|选择此项将在大容量插入期间锁定表。|  
|**激发触发器**|选择此项将激发对表上的触发器的任意插入、更新或删除操作。|  
  
 **SortedData**  
 指定大容量插入语句中的 ORDER BY 子句。 所提供的列名必须是目标表中的有效列。 默认值为 **false**。 这意味着 ORDER BY 子句将不对数据进行排序。  
  
 **MaxErrors**  
 指定在取消大容量插入操作之前可以发生的最大错误数量。 如果值为 0，则指示对错误的数量没有限制。  
  
> [!NOTE]  
>  大容量加载操作不能导入的每一行都被计为一个错误。  
