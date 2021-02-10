---
description: Refresh 方法 (ADO)
title: " (ADO) 刷新方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Refresh
- Parameters::Refresh
- Properties::Refresh
helpviewer_keywords:
- Refresh method [ADO]
ms.assetid: 089b7ca7-684f-4259-8032-5bd1ecc54426
author: rothja
ms.author: jroth
ms.openlocfilehash: ea82fa376046477277398012236559b08a6fe1a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051678"
---
# <a name="refresh-method-ado"></a>Refresh 方法 (ADO)
更新集合中的对象，以反映提供程序提供的和特定于提供程序的对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
collection.Refresh  
```  
  
## <a name="remarks"></a>备注  
 **Refresh** 方法完成不同的任务，具体取决于调用该方法的集合。  
  
### <a name="parameters"></a>参数  
 对 [command](./command-object-ado.md)对象的 [Parameters](./parameters-collection-ado.md)集合使用 **Refresh** 方法将检索在 **命令** 对象中指定的存储过程或参数化查询的提供程序端参数信息。 对于不支持存储过程调用或参数化查询的访问接口，该集合将为空。  
  
 在调用 **Refresh** 方法之前，应将 **命令** 对象的 [ActiveConnection](./activeconnection-property-ado.md)属性设置为有效的 [连接](./connection-object-ado.md)对象，将 [CommandText](./commandtext-property-ado.md)属性设置为有效的命令，并将 [CommandType](./commandtype-property-ado.md)属性设置为 **adCmdStoredProc** 。  
  
 如果在调用 **Refresh** 方法之前访问 **参数** 集合，则 ADO 将自动调用方法并填充集合。  
  
> [!NOTE]
>  如果使用 **Refresh** 方法从提供程序中获取参数信息，并返回一个或多个可变长度的数据类型 [参数](./parameter-object.md) 对象，则 ADO 可能会根据其最大可能大小为参数分配内存，这将导致执行过程中出错。 在调用[Execute](./execute-method-ado-command.md)方法以防止错误之前，应显式设置这些参数的[大小](./size-property-ado-parameter.md)属性。  
  
### <a name="fields"></a>字段  
 对 [字段](./fields-collection-ado.md)集合使用 **Refresh** 方法不会产生任何效果。 若要从基础数据库结构中检索更改，必须使用 [Requery](./requery-method.md) 方法，如果 [Recordset](./recordset-object-ado.md) 对象不支持书签，则必须使用 [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) 方法。  
  
### <a name="properties"></a>属性  
 对某些对象的 **Properties** 集合使用 **Refresh** 方法将使用提供程序公开的动态属性来填充集合。 这些属性提供了有关特定于提供程序的功能的信息，而不是 ADO 支持的内置属性。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [轴集合](../ado-md-api/axes-collection-ado-md.md)  
        [列集合](../adox-api/columns-collection-adox.md)  
        [CubeDefs 集合](../ado-md-api/cubedefs-collection-ado-md.md)  
        [维度集合](../ado-md-api/dimensions-collection-ado-md.md)  
        [错误集合](./errors-collection-ado.md)  
        [字段集合](./fields-collection-ado.md)  
        [组集合](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [层次结构集合](../ado-md-api/hierarchies-collection-ado-md.md)  
        [索引集合](../adox-api/indexes-collection-adox.md)  
        [键集合](../adox-api/keys-collection-adox.md)  
        [色阶集合](../ado-md-api/levels-collection-ado-md.md)  
        [成员集合](../ado-md-api/members-collection-ado-md.md)  
        [Parameters 集合](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [位置集合](../ado-md-api/positions-collection-ado-md.md)  
        [过程集合](../adox-api/procedures-collection-adox.md)  
        [Properties 集合](./properties-collection-ado.md)  
        [表集合](../adox-api/tables-collection-adox.md)  
        [用户集合](../adox-api/users-collection-adox.md)  
        [视图集合](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [ (VB) Refresh 方法示例 ](./refresh-method-example-vb.md)   
 [ (VC + +) 的 Refresh 方法示例 ](./refresh-method-example-vc.md)   
 [ADO)  (计数属性 ](./count-property-ado.md)   
 [Refresh 方法 (RDS)](../rds-api/refresh-method-rds.md)