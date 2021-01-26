---
description: SQL Server 目标
title: SQL Server 目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlserverdest.f1
- sql13.dts.designer.sqlserverdestadapter.connection.f1
- sql13.dts.designer.sqlserverdestadapter.mappings.f1
- sql13.dts.designer.sqlserverdestadapter.advanced.f1
helpviewer_keywords:
- SQL Server destination
- loading data
- destinations [Integration Services], SQL Server
- inserting data
- bulk load [Integration Services]
ms.assetid: a0227cd8-6944-4547-87e8-7b2507e26442
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e980e075f23d3a943870fc6dd02aa420797f8a4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596479"
---
# <a name="sql-server-destination"></a>SQL Server 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  SQL Server 目标连接到本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，并将数据大容量加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表和视图中。 如果包访问远程服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，则不能在包中使用 SQL Server 目标。 相反，包应使用 OLE DB 目标。 有关详细信息，请参阅 [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md)。  
  
## <a name="permissions"></a>权限  
 如果用户所执行的包中包括 SQL Server 目标，则用户需要“创建全局对象”权限。 通过使用从 **“管理工具”** 菜单打开的本地安全策略工具，可以将此权限授予用户。 如果在执行使用 SQL Server 目标的包时收到错误消息，请确保运行包的帐户拥有“创建全局对象”权限。  
  
## <a name="bulk-inserts"></a>大容量插入  
 如果尝试使用 SQL Server 目标向远程 SQL Server 数据库中大容量加载数据，您将看到如下错误消息：已获得 OLE DB 记录。 源: “Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client” Hresult: 0x80040E14 说明:“由于无法打开 SSIS 文件映射对象 ‘Global\DTSQLIMPORT’，无法进行大容量加载。 操作系统错误代码为 2 （系统找不到指定的文件。）。 请确保您是通过 Windows 安全性访问本地服务器的。”  
  
 此 SQL Server 目标将数据插入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的速度与使用“大容量插入”任务时一样快；但使用 SQL Server 目标可以在数据加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中之前，对列数据应用转换。  
  
 为了将数据加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，应考虑使用 SQL Server 目标而不是 OLE DB 目标。  
  
### <a name="bulk-insert-options"></a>大容量插入选项  
 如果 SQL Server 目标使用快速加载数据访问模式，则可以指定下列快速加载选项：  
  
-   保留导入数据文件的标识值，或使用由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]分配的唯一值。  
  
-   在大容量加载操作过程中保留空值。  
  
-   在大容量导入操作过程中，验证目标表或视图上的约束。  
  
-   在大容量加载操作期间获取表级锁。  
  
-   在大容量加载操作过程中，执行对目标表定义的插入触发器。  
  
-   在大容量插入操作过程中，指定要加载的输入中第一行的行号。  
  
-   在大容量插入操作过程中，指定要加载的输入中最后一行的行号。  
  
-   指定在大容量加载操作取消之前允许的最大错误数。 无法导入的每一行将被计为一个错误。  
  
-   指定输入中包含已排序数据的列。  
  
 有关大容量加载选项的详细信息，请参阅 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
#### <a name="performance-improvements"></a>性能改进  
 若要改善大容量插入的性能和在大容量插入操作过程中对表数据的访问，应对默认选项作如下更改：  
  
-   在大容量导入操作过程中，不要验证目标表或视图上的约束。  
  
-   在大容量加载操作过程中，不要执行在目标表上定义的插入触发器。  
  
-   不要对表应用锁。 这样，其他用户和应用程序在大容量插入操作过程中将可以一直使用该表。  
  
## <a name="configuration-of-the-sql-server-destination"></a>SQL Server 目标的配置  
 可以使用下列方式配置 SQL Server 目标：  
  
-   指定要向其大容量加载数据的表或视图。  
  
-   通过指定如是否检查约束之类的选项，自定义大容量加载操作。  
  
-   指定是否在一批中提交所有行，或设置在一批中提交的最大行数。  
  
