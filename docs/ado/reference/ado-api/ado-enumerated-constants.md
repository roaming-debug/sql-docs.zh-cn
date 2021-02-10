---
description: ADO 枚举常量
title: ADO 枚举的常量 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- enumerated constants [ADO]
ms.assetid: c97ed131-1a93-463c-9e61-22f029b0c474
author: rothja
ms.author: jroth
ms.openlocfilehash: a0295fd9f1feec8db54734a5dfc02eba483e1615
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031279"
---
# <a name="ado-enumerated-constants"></a>ADO 枚举常量
为了帮助进行调试，ADO 枚举列出了每个常量的值。 不过，此值是纯粹的建议，可能会从 ADO 的一种版本更改为另一版本。 你的代码只应依赖于每个枚举常量的名称，而不是实际值。  
  
|返回的常量|说明|  
|-|-|  
|[ADCPROP_ASYNCTHREADPRIORITY_ENUM](./adcprop-asyncthreadpriority-enum.md)|对于 RDS **记录集** 对象，指定检索数据的异步线程的执行优先级。|  
|[ADCPROP_AUTORECALC_ENUM](./adcprop-autorecalc-enum.md)|指定 **MSDataShape** 提供程序何时重新计算分层 **记录集中** 的聚合和计算列。|  
|[ADCPROP_UPDATECRITERIA_ENUM](./adcprop-updatecriteria-enum.md)|指定哪些字段可用于在使用 **记录集** 对象的数据源的行的开放式更新过程中检测冲突。|  
|[ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md)|指定 **UpdateBatch** 方法是否后跟隐式重新 **同步** 方法操作，如果是，则为该操作的范围。|  
|[AffectEnum](./affectenum.md)|指定哪些记录受操作影响。|  
|[BookmarkEnum](./bookmarkenum.md)|指定指示操作开始位置的书签。|  
|[CommandTypeEnum](./commandtypeenum.md)|指定如何解释命令参数。|  
|[CompareEnum](./compareenum.md)|指定由书签表示的两个记录的相对位置。|  
|[ConnectModeEnum](./connectmodeenum.md)|指定可用于修改 **连接** 中的数据、打开 **记录** 或为 **Record** 和 **Stream** 对象的 **Mode** 属性指定值的可用权限。|  
|[ConnectOptionEnum](./connectoptionenum.md)|指定 **连接** 对象的 **Open** 方法是否应在 (同步之后返回同步) ，或在建立连接)  (异步之前返回。|  
|[ConnectPromptEnum](./connectpromptenum.md)|指定在打开与 ODBC 数据源的连接时是否应显示一个对话框，以提示输入缺少的参数。|  
|[CopyRecordOptionsEnum](./copyrecordoptionsenum.md)|指定 **CopyRecord** 方法的行为。|  
|[CursorLocationEnum](./cursorlocationenum.md)|指定游标引擎的位置。|  
|[CursorOptionEnum](./cursoroptionenum.md)|指定 **支持** 方法应对哪些功能进行测试。|  
|[CursorTypeEnum](./cursortypeenum.md)|指定 **记录集** 对象中使用的游标类型。|  
|[DataTypeEnum](./datatypeenum.md)|指定 **字段**、 **参数** 或 **属性** 的数据类型。|  
|[EditModeEnum](./editmodeenum.md)|指定记录的编辑状态。|  
|[ErrorValueEnum](./errorvalueenum.md)|指定 ADO 运行时错误的类型。|  
|[EventReasonEnum](./eventreasonenum.md)|指定导致事件发生的原因。|  
|[EventStatusEnum](./eventstatusenum.md)|指定事件的当前执行状态。|  
|[ExecuteOptionEnum](./executeoptionenum.md)|指定提供程序执行命令的方式。|  
|[FieldEnum](./fieldenum.md)|指定 **记录** 对象的 **fields** 集合中引用的特殊字段。|  
|[FieldAttributeEnum](./fieldattributeenum.md)|指定 **Field** 对象的一个或多个属性。|  
|[FieldStatusEnum](./fieldstatusenum.md)|指定 **字段** 对象的状态。|  
|[FilterGroupEnum](./filtergroupenum.md)|指定要从 **记录集中** 筛选的一组记录。|  
|[GetRowsOptionEnum](./getrowsoptionenum.md)|指定要从 **记录集中** 检索的记录数。|  
|[IsolationLevelEnum](./isolationlevelenum.md)|指定 **连接** 对象的事务隔离级别。|  
|[LineSeparatorsEnum](./lineseparatorsenum.md)|指定在文本 **流** 对象中用作行分隔符的字符。|  
|[LockTypeEnum](./locktypeenum.md)|指定在编辑过程中放置在记录上的锁的类型。|  
|[MarshalOptionsEnum](./marshaloptionsenum.md)|指定应返回到服务器的记录。|  
|[MoveRecordOptionsEnum](./moverecordoptionsenum.md)|指定 **Record** object **MoveRecord** 方法的行为。|  
|[ObjectStateEnum](./objectstateenum.md)|指定是打开还是关闭对象、连接到数据源、执行命令或获取数据。|  
|[ParameterAttributesEnum](./parameterattributesenum.md)|指定 **参数** 对象的特性。|  
|[ParameterDirectionEnum](./parameterdirectionenum.md)|指定 **参数** 是否表示输入参数和/或输出参数，如果参数是来自存储过程的返回值，则为。|  
|[PersistFormatEnum](./persistformatenum.md)|指定保存 **记录集** 的格式。|  
|[PositionEnum](./positionenum.md)|指定记录指针在 **记录集中** 的当前位置。|  
|[PropertyAttributesEnum](./propertyattributesenum.md)|指定 **属性** 对象的特性。|  
|[RecordCreateOptionsEnum](./recordcreateoptionsenum.md)|为 **记录** 对象 **Open** 方法指定是否应打开现有 **记录** ，或者是否创建新 **记录** 。|  
|[RecordOpenOptionsEnum](./recordopenoptionsenum.md)|指定用于打开 **记录** 的选项。 可以使用 OR 运算符组合这些值。|  
|[RecordStatusEnum](./recordstatusenum.md)|指定有关批更新和其他批量操作的记录的状态。|  
|[RecordTypeEnum](./recordtypeenum.md)|指定 **记录** 对象的类型。|  
|[ResyncEnum](./resyncenum.md)|指定是否通过调用 **Resync** 覆盖基础值。|  
|[SaveOptionsEnum](./saveoptionsenum.md)|指定在从 **流** 对象保存时是否应创建或覆盖文件。 值可以与 AND 运算符组合。|  
|[SchemaEnum](./schemaenum.md)|指定 **OpenSchema** 方法检索的架构 **记录集** 的类型。 指定记录 **集** 内的记录搜索的方向。|  
|[SearchDirectionEnum](./searchdirectionenum.md)|指定记录 **集** 内的记录搜索的方向。 指定要执行的 **搜索** 的类型。|  
|[SeekEnum](./seekenum.md)|指定要执行的 **搜索** 的类型。 指定用于打开 **流** 对象的选项。 值可以与 AND 运算符组合。|  
|[StreamOpenOptionsEnum](./streamopenoptionsenum.md)|指定用于打开 **流** 对象的选项。 值可以与 AND 运算符组合。 指定是应从 **流** 对象中读取整个流还是下一行。|  
|[StreamReadEnum](./streamreadenum.md)|指定是应从 **流** 对象中读取整个流还是下一行。 指定 **流** 对象中存储的数据的类型。|  
|[StreamTypeEnum](./streamtypeenum.md)|指定 **流** 对象中存储的数据的类型。 指定是否将行分隔符追加到写入 **流** 对象的字符串。|  
|[StreamWriteEnum](./streamwriteenum.md)|指定是否将行分隔符追加到写入 **流** 对象的字符串。 指定以字符串形式检索 **记录集** 时的格式。|  
|[StringFormatEnum](./stringformatenum.md)|指定以字符串形式检索 **记录集** 时的格式。 指定 **连接** 对象的事务特性。|  
|[XactAttributeEnum](./xactattributeenum.md)|指定 **连接** 对象的事务特性。|  
  
## <a name="see-also"></a>另请参阅  
 [ADO API 参考](./ado-api-reference.md)   
 [ADO 集合](./ado-collections.md)   
 [ADO 动态属性](./ado-dynamic-properties.md)   
 [附录 B： ADO 错误](../../guide/appendixes/appendix-b-ado-errors.md)   
 [ADO 事件](./ado-events.md)   
 [ADO 方法](./ado-methods.md)   
 [ADO 对象模型](./ado-object-model.md)   
 [ADO 对象和接口](./ado-objects-and-interfaces.md)   
 [ADO 属性](./ado-properties.md)