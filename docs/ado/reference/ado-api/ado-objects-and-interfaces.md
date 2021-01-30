---
description: ADO 对象和接口
title: ADO 对象和接口 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, objects and interfaces
- objects [ADO]
ms.assetid: d0b7e254-c89f-4406-b846-a060ef038c30
author: rothja
ms.author: jroth
ms.openlocfilehash: c694a5051729a6ce9b925014ae93b5dbde11f5d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164897"
---
# <a name="ado-objects-and-interfaces"></a>ADO 对象和接口
这些对象之间的关系在 [ADO 对象模型](./ado-object-model.md)中表示。  
  
 每个对象都可以包含在其对应的集合中。 例如， [错误对象可以](./error-object.md) 包含在 [错误](./errors-collection-ado.md) 集合中。 有关详细信息，请参阅 [ADO 集合](./ado-collections.md) 或特定集合主题。  
  
|对象或接口|说明|  
|-|-|  
|[IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))|用于从 ADOCommand 对象检索基础 OLEDB 命令。|  
|[ADORecordConstruction](./adorecordconstruction-interface.md)|从 C/c + + 应用程序中的 OLE DB **行** 对象构造 ADO **记录** 对象。|  
|[ADORecordsetConstruction](./adorecordsetconstruction-interface.md)|从 C/c + + 应用程序中的 OLE DB **行** 集对象构造 ADO **记录集** 对象。|  
|[ADOStreamConstruction 接口](./adostreamconstruction-interface.md)|从 C/c + + 应用程序中的 OLE DB **IStream** 对象构造 ADO **流** 对象。|  
|[命令](./command-object-ado.md)|定义要对数据源执行的特定命令。<br /><br /> **命令** 对象对于脚本编写是不安全的。|  
|[连接](./connection-object-ado.md)|表示到数据源的连接是打开的。<br /><br /> **连接** 对象对于脚本是安全的。|  
|[IDSOShapeExtensions 接口](./idsoshapeextensions-interface.md)|获取形状提供程序的基础 OLEDB 数据源对象。|  
|[错误](./error-object.md)|包含有关与涉及提供程序的单个操作相关的数据访问错误的详细信息。<br /><br /> **错误** 对象对于脚本编写是不安全的。|  
|字段|表示数据类型为通用数据类型的列。|  
|[参数](./parameter-object.md)|表示基于参数化查询或存储过程与 **命令** 对象关联的参数或参数。<br /><br /> **参数** 对象对于脚本编写是不安全的。|  
|[属性](./property-object-ado.md)|表示由提供程序定义的 ADO 对象的动态特性。|  
|[记录](./record-object-ado.md)|表示 **记录集** 的一行或文件系统中的目录或文件。 **记录** 对象对于脚本是安全的。|  
|[Recordset](./recordset-object-ado.md)|表示基表中的记录集或执行的命令的结果。 **记录集** 对象随时仅指集内的单个记录作为当前记录。<br /><br /> 对于脚本编写， **Recordset** 对象是安全的。|  
|[流](./stream-object-ado.md)|表示数据的二进制流。<br /><br /> **流式** 处理对象是安全的。|  
  
## <a name="see-also"></a>另请参阅  
 [ADO API 参考](./ado-api-reference.md)   
 [ADO 集合](./ado-collections.md)   
 [ADO 动态属性](./ado-dynamic-properties.md)   
 [ADO 枚举常量](./ado-enumerated-constants.md)   
 [附录 B： ADO 错误](../../guide/appendixes/appendix-b-ado-errors.md)   
 [ADO 事件](./ado-events.md)   
 [ADO 方法](./ado-methods.md)   
 [ADO 对象模型](./ado-object-model.md)   
 [ADO 属性](./ado-properties.md)