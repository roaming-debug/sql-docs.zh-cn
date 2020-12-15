---
description: '本机客户端 OLE DB 提供程序 (的行集和 SQL Server 游标) '
title: '本机客户端 OLE DB 提供程序 (的行集和 SQL Server 游标) '
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB rowsets, cursors
- SQL Server Native Client OLE DB provider, rowsets
- rowsets [OLE DB], cursors
- properties [OLE DB]
- cursors [OLE DB]
ms.assetid: 26a11e26-2a3a-451e-8f78-fba51e330ecb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3de88fa5e6e662836a55a715daa33cb5c102674e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476108"
---
# <a name="rowsets-and-sql-server-cursors-native-client-ole-db-provider"></a>本机客户端 OLE DB 提供程序 (的行集和 SQL Server 游标) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用两种方法将结果集返回到使用者：  
  
-   默认结果集，它可以：  
  
    -   使开销最小化。  
  
    -   在提取数据过程中提供最大性能。  
  
    -   仅支持默认的只进、只读游标功能。  
  
    -   每次一行将行返回到使用者。  
  
    -   在一个连接上每次仅支持一个活动语句。  
  
         执行完某语句之后，直到使用者已检索所有结果或已取消此语句，才能在连接上执行其他语句。  
  
    -   支持所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   服务器游标，它可以：  
  
    -   支持所有游标功能。  
  
    -   将行块返回到使用者。  
  
    -   在单个连接上支持多个活动语句。  
  
    -   根据性能平衡游标功能。  
  
         相对于默认结果集，对游标功能的支持会使性能降低。 如果使用者可以使用游标功能检索更小的行集，则可以将其抵销。  
  
    -   不支持任何返回多个结果集的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 通过设置某些行集属性，使用者可以请求行集中的其他游标行为。 如果使用者未设置这些行集属性中的任何一个，或将它们全部设置为其默认值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序使用默认结果集实现行集。 如果将这些属性中的任何一个设置为默认值以外的值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将使用服务器游标实现行集。  
  
 以下行集属性指引 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标。 某些属性可以安全地与其他属性组合。 例如，展示 DBPROP_IRowsetScroll 和 DBPROP_IRowsetChange 属性的行集将是展示立即更新行为的书签行集。 其他属性相互排斥。 例如，展示 DBPROP_OTHERINSERT 的行集不能包含书签。  
  
