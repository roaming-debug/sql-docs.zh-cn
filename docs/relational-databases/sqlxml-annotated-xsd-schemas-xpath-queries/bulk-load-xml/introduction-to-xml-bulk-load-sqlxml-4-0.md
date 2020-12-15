---
title: XML 大容量加载 (SQLXML) 简介
description: 了解 XML 大容量加载实用工具，它是 SQLXML 4.0 中的独立 COM 对象，可用于将半结构化 XML 数据加载到 Microsoft SQL Server 表中。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nontransacted XML Bulk Load operations
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
- transacted XML Bulk Load operations
- streaming XML data
ms.assetid: 38bd3cbd-65ef-4c23-9ef3-e70ecf6bb88a
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bd7ea649bc00090c64b312f007b40ea15bdf9bb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439673"
---
# <a name="introduction-to-xml-bulk-load-sqlxml-40"></a>XML 大容量加载简介 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  XML 大容量加载是一个独立的 COM 对象，可用于将半结构化 XML 数据加载到 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表中。  
  
 您可以使用 INSERT 语句和 OPENXML 函数将 XML 数据插入到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库；但是，当需要插入大量 XML 数据时，大容量加载实用工具提供了更好的性能。  
  
 XML 大容量加载对象模型的 Execute 方法采用两个参数：  
  
-   带批注的 XML 架构定义 (XSD) 或 XML 数据精简 (XDR) 架构。 XML 大容量加载实用工具解释此映射架构和在标识要插入 XML 数据的 SQL Server 表时在该架构中指定的批注。  
  
-   XML 文档或文档片段（文档片段是没有单个顶级元素的文档）。 可以指定 XML 大容量加载可读取的文件名或流。  
  
 XML 大容量加载解释此映射架构，并标识要插入 XML 数据的表。  
  
 本部分假定您熟悉以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能：  
  
-   带批注的 XSD 架构和 XDR 架构。 有关带批注的 XSD 架构的详细信息，请参阅 [&#40;SQLXML 4.0&#41;中带批注的 Xsd 架构简介 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)。 有关带批注的 XDR 架构的信息，请参阅 [SQLXML 4.0&#41;中 &#40;弃用的带批注 Xdr 架构 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大容量插入机制，例如 [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT 语句和 bcp 实用工具。 有关详细信息，请参阅 [&#40;transact-sql&#41;](../../../t-sql/statements/bulk-insert-transact-sql.md) 和 [bcp 实用工具](../../../tools/bcp-utility.md)BULK INSERT。  
  
## <a name="streaming-of-xml-data"></a>XML 数据流式处理  
 由于源 XML 文档可能很大，因此无法将整个文档读入内存以进行大容量加载处理。 XML 大容量加载而是将 XML 数据解释为流并读取它。 当该实用工具读取数据时，该工具标识数据库表，并根据 XML 数据源生成相应记录，然后再将这些记录发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 以便插入。  
  
 例如，以下源 XML 文档包含 **\<Customer>** 元素和 **\<Order>** 子元素：  
  
```  
<Customer ...>  
    <Order.../>  
    <Order .../>  
     ...  
</Customer>  
...  
```  
  
 XML 大容量加载读取元素时，将为 **\<Customer>** Customertable 生成记录。 读取 **\</Customer>** 结束标记后，XML 大容量加载会将该记录插入到中的表中 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 同样，在读取 **\<Order>** 元素时，XML 大容量加载将为 orderaddeddate 生成一条记录，然后在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 读取结束标记时将该记录插入到表中 **\</Order>** 。  
  
## <a name="transacted-and-nontransacted-xml-bulk-load-operations"></a>事务和非事务 XML 大容量加载操作  
 XML 大容量加载可以以事务或非事务模式运行。 如果在非事务模式下进行大容量加载，则性能通常是最佳的：也就是说，Transaction 属性设置为 FALSE) 并且满足以下条件之一：  
  
-   要向其大容量加载数据的表为空，且没有任何索引。  
  
-   表具有数据和唯一索引。  
  
 非事务方法不能保证在大容量加载进程发生错误时回滚（但是可以进行部分回滚）。 非事务大容量加载适用于数据库为空的情况。 因此，如果发生错误，您可以清除数据库并重新启动 XML 大容量加载。  
  
> [!NOTE]  
>  在非事务模式下，XML 大容量加载使用并提交默认的内部事务。 当 Transaction 属性设置为 TRUE 时，XML 大容量加载不会对此事务调用 commit。  
  
 如果将 Transaction 属性设置为 TRUE，则 XML 大容量加载会创建临时文件，每个文件对应于映射架构中标识的每个表。 XML 大容量加载首先将源 XML 文档中的记录存储到这些临时文件中。 接着，[!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT 语句检索这些文件中的上述记录，并将其存储到相应的表中。 可以使用 TempFilePath 属性指定这些临时文件的位置。 您必须确保用于 XML 大容量加载的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 帐户有权访问此路径。 如果未指定 TempFilePath 属性，则将使用在 TEMP 环境变量中指定的默认文件路径来创建临时文件。  
  
 如果将 Transaction 属性设置为 FALSE (默认设置) ，XML 大容量加载将使用 OLE DB 接口 IRowsetFastLoad 大容量加载数据。  
  
 如果 ConnectionString 属性设置连接字符串，并且 Transaction 属性设置为 TRUE，则 XML 大容量加载会在其自己的事务上下文中运行。 （例如，XML 大容量加载启动其自己的事务，并根据需要提交或回滚。）  
  
 如果 ConnectionCommand 属性设置了与现有连接对象的连接，并且 Transaction 属性设置为 TRUE，则在成功或失败的情况下，XML 大容量加载不会发出 COMMIT 或 ROLLBACK 语句。 如果出现错误，XML 大容量加载则返回相应的错误消息。 执行 COMMIT 或 ROLLBACK 语句由启动该大容量加载的客户端决定。 用于 XML 大容量加载的 connection 对象应为 ICommand 类型或 ADO 命令对象。  
  
 在 SQLXML 4.0 中，不能将 ConnectionObject 与 Transaction 属性设置为 FALSE 一起使用。 ConnectionObject 不支持非事务模式，因为无法在传入的会话中打开多个 IRowsetFastLoad 接口。  
  
  
