---
title: " (Native Client OLE DB 提供程序将数据插入表值参数) |Microsoft Docs"
description: 了解 SQL Server Native Client OLE DB 提供程序支持使用者为表值参数行指定数据的两个模型。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, inserting data into
ms.assetid: 9c1a3234-4675-40d3-b473-8df06208f880
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a5f6c42de35cc635c958fb45731ced7908b637f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753497"
---
# <a name="inserting-data-into-table-valued-parameters-native-client-ole-db-provider"></a>将数据插入 Table-Valued 参数 (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持使用两种模型来指定表值参数行的数据：推送模型和请求模型。 提供演示请求模型的示例；请参阅 [SQL Server 数据编程示例](https://msftdpprodsamples.codeplex.com/)。  
  
> [!NOTE]  
>  表值参数列要么必须在所有行中具有非默认值，要么必须在所有行中具有默认值。 不能在某些行中具有默认值，而在其他行中不具有默认值。 因此，在表值参数绑定中，表值参数行集列数据仅允许状态值 DBSTATUS_S_ISNULL 和 DBSTATUS_S_OK。 DBSTATUS_S_DEFAULT 将导致失败，而绑定的状态值将设置为 DBSTATUS_E_BADSTATUS。  
  
## <a name="push-model-loads-all-table-valued-parameter-data-in-memory"></a>推送模型（在内存中加载所有表值参数数据）  
 推送模型类似于使用参数集（也即，ICommand::Execute 中的 DBPARAMS 参数）。 仅当在未对 IRowset 接口执行自定义实现的情况下使用表值参数行集对象时，才使用推送模型。 当表值参数行集中的行数较少且预计不会给应用程序带来过量内存压力时，建议使用推送模型。 这比请求模型更简单，因为它向使用者应用程序要求的功能不会超过典型 OLE DB 应用程序中当前常用的功能。  
  
 使用者应在执行命令之前向访问接口提供所有表值参数数据。 为提供数据，使用者为每个表值参数填充一个表值参数行集对象。 表值参数行集对象显示行集 Insert、Set 和 Delete 操作，使用者将使用这些操作来操作表值参数数据。 访问接口在执行时将从该表值参数行集对象中提取数据。  
  
 当向使用者提供表值参数行集对象时，使用者可以将其作为行集对象处理。 使用者可以使用 IColumnsInfo::GetColumnInfo 或 IColumnsRowset::GetColumnsRowset 接口方法，获得每列的类型信息（类型、长度上限、精度和确定位数）。 然后，使用者创建取值函数，以便为数据指定绑定。 下一步是将数据行插入表值参数行集。 为此，可以使用 IRowsetChange::InsertRow。 如果必须操作数据，还可以对表值参数行集对象使用 IRowsetChange::SetData 或 IRowsetChange::DeleteRows。 表值参数行集对象将进行引用计数，这与流对象类似。  
  
 如果使用的是 IColumnsRowset::GetColumnsRowset，后续会对结果列的行集对象调用 IRowset::GetNextRows、IRowset::GetData 和 IRowset::ReleaseRows 方法。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序开始执行命令后，将从该表值参数行集对象中提取表值参数值，并将其发送到服务器。  
  
 推送模型对于使用者的工作量要求最少，但占用的内存高于请求模型，原因在于在执行时所有表值参数数据都必须位于内存中。  
  
## <a name="pull-model-obtaining-table-valued-parameter-data-on-demand-from-the-consumer"></a>请求模型（从使用者按需获得表值参数数据）  
 请求模型用于两种应用场景：  
  
-   使行成为流数据。  
  
-   如果其他访问接口中的行集正用作表值参数值。  
  
 在请求模型中，使用者按需为访问接口提供数据。 如果应用程序具有许多数据插入操作，并且内存中的表值参数行集数据将导致过量的内存访问，则使用此方法。 如果使用多个 OLE DB 访问接口，则借助于使用者请求模型，使用者可以将任何行集对象作为表值参数值提供。  
  
 为了使用请求模型，使用者必须提供其自己的对于行集对象的实现。 如果结合使用拉取模型和表值参数行集 (CLSID_ROWSET_TVP)，使用者必须聚合提供程序通过 ITableDefinitionWithConstraints::CreateTableWithConstraints 方法或 IOpenRowset::OpenRowset 方法公开的表值参数行集对象。 使用者对象应只覆盖 IRowset 接口实现。 您必须覆盖以下函数：  
  
-   IRowset::GetNextRows  
  
-   IRowset::AddRefRows  
  
-   IRowset::GetData  
  
-   IRowset::ReleaseRows  
  
-   IRowset::RestartPosition  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将一次从使用者行集对象中读取一行或多行，以便在表值参数中支持流行为。 例如，用户可能在磁盘上（而非内存中）具有表值参数行集数据，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口要求时，可能实现从磁盘读取数据的功能。  
  
 使用者将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用表值参数行集对象上的 IAccessor：： CreateAccessor 将其数据格式与 Native Client OLE DB 提供程序进行通信。 当从使用者缓冲区读取数据时，访问接口将验证所有可写入和非默认列是否至少可用于一个取值函数句柄，并使用相应的句柄来读取列数据。 为了避免混乱，在表值参数行集列与绑定之间应具有一对一的对应关系。 与同一列的重复绑定将导致错误。 此外，每个取值函数都应按顺序拥有 DBBindings 的 iOrdinal  成员。 对于 IRowset::GetData 的调用数将与每行的取值函数数量相同，调用的顺序将基于 iOrdinal 值的顺序（从低值到高值）  。  
  
 访问接口应实现由表值参数行集对象显示的大多数接口。 使用者将使用最少的接口实现行集对象 (IRowset)。 由于盲聚合 (blind aggregation)，因此，表值参数行集对象将实现剩下的所必需的行集对象接口。  
  
 对于任何其他行集对象（如对于任何 OLE DB 访问接口获得的行集对象），使用者提供的行集必须按照 OLE DB 规范要求实现所有必需的行集对象接口。  
  
 在执行时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将回调行集对象以提取行并读取列数据。  
  
## <a name="see-also"></a>另请参阅  
 [表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [使用表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
