---
description: MoveRecord 方法 (ADO)
title: " (ADO) 的 MoveRecord 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::MoveRecord
- _Record::raw_MoveRecord
helpviewer_keywords:
- MoveRecord method [ADO]
ms.assetid: 6d2807b0-b861-4583-bcaf-fb0b82e0f2d0
author: rothja
ms.author: jroth
ms.openlocfilehash: a92b7bc00b2deed4047dca5ed8bcf67fcd14ced9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041637"
---
# <a name="moverecord-method-ado"></a>MoveRecord 方法 (ADO)
将 [记录](./record-object-ado.md) 表示的实体移到另一个位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
Record.MoveRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 一个 **字符串** 值，该值包含用于标识要移动的 **记录** 的 URL。 如果省略了 *Source* 或指定了空字符串，则移动此 **记录** 表示的对象。 例如，如果 **记录** 表示文件，则该文件的内容将移动到 *Destination* 指定的位置。  
  
 *目标*  
 可选。 一个 **字符串** 值，该值包含指定将移动 *源* 的位置的 URL。  
  
 *UserName*  
 可选。 一个 **字符串** 值，该值包含用户 ID，该 ID 在需要时授予访问 *目标* 的权限。  
  
 *密码*  
 可选。 一个包含密码的 **字符串** ，如有必要，将验证 *用户名*。  
  
 *选项*  
 可选。 一个 [MoveRecordOptionsEnum](./moverecordoptionsenum.md) 值，其默认值为 **adMoveUnspecified**。 指定此方法的行为。  
  
 *异步*  
 可选。 一个 **布尔** 值，如果 **为 True，则** 将此操作指定为异步。  
  
## <a name="return-value"></a>返回值  
 一个字符串值。 通常，返回 *目标* 的值。 但是，返回的确切值与提供程序相关。  
  
## <a name="remarks"></a>备注  
 *源* 和 *目标* 的值不得完全相同;否则，会发生运行时错误。 至少服务器、路径和资源名称必须不同。  
  
 对于使用 Internet 发布提供程序移动的文件，此方法将更新要移动的文件中的所有超文本链接，除非由 *选项* 指定。 如果 *Destination* 标识现有对象 (例如，) 文件或目录，则此方法将失败，除非指定了 **adMoveOverWrite** 。  
  
> [!NOTE]
>  谨慎使用 **adMoveOverWrite** 选项。 例如，在将文件移动到目录时指定此选项将删除该目录并将其替换为文件。  
  
 此操作完成后，将不会更新 **记录** 对象的某些属性（例如 [ParentURL](./parenturl-property-ado.md) 属性）。 通过关闭记录来刷新 **记录** 对象的属性，然后使用移动文件或目录的位置的 URL 重新打开该 **记录**。  
  
 如果此 **记录** 是从 [记录集中](./recordset-object-ado.md)获取的，则移动的文件或目录的新位置不会立即反映在 **记录集中**。 请关闭并重新打开 **记录集** 。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO (移动方法) ](./move-method-ado.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) ](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)