-   指定大容量加载操作的超时值。  
  
 此目标使用 OLE DB 连接管理器连接到数据源，该连接管理器指定要使用的 OLE DB 访问接口。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目还提供了创建 OLE DB 连接管理器所用的数据源对象。 这样，SQL Server 目标便可以使用数据源和数据源视图。  
  
 SQL Server 目标具有一个输入。 它不支持错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [SQL Server 目标自定义属性](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
 有关如何设置属性的详细信息，请单击下列主题之一：  
  
-   [使用 SQL Server 目标大容量加载数据](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [使用 SQL Server 目标大容量加载数据](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>相关内容  
  
-   support.microsoft.com 上的技术文章 [您可能会在支持 UAC 的系统上看到“无法准备 SSIS 大容量插入以插入数据”错误](/troubleshoot/sql/integration-services/error-you-run-ssis-package)。  
  
-   msdn.microsoft.com 上的技术文章 [数据加载性能指南](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))。  
  
-   simple-talk.com 上的技术文章 [使用 SQL Server Integration Services 大容量加载数据](https://go.microsoft.com/fwlink/?LinkId=233701)。  
  
## <a name="sql-destination-editor-connection-manager-page"></a>SQL 目标编辑器（“连接管理器”页）
  可以使用 **“SQL 目标编辑器”** 对话框的 **“连接管理器”** 页，指定数据源信息以及预览结果。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标可以将数据加载到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的表或视图中。  
  
### <a name="options"></a>选项  
 **“无缓存”**  
 从列表中选择现有连接，或通过单击“新建”创建一个新连接。  
  
 **新建**  
 通过使用“配置 OLE DB 连接管理器”  对话框创建新的连接。  
  
 **使用表或视图**  
 从列表中选择现有的表或视图，或单击“新建”创建新的连接。  
  
 **新建**  
 通过使用“创建表”  对话框创建一个新表。  
  
> [!NOTE]  
>  单击 **“新建”** 时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将基于所连接的数据源生成一条默认的 CREATE TABLE 语句。 即使源表包含一个已声明了 FILESTREAM 属性的列，此默认 CREATE TABLE 语句也不会包含 FILESTREAM 属性。 若要运行具有 FILESTREAM 属性的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件，首先要在目标数据库上实现 FILESTREAM 存储。 然后在 **“创建表”** 对话框中将 FILESTREAM 属性添加到 CREATE TABLE 语句中。 有关详细信息，请参阅[二进制大型对象 (Blob) 数据 (SQL Server)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)。  
  
 **预览**  
 使用“预览查询结果”  对话框预览结果。 预览最多可以显示 200 行。  
  
## <a name="sql-destination-editor-mappings-page"></a>SQL 目标编辑器（“映射”页）
  可以使用 **“SQL 目标编辑器”** 对话框的 **“映射”** 页，将输入列映射到目标列。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 使用拖放操作可以将表中的可用输入列映射到目标列。  
  
 **可用目标列**  
 查看可用目标列的列表。 使用拖放操作可以将表中的可用目标列映射到输入列。  
  
 **输入列**  
 查看从上表中选择的输入列。 可以通过使用 **“可用输入列”** 列表来更改映射。  
  
 **目标列**  
 查看每个可用的目标列，包括已映射或未映射的目标列。  
  
## <a name="sql-destination-editor-advanced-page"></a>SQL 目标编辑器（“高级”页）
  可以使用 **“SQL 目标编辑器”** 对话框的 **“高级”** 页指定大容量插入高级选项。  
  
### <a name="options"></a>选项  
 **保留标识**  
 指定任务是否应在标识列中插入值。 此属性的默认值为 **False**。  
  
 **保留 Null**  
 指定任务是否应保留空值。 此属性的默认值为 **False**。  
  
 **表锁**  
 指定在加载数据时是否锁定表。 此属性的默认值为 **True**。  
  
 **检查约束**  
 指定任务是否应检查约束。 此属性的默认值为 **True**。  
  
 **激发触发器**  
 指定大容量插入任务是否应对表激发触发器。 此属性的默认值为 **False**。  
  
 **首行**  
 指定要插入的第一行。 此属性的默认值为 **-1**，表示尚未分配值。  
  
> [!NOTE]  
>  如果在 **“SQL 目标编辑器”** 中清空此文本框，则表示不希望为此属性分配值。 请在“属性”窗口、“高级编辑器”和对象模型中使用 -1。  
  
 **末行**  
 指定要插入的最后一行。 此属性的默认值为 **-1**，表示尚未分配值。  
  
> [!NOTE]  
>  如果在 **“SQL 目标编辑器”** 中清空此文本框，则表示不希望为此属性分配值。 请在“属性”窗口、“高级编辑器”和对象模型中使用 -1。  
  
 **最大错误数**  
 指定在大容量插入任务停止之前可以发生的错误数量。 此属性的默认值为 **-1**，表示尚未分配值。  
  
> [!NOTE]  
>  如果在 **“SQL 目标编辑器”** 中清空此文本框，则表示不希望为此属性分配值。 请在“属性”窗口、“高级编辑器”和对象模型中使用 -1。  
  
 **超时**  
 指定在大容量插入任务因超时而停止之前等待的秒数。  
  
 **设置列顺序**  
 键入排序列的名称。 每一列都可以按升序或降序排序。 如果您使用多个排序列，请用逗号分隔列表。  
  
## <a name="see-also"></a>另请参阅  
 [数据流](../../integration-services/data-flow/data-flow.md)  
