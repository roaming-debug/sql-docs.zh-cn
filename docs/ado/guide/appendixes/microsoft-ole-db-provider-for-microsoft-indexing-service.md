---
description: Microsoft OLE DB Microsoft 索引服务提供商概述
title: Microsoft OLE DB Provider for Microsoft 索引服务 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Indexing Service provider [ADO]
- providers [ADO], OLE DB provider for Microsoft Indexing service
- OLE DB provider for Microsoft Indexing service [ADO]
ms.assetid: f86a0598-5097-471b-8318-d2c859d085f2
author: rothja
ms.author: jroth
ms.openlocfilehash: f7d2204a2d91e862616713dbfee6eec3d6d3e8ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029301"
---
# <a name="microsoft-ole-db-provider-for-microsoft-indexing-service-overview"></a>Microsoft OLE DB Microsoft 索引服务提供商概述
Microsoft OLE DB Provider for Microsoft 索引服务提供对 Microsoft 索引服务编制索引的文件系统和 Web 数据的只读访问。 ADO 应用程序可以发出 SQL 查询来检索内容和文件属性信息。

 提供程序已启用自由线程和 UNICODE。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到该提供程序，请将 **提供程序 =** 参数设置为 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) 属性，以执行以下操作：

```vb
MSIDXS
```

 读取 [提供程序](../../reference/ado-api/provider-property-ado.md) 属性也会返回此字符串。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```vb
"Provider=MSIDXS;Data Source=myCatalog;Locale Identifier=nnnn;"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定 Microsoft 索引服务的 OLE DB 提供程序。 通常，这是在连接字符串中指定的唯一关键字。|
|**数据源**|指定索引服务目录名称。 如果未指定此关键字，则使用默认系统目录。|
|**区域设置标识符**|指定唯一的32位数字 (例如，1033) 指定与用户语言相关的首选项。 如果未指定此关键字，则使用默认的系统区域设置标识符。|

## <a name="command-text"></a>命令文本
 索引服务 SQL 查询语法由 SQL-92 **SELECT** 语句的扩展及其 **FROM** 和 **WHERE** 子句组成。 查询的结果通过 OLE DB 行集返回，这些行集可由 ADO 使用并作为 [Recordset](../../reference/ado-api/recordset-object-ado.md) 对象进行操作。

 可以搜索确切的单词或短语，也可以使用通配符搜索单词的模式或词干。 搜索逻辑可以基于布尔值决策、加权字词或接近其他单词。 您还可以通过 "自由文本" 进行搜索，该文本根据含义（而不是精确的单词）查找匹配项。

 特定的命令方言完全记录在用于索引服务的查询语言文档中。

 提供程序不接受存储过程调用或简单的表名称 (例如， [CommandType](../../reference/ado-api/commandtype-property-ado.md) 属性将始终) 为 **adCmdText** 。

## <a name="recordset-behavior"></a>记录集行为
 下表列出了通过此提供程序打开的 **记录集** 对象的可用功能。 只有静态游标类型 (**adOpenStatic**) 可用。

 有关提供程序配置的 **记录集** 行为的详细信息，请运行 [支持](../../reference/ado-api/supports-method.md)方法，并枚举 **记录集** 的 [Properties](../../reference/ado-api/properties-collection-ado.md)集合，以确定是否存在特定于提供程序的动态属性。

 **标准 ADO 记录集属性的可用性：**

|属性|可用性|
|--------------|------------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|读/写|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|读/写|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|只读|
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|只读|
|[书签](../../reference/ado-api/bookmark-property-ado.md)*|读/写|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|读/写|
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|始终 **adUseServer**|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|始终 **adOpenStatic**|
|[EditMode](../../reference/ado-api/editmode-property.md)|始终 **adEditNone**|
|[EOF](../../reference/ado-api/bof-eof-properties-ado.md)|只读|
|[Filter](../../reference/ado-api/filter-property.md)|读/写|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|读/写|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|不可用|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|读/写|
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|只读|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|读/写|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|只读|
|[Source](../../reference/ado-api/source-property-ado-recordset.md)|读/写|
|[State](../../reference/ado-api/state-property-ado.md)|只读|
|[Status](../../reference/ado-api/status-property-ado-recordset.md)|只读|

 \*必须在提供程序中启用书签，此功能才能存在于 **记录集中**。

 **标准 ADO 记录集方法的可用性：**

|方法|是否可用？|
|------------|----------------|
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|否|
|[取消](../../reference/ado-api/cancel-method-ado.md)|是|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|否|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|否|
|[克隆](../../reference/ado-api/clone-method-ado.md)|是|
|[关闭](../../reference/ado-api/close-method-ado.md)|是|
|[删除](../../reference/ado-api/delete-method-ado-recordset.md)|否|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|是|
|[移动](../../reference/ado-api/move-method-ado.md)|是|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)|是|
|[打开](../../reference/ado-api/open-method-ado-recordset.md)|是|
|[重新](../../reference/ado-api/requery-method.md)|是|
|[重新同步](../../reference/ado-api/resync-method.md)|是|
|[支持](../../reference/ado-api/supports-method.md)|是|
|[Update](../../reference/ado-api/update-method.md)|否|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|否|

 有关 microsoft OLE DB 提供商 for Microsoft 索引服务的具体实现详细信息和功能信息，请参阅 [OLE DB 程序员指南](/previous-versions/windows/desktop/ms713643(v=vs.85))，或访问 Windows NT Server 网站的 "Web 服务" 页。

## <a name="see-also"></a>另请参阅
 [CommandType 属性 (ado) ](../../reference/ado-api/commandtype-property-ado.md) [CONNECTIONSTRING 属性 (Ado) ](../../reference/ado-api/connectionstring-property-ado.md) [Properties Collection ](../../reference/ado-api/properties-collection-ado.md) (ado) [提供程序属性 ](../../reference/ado-api/provider-property-ado.md) (Ado) [Recordset 对象 ](../../reference/ado-api/recordset-object-ado.md) (ado) [支持方法](../../reference/ado-api/supports-method.md)