|属性 ID|值|行集行为|  
|-----------------|-----------|---------------------|  
|DBPROP_SERVERCURSOR|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集是有顺序的，只支持向前滚动和提取。 支持相对行定位。 命令文本可以包含 ORDER BY 子句。|  
|DBPROP_CANSCROLLBACKWARDS 或 DBPROP_CANFETCHBACKWARDS|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集支持双向滚动和提取。 支持相对行定位。 命令文本可以包含 ORDER BY 子句。|  
|DBPROP_BOOKMARKS 或 DBPROP_LITERALBOOKMARKS|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集是有顺序的，只支持向前滚动和提取。 支持相对行定位。 命令文本可以包含 ORDER BY 子句。|  
|DBPROP_OWNUPDATEDELETE 或 DBPROP_OWNINSERT 或 DBPROP_OTHERUPDATEDELETE|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集支持双向滚动和提取。 支持相对行定位。 命令文本可以包含 ORDER BY 子句。|  
|DBPROP_OTHERINSERT|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集支持双向滚动和提取。 支持相对行定位。 如果针对被引用的列存在索引，则命令文本可以包括 ORDER BY 子句。<br /><br /> 如果行集包含书签，则 DBPROP_OTHERINSERT 不能是 VARIANT_TRUE。 如果试图创建具有该可视性属性和书签的行集，将导致错误。|  
|DBPROP_IRowsetLocate 或 DBPROP_IRowsetScroll|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集支持双向滚动和提取。 支持在行集中通过 IRowsetLocate 接口执行书签操作和绝对定位  。 命令文本可以包含 ORDER BY 子句。<br /><br /> DBPROP_IRowsetLocate 和 DBPROP_IRowsetScroll 需要行集中有书签。 如果试图创建具有书签并将 DBPROP_OTHERINSERT 设置为 VARIANT_TRUE 的行集，将导致错误。|  
|DBPROP_IRowsetChange 或 DBPROP_IRowsetUpdate|VARIANT_TRUE|可以通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集是有顺序的，只支持向前滚动和提取。 支持相对行定位。 支持可更新游标的所有命令都可以支持这些接口。|  
|DBPROP_IRowsetLocate 或 DBPROP_IRowsetScroll 和 DBPROP_IRowsetChange 或 DBPROP_IRowsetUpdate|VARIANT_TRUE|可以通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集支持双向滚动和提取。 支持在行集中通过 IRowsetLocate 执行书签操作和绝对定位  。 命令文本可以包含 ORDER BY 子句。|  
|DBPROP_IMMOBILEROWS|VARIANT_FALSE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集只支持向前滚动。 支持相对行定位。 如果针对被引用的列存在索引，则命令文本可以包括 ORDER BY 子句。<br /><br /> DBPROP_IMMOBILEROWS 仅在可以显示由其他会话的命令或由其他用户插入的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 行的行集中可用。 如果试图在其 DBPROP_OTHERINSERT 不能是 VARIANT_TRUE 的任何行集上打开一个该属性设置为 VARIANT_FALSE 的行集，将导致错误。|  
|DBPROP_REMOVEDELETED|VARIANT_TRUE|无法通过行集更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。 行集只支持向前滚动。 支持相对行定位。 除非受另一个属性约束，否则命令文本可以包含 ORDER BY 子句。|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]可以通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **IOpenRowset：： OpenRowset** 方法，在基础表或视图上轻松创建服务器游标支持的 Native Client OLE DB 提供程序行集。 按名称指定表或视图，同时在 rgPropertySets 参数中传递所需的行集属性集  。  
  
 当使用者需要服务器游标支持行集时，创建行集的命令文本将受到限制。 具体来说，命令文本只能是返回单个行集结果的单个 SELECT 语句，或实现返回单个行集结果的单个 SELECT 语句的存储过程。  
  
 以下两个表显示各种 OLE DB 属性与各游标模型之间的一一对应关系。 它们还显示使用某一类型游标模型时应当设置哪些行集属性。  
  
 表中的每个单元格都包含相应行集属性对于特定游标模型的值。 本主题前面列出的所有行集属性的数据类型都是 VT_BOOL，并且默认值都是 VARIANT_FALSE。 下面的表使用了以下符号。  
  
 F = 默认值 (VARIANT_FALSE)  
  
 T = VARIANT_TRUE  
  
 \- = VARIANT_TRUE 或 VARIANT_FALSE  
  
 若要使用某一类型的游标模型，请找到对应于该游标模型的列，并查找在此列中值为“T”的所有行集属性。 将这些行集属性设置为 VARIANT_TRUE 即可使用特定游标模型。 可以将值为“-”的行集属性设置为 VARIANT_TRUE 或 VARIANT_FALSE。  
  
