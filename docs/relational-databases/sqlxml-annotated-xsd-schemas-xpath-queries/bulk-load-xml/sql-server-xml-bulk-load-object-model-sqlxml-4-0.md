---
title: 'SQL Server XML 大容量加载对象模型 (SQLXML) '
description: 了解用于在 SQLXML 4.0 中大容量加载 XML 的 SQLXMLBulkLoad 对象的方法和属性。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- bulk load [SQLXML], object model
- ErrorLogFile property
- SGDropTables property
- SGUseID property
- KeepNulls property
- ConnectionCommand property
- SchemaGen property
- XMLFragment property
- SQLXMLBulkLoad object
- ForceTableLock property
- CheckConstraints property
- BulkLoad property
- TempFilePath property
- IgnoreDuplicateKeys property
- Transaction property
- KeepIdentity property
- ConnectionString property
- FireTriggers property
- Execute method
- XML Bulk Load [SQLXML], object model
ms.assetid: a9efbbde-ed2b-4929-acc1-261acaaed19d
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bc5414ba64ec7a801cf279a4735a2f8e85cd3731
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461728"
---
# <a name="sql-server-xml-bulk-load-object-model-sqlxml-40"></a>SQL Server XML 大容量加载对象模型 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] XML 大容量加载对象模型由 SQLXMLBulkLoad 对象组成。 该对象支持以下方法和属性。  
  
## <a name="methods"></a>方法  
 执行  
 通过使用作为参数提供的架构文件和数据文件（或流），大容量加载数据。  
  
## <a name="properties"></a>属性  
 BulkLoad  
 指定是否应执行大容量加载。 如果只想生成架构 (查看) 的 SchemaGen、SGDropTables 和 SGUseID 属性，而不是执行大容量加载，则此属性很有用。 此属性是一个布尔属性。 当此属性设置为 TRUE 时，XML 大容量加载将执行。 当此属性设置为 FALSE 时，XML 大容量加载将不执行。  
  
 默认值为 TRUE。  
  
 CheckConstraints  
 指定在 XML 大容量加载将数据插入列时是否应检查对该列指定的约束（例如，由于列之间的主键/外键关系而产生的约束）。 此属性是一个布尔属性。  
  
 在此属性设置为 TRUE 时，XML 大容量加载检查每个插入的值的约束（这意味着约束冲突将导致错误）。  
  
> [!NOTE]  
>  若要将此属性保留为 "FALSE"，您必须对目标表具有 **ALTER TABLE** 权限。 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)。  
  
 默认值是 FALSE。 在该值设置为 FALSE 时，XML 大容量加载在插入操作期间将忽略这些约束。 在当前实现中，您必须按照映射架构中的主键和外键关系的顺序定义表。 也就是说，具有主键的表必须在具有外键的相应表之前定义；否则，XML 大容量加载将失败。  
  
 请注意，如果 ID 传播正在进行，则此选项不适用并且约束检查将保持。 这将在 `KeepIdentity=False` 并且存在定义的关系（即，父级是标识字段并且在生成子级时值将提供给子级）发生。  
  
 ConnectionCommand  
 标识现有连接对象 (例如，XML 大容量加载应使用的 ADO 或 ICommand 命令对象) 。 可以使用 ConnectionCommand 属性，而不是使用 ConnectionString 属性指定连接字符串。 如果使用 ConnectionCommand，则必须将 Transaction 属性设置为 TRUE。  
  
 如果同时使用 ConnectionString 和 ConnectionCommand 属性，则 XML 大容量加载将使用最后一个指定的属性。  
  
 默认值为 NULL。  
  
 ConnectionString  
 标识 OLE DB 连接字符串，该字符串提供建立与数据库实例的连接所必需的信息。 如果同时使用 ConnectionString 和 ConnectionCommand 属性，则 XML 大容量加载将使用最后一个指定的属性。  
  
 默认值为 NULL。  
  
 ErrorLogFile  
 指定 XML 大容量加载将错误和消息记录到的文件名。 默认值为空字符串，在此情况下将不发生日志记录。  
  
 FireTriggers  
 指定在大容量加载操作期间是否应引发对目标表定义的触发器。 默认值为 FALSE。  
  
 在设置为 TRUE 时，在插入操作期间触发器将照常触发。  
  
