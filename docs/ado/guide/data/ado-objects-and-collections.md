---
description: ADO 对象和集合
title: ADO 对象和集合 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, objects and collections
ms.assetid: 7a745aae-9372-49b6-8dae-b9c93e5f3216
author: rothja
ms.author: jroth
ms.openlocfilehash: c03e1b792e23552252eb84fae9a71e693b3781c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028062"
---
# <a name="ado-objects-and-collections"></a>ADO 对象和集合
ADO 包含以下九个对象和四个集合。  
  
|对象或集合|说明|  
|--------------------------|-----------------|  
|**Connection** 对象|表示与某一数据源的唯一会话。 对于客户端/服务器数据库系统，它可能等效于到服务器的实际网络连接。 根据提供程序支持的功能， **连接** 对象的某些集合、方法或属性可能不可用。|  
|**Command** 对象|用于定义要针对数据源运行的特定命令，如 SQL 查询。|  
|**Recordset** 对象|表示基表中的整个记录集或执行的命令的结果。 所有 **记录集** 对象都包含 (行) 和字段 (列) 中的记录。|  
|**Record** 对象|表示单个数据行，从 **记录集** 或从提供程序中。 此记录可以表示数据库记录或某些其他类型的对象（如文件或目录），这取决于您的提供程序。|  
|**Stream** 对象|表示二进制或文本数据的流。 例如，可以将 XML 文档加载到流中以执行命令输入，或者将其作为查询结果从特定提供程序返回。 **流** 对象可用于操作包含这些数据流的字段或记录。|  
|**参数** 对象|根据参数化查询或存储过程，表示与 **命令** 对象关联的参数或参数。|  
|**Field** 对象|表示数据类型为通用数据类型的列。 每个 **字段** 对象都对应于 **记录集中** 的一列。|  
|**Property** 对象|表示由提供程序定义的 ADO 对象的特性。 ADO 对象具有两种类型的属性：内置和动态。 内置属性是在 ADO 中实现并立即可用于任何新对象的属性。 **属性** 对象是动态属性的容器，由基础提供程序定义。|  
|**Error** 对象|包含有关与涉及提供程序的单个操作相关的数据访问错误的详细信息。|  
|**字段** 集合|包含 **Recordset** 或 **Record** 对象的所有 **字段** 对象。|  
|**Properties** 集合|包含对象的特定实例的所有 **属性** 对象。|  
|**Parameters** 集合|包含 **Command** 对象的所有 **参数** 对象。|  
|**错误** 集合|包含为响应单个提供程序相关的失败而创建的所有 **错误** 对象。|  
  
## <a name="see-also"></a>另请参阅  
 [ADO 对象模型](../../reference/ado-api/ado-object-model.md)