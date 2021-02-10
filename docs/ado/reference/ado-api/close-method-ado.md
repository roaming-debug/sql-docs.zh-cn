---
description: Close 方法 (ADO)
title: ADO)  (关闭方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Close
- _Stream::Close
- _Record::Close
helpviewer_keywords:
- Close method [ADO]
ms.assetid: 3cdf27d1-a180-4cff-8e42-95dec5fb1b55
author: rothja
ms.author: jroth
ms.openlocfilehash: 0e7f1edd109f9ea4fe66c3f77a732aeec27e41c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027075"
---
# <a name="close-method-ado"></a>Close 方法 (ADO)
关闭打开的对象和所有依赖对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.Close  
```  
  
## <a name="remarks"></a>备注  
 使用 **close** 方法可关闭 [连接](./connection-object-ado.md)、 [记录](./record-object-ado.md)、 [记录集](./recordset-object-ado.md)或 [流](./stream-object-ado.md) 对象，以释放任何关联的系统资源。 关闭对象并不会将其从内存中删除;您可以更改其属性设置，稍后再次打开它。 若要从内存中完全消除对象，请关闭对象，然后在 Visual Basic) 中将对象变量设置为 *Nothing* (。  
  
## <a name="connection"></a>连接  
 使用 **close** 方法关闭 **连接** 对象也会关闭与该连接关联的任何活动 **记录集** 对象。 与正在关闭的 **连接** 对象关联的 [命令](./command-object-ado.md)对象将会保持不变，但它将不再与 **连接** 对象关联;也就是说，其 [ActiveConnection](./activeconnection-property-ado.md)属性将设置为 **Nothing**。 此外，将清除 **命令** 对象的 [参数](./parameters-collection-ado.md) 集合中的任何提供程序定义的参数。  
  
 稍后可以调用 [Open](./open-method-ado-connection.md) 方法来重新建立与相同或其他数据源的连接。 **连接** 对象关闭时，调用任何需要打开到数据源的连接的方法都会生成错误。  
  
 如果在连接上存在打开的 **记录集** 对象时关闭 **连接** 对象，则会回滚所有 **记录集** 对象中的所有挂起的更改。 在事务正在进行时， (调用 **Close** 方法) 显式关闭 **连接** 对象会生成错误。 如果某个 **连接** 对象在事务正在进行时超出范围，则 ADO 会自动回滚该事务。  
  
## <a name="recordset-record-stream"></a>记录集，记录，流  
 使用 **close** 方法关闭 **Recordset**、 **Record** 或 **Stream** 对象时，将释放关联的数据和您通过此特定对象对数据所拥有的任何独占访问权限。 稍后可以调用 [Open](./open-method-ado-recordset.md) 方法，以打开具有相同的或已修改的特性的对象。  
  
 当 **记录集** 对象关闭时，调用任何需要动态游标的方法将生成错误。  
  
 如果在即时更新模式下编辑正在进行，则调用 **Close** 方法会生成错误;请先调用 [Update](./update-method.md) 或 [CancelUpdate](./cancelupdate-method-ado.md) 方法。 如果在批处理更新模式下关闭 **Recordset** 对象，则自上次调用 [UpdateBatch](./updatebatch-method.md) 后发生的所有更改都会丢失。  
  
 如果使用 [Clone](./clone-method-ado.md) 方法创建打开的 **Recordset** 对象的副本，则关闭原始或克隆不会影响任何其他副本。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [连接对象 (ADO)](./connection-object-ado.md)  
        [记录对象 (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
        [流对象 (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [ (VB) 的打开和关闭方法示例 ](./open-and-close-methods-example-vb.md)   
 [ (VBScript) 的打开和关闭方法示例 ](./open-and-close-methods-example-vbscript.md)   
 [打开和关闭方法示例 (VC + +) ](./open-and-close-methods-example-vc.md)   
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [ADO 记录集 (打开方法) ](./open-method-ado-recordset.md)   
 [Save 方法](./save-method.md)