|行集属性/游标模型|默认<br /><br /> result<br /><br /> set<br /><br /> (RO)|Fast<br /><br /> 只<br /><br /> 进<br /><br /> (RO)|静态<br /><br /> (RO)|Keyset<br /><br /> 驱动<br /><br /> (RO)|  
|--------------------------------------|-------------------------------------------|--------------------------------------------|-----------------------|----------------------------------|  
|DBPROP_SERVERCURSOR|F|T|T|T|  
|DBPROP_DEFERRED|F|F|-|-|  
|DBPROP_IrowsetChange|F|F|F|F|  
|DBPROP_IrowsetLocate|F|F|-|-|  
|DBPROP_IrowsetScroll|F|F|-|-|  
|DBPROP_IrowsetUpdate|F|F|F|F|  
|DBPROP_BOOKMARKS|F|F|-|-|  
|DBPROP_CANFETCHBACKWARDS|F|F|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|F|F|-|-|  
|DBPROP_CANHOLDROWS|F|F|-|-|  
|DBPROP_LITERALBOOKMARKS|F|F|-|-|  
|DBPROP_OTHERINSERT|F|T|F|F|  
|DBPROP_OTHERUPDATEDELETE|F|T|F|T|  
|DBPROP_OWNINSERT|F|T|F|T|  
|DBPROP_OWNUPDATEDELETE|F|T|F|T|  
|DBPROP_QUICKSTART|F|F|-|-|  
|DBPROP_REMOVEDELETED|F|F|F|-|  
|DBPROP_IrowsetResynch|F|F|F|-|  
|DBPROP_CHANGEINSERTEDROWS|F|F|F|F|  
|DBPROP_SERVERDATAONINSERT|F|F|F|-|  
|DBPROP_UNIQUEROWS|-|F|F|F|  
|DBPROP_IMMOBILEROWS|-|-|-|T|  
  
|行集属性/游标模型|动态 (RO)|键集 (R/W)|动态 (R/W)|  
|--------------------------------------|--------------------|---------------------|----------------------|  
|DBPROP_SERVERCURSOR|T|T|T|  
|DBPROP_DEFERRED|-|-|-|  
|DBPROP_IrowsetChange|F|-|-|  
|DBPROP_IrowsetLocate|F|-|F|  
|DBPROP_IrowsetScroll|F|-|F|  
|DBPROP_IrowsetUpdate|F|-|-|  
|DBPROP_BOOKMARKS|F|-|F|  
|DBPROP_CANFETCHBACKWARDS|-|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|-|-|-|  
|DBPROP_CANHOLDROWS|F|-|F|  
|DBPROP_LITERALBOOKMARKS|F|-|F|  
|DBPROP_OTHERINSERT|T|F|T|  
|DBPROP_OTHERUPDATEDELETE|T|T|T|  
|DBPROP_OWNINSERT|T|T|T|  
|DBPROP_OWNUPDATEDELETE|T|T|T|  
|DBPROP_QUICKSTART|-|-|-|  
|DBPROP_REMOVEDELETED|T|-|T|  
|DBPROP_IrowsetResynch|-|-|-|  
|DBPROP_CHANGEINSERTEDROWS|F|-|F|  
|DBPROP_SERVERDATAONINSERT|F|-|F|  
|DBPROP_UNIQUEROWS|F|F|F|  
|DBPROP_IMMOBILEROWS|F|T|F|  
  
 对于一组特定的行集属性，将按如下方式确定选择的游标模型。  
  
 从这组指定的行集属性，获得在以前的表中列出的属性的子集。 根据每个行集属性的必需（T、F）或可选 (-) 标志值，将这些属性划分到两个子组中。 对于每个游标模型，从第一个表开始，并从左向右移动。 将两个子组中属性的值与相应列中相应属性的值进行比较。 将选择与必需的属性完全匹配以及与可选的属性有最少不匹配数的游标模型。 如果有多个游标模型，则选择最左侧的游标模型。  
  
## <a name="sql-server-cursor-block-size"></a>SQL Server 游标块大小  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序行集时， **IRowset：： GetNextRows** 或 **IRowsetLocate：： GetRowsAt** 方法的行句柄数组参数中的元素数目将定义游标块大小。 该数组中的控点指示的行是游标块的成员。  
  
 对于支持书签的行集，游标块的成员由通过 IRowsetLocate::GetRowsByBookmark 方法检索到的行控点进行定义。  
  
 不管使用什么方法填充行集和形成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标块，游标块都会处于活动状态，直至对行集执行下一个行提取方法。  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
