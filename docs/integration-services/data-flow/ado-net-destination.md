---
description: ADO NET 目标
title: ADO NET 目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetdest.f1
- sql13.dts.designer.adonetdest.connection.f1
- sql13.dts.designer.adonetdest.mappings.f1
- sql13.dts.designer.adonetdest.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], ADO.NET
- ADO.NET destination
ms.assetid: cb883990-d875-4d8b-b868-45f9f15ebeae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4ae905d868554c20948af3eb0191937382eff56f
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536369"
---
# <a name="ado-net-destination"></a>ADO NET 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  ADO NET 目标可将数据加载到各种使用数据库表或视图的兼容 [!INCLUDE[vstecado](../../includes/vstecado-md.md)]的数据库中。 你可以选择将这些数据加载到现有表或视图中，或者先创建一个新表，然后将这些数据加载到新表中。  
  
 可使用 ADO NET 目标连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]。 不支持使用 OLE DB 连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。 有关 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 的详细信息，请参阅[通用指导原则和限制（Azure SQL 数据库）](/previous-versions/azure/ee336245(v=azure.100))。  
  
## <a name="troubleshooting-the-ado-net-destination"></a>ADO NET 目标故障排除  
 可以记录 ADO NET 目标对外部数据访问接口所做的调用。 利用此日志记录功能，可以排除 ADO NET 目标执行将数据保存到外部数据源的操作过程中发生的故障。 若要记录 ADO NET 目标对外部数据访问接口所做的调用，请在包级别启用包日志记录并选择 **“诊断”** 事件。 有关详细信息，请参阅 [包执行的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)。  
  
## <a name="configuring-the-ado-net-destination"></a>配置 ADO NET 目标  
 此目标使用 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器连接到数据源，并且此连接管理器可指定要使用的 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 访问接口。 有关详细信息，请参阅 [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md)。  
  
 ADO NET 目标包括输入列和目标数据源中的列之间的映射。 不必将输入列映射到所有目标列。 但是，某些目标列的属性可能需要输入列的映射。 否则，可能会发生错误。 例如，如果目标列不允许出现空值，则必须将输入列映射到该目标列。 另外，映射的列的数据类型必须是兼容的。 例如，如果 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 访问接口不支持将数据类型为字符串的输入列映射到数据类型为数值的目标列，则不能进行此映射。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持将文本插入到数据类型设置为图像的列中。 有关值数据类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