> [!NOTE]  
>  若要将此属性保留为 "FALSE"，您必须对目标表具有 **ALTER TABLE** 权限。 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)。  
  
 请注意，如果 ID 传播正在进行，则此选项不适用并且触发器将保持。 这将在 `KeepIdentity=False` 并且存在定义的关系（即，父级是标识字段并且在生成子级时值将提供给子级）发生。  
  
 ForceTableLock  
 指定在大容量加载期间，XML 大容量加载将数据复制到的表是否应被锁定。 此属性是一个布尔属性。 在此属性设置为 TRUE 时，XML 大容量加载要求在大容量加载期间执行表锁定。 在此属性设置为 FALSE 时，XML 大容量加载在每次将记录插入到表中时要求表锁定。  
  
 默认值是 FALSE。  
  
 IgnoreDuplicateKeys  
 指定尝试在某一键列中插入重复值时应采取什么后续操作。 如果此属性设置为 TRUE 并且尝试在某一键列中插入具有重复值的记录，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将不插入该记录。 但它将插入随后的记录；这样，大容量加载操作将不会失败。 如果此属性设置为 FALSE，则尝试在某一键列中插入重复值时大容量加载将失败。  
  
 当 IgnoreDuplicateKeys 属性设置为 TRUE 时，将为表中插入的每个记录发出 COMMIT 语句。 这将降低性能。 仅当 Transaction 属性设置为 FALSE 时，才能将属性设置为 TRUE，因为事务行为是使用文件实现的。  
  
 默认值是 FALSE。  
  
 KeepIdentity  
 指定如何处理源文件中标识类型列的值。 此属性是一个布尔属性。 在此属性设置为 TRUE 时，XML 大容量加载将在源文件中指定的值分配给标识列。 在该属性设置为 FALSE 时，大容量加载操作将忽略在源文件中指定的标识列值。 在此情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将某一值分配给标识列。  
  
 如果大容量加载涉及作为外键的列，该外键引用存储 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 生成的值的标识列，则大容量加载会相应将这些标识值传播到外键列。  
  
 此属性的值适用于在大容量加载中涉及的所有列。 默认值为 TRUE。  
  
> [!NOTE]  
>  若要将此属性保留为 TRUE，您必须对目标表具有 **ALTER TABLE** 权限。 否则，它必须设置为 FALSE 的值。 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)。  
  
 KeepNulls  
 指定哪些值将用于在 XML 文档中缺少相应属性或子元素的列。 此属性是一个布尔属性。 当此属性设置为 TRUE 时，XML 大容量加载将 Null 值分配给该列。 它并不分配在服务器上设置的该列的默认值（如果有）。 此属性的值适用于在大容量加载中涉及的所有列。  
  
 默认值是 FALSE。  
  
 SchemaGen  
 指定是否在执行大容量加载操作前创建必需的表。 此属性是一个布尔属性。 如果该属性设置为 TRUE，则创建在映射架构中标识的表（数据库必须存在）。 如果数据库中已存在一个或多个表，则 SGDropTables 属性将确定是否删除并重新创建这些预先存在的表。  
  
 SchemaGen 属性的默认值为 FALSE。 SchemaGen 不会对新创建的表创建 PRIMARY KEY 约束。 但是，如果在映射架构中可以找到匹配的 **sql： relationship** 和 **sql： key 字段** 注释，并且键字段由单个列构成，则 SchemaGen 会在数据库中创建 FOREIGN KEY 约束。  
  
 请注意，如果将 SchemaGen 属性设置为 TRUE，则 XML 大容量加载将执行以下操作：  
  
-   根据元素和属性名称创建必需的表。 因此，不采用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 保留字作为架构中的元素和属性名称十分重要。  
  
-   为使用[xml 数据类型](../../../t-sql/xml/xml-transact-sql.md)格式的[sql：溢出字段](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-overflow-field.md)指定的任何列返回溢出数据。  
  
 SGDropTables  
 指定是否应删除并重新创建现有表。 当 SchemaGen 属性设置为 TRUE 时，将使用此属性。 如果 SGDropTables 为 FALSE，则保留现有表。 在此属性为 TRUE 时，将删除并重新创建现有表。  
  
 默认值是 FALSE。  
  
 SGUseID  
 指定在创建表时，是否可以使用映射架构中标识为 **id** 类型的属性来创建 PRIMARY KEY 约束。 当 SchemaGen 属性设置为 TRUE 时，使用此属性。 如果 SGUseID 为 TRUE，则 SchemaGen 实用程序会使用一个属性，该属性的 **dt： type = "id"** 被指定为主键列，并在创建表时添加适当的 primary key 约束。  
  
 默认值是 FALSE。  
  
 TempFilePath  
 指定文件路径，该路径中包含 XML 大容量加载为事务性大容量加载创建的临时文件。 仅当 Transaction 属性设置为 TRUE 时， (此属性才有用。 ) 必须确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用于 XML 大容量加载的帐户有权访问此路径。 如果未设置此属性，XML 大容量加载将 TEMP 环境变量上指定的位置中存储临时文件。  
  
 事务  
 指定大容量加载是否应作为事务实现，在此情况下，如果大容量加载失败，则确保回滚。 此属性是一个布尔属性。 如果此属性设置为 TRUE，则大容量加载将在事务上下文中发生。 仅当 Transaction 设置为 TRUE 时，TempFilePath 属性才有用。  
  
> [!NOTE]  
>  如果正在加载二进制数据 (例如，将 bin. hex、bin .xml XML 数据类型加载到二进制文件、图像 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型) ，则必须将 Transaction 属性设置为 FALSE。  
  
 默认值是 FALSE。  
  
 XMLFragment  
 指定源数据是否是 XML 片断。 XML 片段是没有单个顶级（根）元素的 XML 文档。 此属性是一个布尔属性。 如果源文件由 XML 片断构成，则此属性必须设置为 TRUE。  
  
 默认值是 FALSE。  
  
  
