---
description: DeleteRecord 方法 (ADO)
title: " (ADO) 的 DeleteRecord 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_DeleteRecord
- _Record::DeleteRecord
helpviewer_keywords:
- DeleteRecord method [ADO]
ms.assetid: 2726498c-dbd8-4266-983b-ae7d62c39142
author: rothja
ms.author: jroth
ms.openlocfilehash: cb6439e59a34beb9b4e8b505f4244c48c173b552
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025479"
---
# <a name="deleterecord-method-ado"></a>DeleteRecord 方法 (ADO)
删除 [记录](../../../ado/reference/ado-api/record-object-ado.md)表示的实体。  
  
## <a name="syntax"></a>语法  
  
```  
  
Record.DeleteRecord Source, Async  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 一个 **字符串** 值，其中包含标识实体的 URL (例如，要删除的文件或目录) 。 如果省略了 *Source* 或指定了空字符串，则将删除当前 [记录](../../../ado/reference/ado-api/record-object-ado.md) 表示的实体。 如果记录是 ([RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md) 的 **adCollectionRecord** 的集合记录，例如目录) 所有子项 (例如，子目录) 也将被删除。  
  
 *异步*  
 可选。 一个 **布尔** 值，如果 **为 True**，则指定删除操作为异步。  
  
## <a name="remarks"></a>备注  
 此方法完成后，对此 **记录** 所表示的对象执行的操作可能会失败。 在调用 **DeleteRecord** 之后，应关闭 **记录** ，因为 **记录** 的行为可能会因提供程序用数据源更新 **记录** 的时间而变得不可预知。  
  
 如果此 **记录** 是从 [记录集中](../../../ado/reference/ado-api/recordset-object-ado.md)获取的，则此操作的结果不会立即反映在 **记录集中**。 刷新记录集，方法是关闭然后重新打开该 **记录集** ，或者 **执行 recordset** [Requery](../../../ado/reference/ado-api/requery-method.md) 方法、 [Update](../../../ado/reference/ado-api/update-method.md) 方法或重新 [同步](../../../ado/reference/ado-api/resync-method.md) 方法。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../../ado/guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](../../../ado/reference/ado-api/record-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 字段集合的 Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [ (ADO 参数集合的 Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Delete 方法（ADO 记录集）](../../../ado/reference/ado-api/delete-method-ado-recordset.md)