> [!NOTE]  
>  ADO NET 目标不支持将数据类型设置为 DT_DBTIME 的输入列映射到数据类型设置为 datetime 的数据库列。 有关 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型的详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
 ADO NET 目标具有一个常规输入和一个错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [ADO NET 自定义属性](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="ado-net-destination-editor-connection-manager-page"></a>ADO NET 目标编辑器（“连接管理器”页）
  可以使用 **“ADO NET 目标编辑器”** 对话框的 **“连接管理器”** 页，为目标选择 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接。 使用此页还可以选择数据库中的表或视图。  
  
 **打开“连接管理器”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开具有 ADO NET 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 ADO NET 目标。  
  
3.  在 **“ADO NET 目标编辑器”** 中，单击 **“连接管理器”** 。  
  
### <a name="static-options"></a>静态选项  
 **“ODBC 目标编辑器”**  
 从列表中选择一个现有连接管理器，或通过单击“新建”  创建一个新连接。  
  
 **新建**  
 使用“配置 ADO.NET 连接管理器”  对话框创建新的连接管理器。  
  
 **使用表或视图**  
 从列表中选择现有表或视图，或单击  “新建”创建新表。  
  
 **新建**  
 使用  “创建表”对话框创建新表或视图。  
  
> [!NOTE]  
>  单击 **“新建”** 时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将基于所连接的数据源生成一条默认的 CREATE TABLE 语句。 即使源表包含一个已声明了 FILESTREAM 属性的列，此默认 CREATE TABLE 语句也不会包含 FILESTREAM 属性。 若要运行具有 FILESTREAM 属性的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件，首先要在目标数据库上实现 FILESTREAM 存储。 然后在 **“创建表”** 对话框中将 FILESTREAM 属性添加到 CREATE TABLE 语句中。 有关详细信息，请参阅[二进制大型对象 (Blob) 数据 (SQL Server)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)。  
  
 **预览**  
 使用“预览查询结果”  对话框预览结果。 预览最多可以显示 200 行。  
  
 **可用时使用大容量插入**  
 指定是否使用 <xref:System.Data.SqlClient.SqlBulkCopy> 接口来提高大容量插入操作的性能。  
  
 只有可返回 <xref:System.Data.SqlClient.SqlConnection> 对象的 ADO.NET 提供程序才支持使用 <xref:System.Data.SqlClient.SqlBulkCopy> 接口。 SQL Server 的 .NET 数据提供程序 (SqlClient) 可以返回 <xref:System.Data.SqlClient.SqlConnection> 对象，而自定义提供程序可以返回 <xref:System.Data.SqlClient.SqlConnection> 对象。  
  
 可使用 SQL Server 的 .NET 数据提供程序 (SqlClient) 来连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]。  
  
 如果选择“可用时使用大容量插入”并将“错误”选项设置为“重定向该行”，则目标重定向到错误输出的数据批次可能包含正确的行。    有关以大容量操作方式处理错误的详细信息，请参阅[数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。 有关  “错误”选项的详细信息，请参阅 [ADO NET 目标编辑器（“错误输出”页）]()。  
  
> [!NOTE]
>  如果 SQL Server 或 Sybase 源表包含一个标识列，则必须在 ADO NET 目标之前使用“执行 SQL 任务”来启用 IDENTITY_INSERT，并在之后再次禁用它。 （标识列属性为列指定一个增量值。 使用 SET IDENTITY_INSERT 语句，可将源表中的显式值插入目标表中的标识列。）  
> 
>   若要成功运行 SET IDENTITY_INSERT 语句和加载数据，须执行以下操作。  
>       1.对“执行 SQL 任务”和 ADO NET 目标使用相同的 ADO.NET 连接管理器。  
>       2.在连接管理器上，将“RetainSameConnection”属性和“MultipleActiveResultSets”属性设置为“True”   。  
>       3.在 ADO.NET 目标上，将“UseBulkInsertWhenPossible”属性设置为“False”  。   
> 
>  有关详细信息，请参阅 [SET IDENTITY_INSERT (Transact SQL)](../../t-sql/statements/set-identity-insert-transact-sql.md) 和 [IDENTITY（属性）(Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)。  
  
## <a name="external-resources"></a>外部资源  
 sqlcat.com 上的技术文章 [快速将数据加载到 Azure SQL 数据库中](https://techcommunity.microsoft.com/t5/datacat/loading-data-to-sql-azure-the-fast-way/ba-p/305066)  
  
## <a name="ado-net-destination-editor-mappings-page"></a>ADO NET 目标编辑器（“映射”页）
  可以使用 **“ADO NET 目标编辑器”** 对话框的 **“映射”** 页，将输入列映射到目标列。  
  
 **打开“映射”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开具有 ADO NET 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 ADO NET 目标。  
  
3.  在 **“ADO NET 目标编辑器”** 中，单击 **“映射”** 。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 使用拖放操作可以将表中的可用输入列映射到目标列。  
  
 **可用目标列**  
 查看可用目标列的列表。 使用拖放操作可以将表中的可用目标列映射到输入列。  
  
 **输入列**  
 查看选定的输入列。 可以通过选择 \<ignore> 以从输出中排除列来移除映射。  
  
 **目标列**  
 查看每个可用目标列，而不管是否已对其进行映射。  
  
## <a name="ado-net-destination-editor-error-output-page"></a>ADO NET 目标编辑器（“错误输出”页）
  可以使用 **“ADO NET 目标编辑器”** 对话框的 **“错误输出”** 页指定错误处理选项。  
  
 **打开“错误输出”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开具有 ADO NET 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 ADO NET 目标。  
  
3.  在 **“ADO NET 目标编辑器”** 中，单击 **“错误输出”** 。  
  
### <a name="options"></a>选项  
 **输入或输出**  
 查看输入的名称。  
  
 **列**  
 未使用。  
  
 **错误**  
 指定发生错误时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **相关主题：** [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **截断**  
 未使用。  
  
 **说明**  
 查看操作的说明。  
  
 **将此值设置到选定的单元格**  
 指定发生错误或截断时应对所有选定单元格执行的操作：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
