---
description: ADO 属性
title: ADO 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- properties [ADO]
- ADO properties
ms.assetid: 0ac0d1a7-6c7a-4f4c-b115-428935e0f98b
author: rothja
ms.author: jroth
ms.openlocfilehash: bdacd558161c09d1d43f590f9f355e14da262b14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164887"
---
# <a name="ado-properties"></a>ADO 属性

|properties|说明|  
|-|-|  
|[AbsolutePage](./absolutepage-property-ado.md)|指示当前记录所在的页。|  
|[AbsolutePosition](./absoluteposition-property-ado.md)|指示 **记录集** 对象的当前记录的序号位置。|  
|[ActiveCommand](./activecommand-property-ado.md)|指示创建关联 **记录集** 对象的 **命令** 对象。|  
|[ActiveConnection](./activeconnection-property-ado.md)|指示指定的 **命令**、**记录集** 或 **记录** 对象当前属于哪个 **连接** 对象。|  
|[ActualSize](./actualsize-property-ado.md)|指示字段值的实际长度。|  
|[属性](./attributes-property-ado.md)|指示对象的一个或多个特征。|  
|[BOF 和 EOF](./bof-eof-properties-ado.md)|**BOF** 指示当前记录位置位于记录集对象中的第一条记录之前。<br /><br /> **EOF** 指示当前记录位置在 Recordset 对象的最后一条记录之后。|  
|书签 |指示一个书签，该书签唯一标识 **recordset** 对象中的当前记录，或将 **记录集** 对象中的当前记录设置为有效书签标识的记录。|  
|[CacheSize](./cachesize-property-ado.md)|指示 **记录集中** 缓存到内存中的记录集的记录数。|  
|[章节](./chapter-property-ado.md)|获取或设置 **ADORecordsetConstruction** 对象上/上的 OLE DB **章节** 对象。|  
|[字符集](./charset-property-ado.md)|指示文本 **流** 内容应翻译成的字符集。|  
|[CommandStream](./commandstream-property-ado.md)|指示用作 **命令** 对象输入的流。|  
|[CommandText](./commandtext-property-ado.md)|指示要对提供程序发出的命令的文本。|  
|[CommandTimeout](./commandtimeout-property-ado.md)|指示在终止尝试并生成错误之前执行命令时等待的时间。|  
|[CommandType](./commandtype-property-ado.md)|指示 **命令** 对象的类型。|  
|[ConnectionString 属性](./connectionstring-property-ado.md)|指示用于建立与数据源的连接的信息。|  
|[ConnectionTimeout](./connectiontimeout-property-ado.md)|指示在终止尝试并生成错误之前建立连接时等待的时间。|  
|“计数”|指示集合中的对象数。|  
|[CursorLocation](./cursorlocation-property-ado.md)|指示游标服务的位置。|  
|[CursorType](./cursortype-property-ado.md)|指示 **记录集** 对象中使用的游标的类型。|  
|[DataMember](./datamember-property.md)|指示将从 **DataSource** 属性所引用的对象中检索的数据成员的名称。|  
|[DataSource](./datasource-property-ado.md)|指示包含要表示为 **记录集** 对象的数据的对象。|  
|[DefaultDatabase](./defaultdatabase-property.md)|指示 **连接** 对象的默认数据库。|  
|[DefinedSize](./definedsize-property.md)|指示 **Field** 对象的数据容量。|  
|[说明](./description-property.md)|描述 **错误** 对象。|  
|[Dialect-p](./dialect-property.md)|指示提供程序将用于分析 **CommandText** 或 **CommandStream** 属性的语法和一般规则。|  
|[方向](./direction-property.md)|指示 **参数** 是表示输入参数和/或输出参数，还是如果参数是来自存储过程的返回值，则为。|  
|[EditMode](./editmode-property.md)|指示当前记录的编辑状态。|  
|[EOS](./eos-property.md)|指示当前位置是否在流的末尾。|  
|[Filter](./filter-property.md)|指示 **记录集中** 数据的筛选器。|  
|[HelpContext 和帮助](./helpcontext-helpfile-properties.md)|指示与 **错误** 对象关联的帮助文件和主题。<br /><br /> "上下文 ID" 为帮助文件中的 **主题返回一个** 上下文 ID，作为一个 **长** 值。<br /><br /> "**帮助文件" 返回一个****字符串** 值，该值的计算结果为帮助文件的完全解析路径。|  
|[Index](./index-property.md)|指示当前对 **记录集** 对象有效的索引的名称。|  
|[IsolationLevel](./isolationlevel-property.md)|指示 **连接** 对象的隔离级别。|  
|[项目](./item-property-ado.md)|按名称或序号指示集合中的特定成员。|  
|[LineSeparator](./lineseparator-property-ado.md)|指示要在文本 **流** 对象中用作行分隔符的二进制字符。|  
|[LockType](./locktype-property-ado.md)|指示在编辑过程中放置在记录上的锁的类型。|  
|[MarshalOptions](./marshaloptions-property-ado.md)|指示哪些记录将被封送回服务器。|  
|[MaxRecords](./maxrecords-property-ado.md)|指示要从查询返回到 **记录集** 的最大记录数。|  
|[模式](./mode-property-ado.md)|指示用于修改 **连接**、 **记录** 或 **流** 对象中的数据的可用权限。|  
|[名称](./name-property-ado.md)|指示对象的名称。|  
|[NativeError](./nativeerror-property-ado.md)|指示特定 **错误** 对象的特定于提供程序的错误代码。|  
|[数字](./number-property-ado.md)|指示唯一标识 **错误** 对象的数字。|  
|[NumericScale](./numericscale-property-ado.md)|指示 **参数** 或 **字段** 对象中数值的小数位数。|  
|[OriginalValue](./originalvalue-property-ado.md)|指示在进行任何更改之前，记录中是否存在 **字段** 的值。|  
|[PageCount](./pagecount-property-ado.md)|指示 **Recordset** 对象包含多少页数据。|  
|[PageSize](./pagesize-property-ado.md)|指示记录 **集中** 的一页的记录数。|  
|[ParentRow](./parentrow-property-ado.md)|设置 **ADORecordConstruction** 对象上 OLE DB **行** 对象的容器，使该行的父对象变为 ADO **记录** 对象。|  
|[ParentURL](./parenturl-property-ado.md)|指示指向当前 **记录** 对象的父 **记录** 的绝对 URL 字符串。|  
|[位置](./position-property-ado.md)|指示 **流** 对象内的当前位置。|  
|[精度](./precision-property-ado.md)|指示 **参数** 对象或数值 **字段** 对象中数值的精度度。|  
|[Prepared](./prepared-property-ado.md)|指示是否在执行前保存命令的已编译版本。|  
|[提供程序](./provider-property-ado.md)|指示 **连接** 对象的访问接口的名称。|  
|[RecordCount](./recordcount-property-ado.md)|指示 **记录集** 对象中的记录数。|  
|[RecordType](./recordtype-property-ado.md)|指示 **记录** 对象的类型。|  
|[总行](./row-property-ado.md)|获取或设置 **ADORecordConstruction** 对象上/的 OLE DB **行** 对象。|  
|[RowPosition](./rowposition-property-ado.md)|获取或设置 **ADORecordsetConstruction** 对象上/的 OLE DB **RowPosition** 对象。|  
|[行集](./rowset-property-ado.md)|获取或设置 **ADORecordsetConstruction** 对象上/的 OLE DB **行集** 对象。|  
|[ (ADO 错误的源) ](./source-property-ado-error.md)|指示最初生成错误的对象或应用程序的名称。|  
|[源代码 (ADO 记录) ](./source-property-ado-record.md)|指示 **Record** 对象表示的实体。|  
|[ADO 记录集 (源) ](./source-property-ado-recordset.md)|指示 **记录集** 对象中数据的源|  
|[SQLState](./sqlstate-property.md)|指示特定 **错误** 对象的 SQL 状态。|  
|[状态](./state-property-ado.md)|指示对象的状态是打开还是关闭的所有适用对象。 对于执行异步方法的所有适用对象，指示对象的当前状态是正在连接、正在执行还是正在检索|  
|[ADO 字段 (状态) ](./status-property-ado-field.md)|指示 **Field** 对象的状态。|  
|[ADO 记录集 (状态) ](./status-property-ado-recordset.md)|指示当前记录的有关批更新或其他大容量操作的状态。|  
|[StayInSync](./stayinsync-property.md)|指示在分层 **记录集** 对象中，对基础子记录的引用 (即，该 *章节*) 父行位置发生更改时所做的更改。|  
|[Stream 属性](./stream-property.md)|获取或设置 **ADOStreamConstruction** 对象上/的 OLE DB **流** 对象。|  
|[Type](./type-property-ado.md)|指示 **参数**、 **字段** 或 **属性** 对象的操作类型或数据类型。|  
|[键入 (ADO Stream) ](./type-property-ado-stream.md)|指示 **流** 中包含的数据的类型 (二进制或文本) 。|  
|[UnderlyingValue](./underlyingvalue-property.md)|指示数据库中 **字段** 对象的当前值。|  
|值|指示赋给 **字段**、 **参数** 或 **属性** 对象的值。|  
|[版本](./version-property-ado.md)|指示 ADO 版本号。|  
  
## <a name="see-also"></a>另请参阅  
 [ADO API 参考](./ado-api-reference.md)   
 [ADO 集合](./ado-collections.md)   
 [ADO 动态属性](./ado-dynamic-properties.md)   
 [ADO 枚举常量](./ado-enumerated-constants.md)   
 [附录 B： ADO 错误](../../guide/appendixes/appendix-b-ado-errors.md)   
 [ADO 事件](./ado-events.md)   
 [ADO 方法](./ado-methods.md)   
 [ADO 对象模型](./ado-object-model.md)   
 [ADO 对象和接口](./ado-objects-and-interfaces.md)