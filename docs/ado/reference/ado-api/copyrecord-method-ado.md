---
description: CopyRecord 方法 (ADO)
title: " (ADO) 的 CopyRecord 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_CopyRecord
- _Record::CopyRecord
helpviewer_keywords:
- CopyRecord method [ADO]
ms.assetid: b9bcf272-3c74-479f-95dd-0229a32e98fc
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f5a6e6f7d607bbc2f50bfad701056aca93fb822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026035"
---
# <a name="copyrecord-method-ado"></a>CopyRecord 方法 (ADO)
将 [记录](./record-object-ado.md) 表示的实体复制到其他位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
Record.CopyRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 一个 **字符串** 值，其中包含指定要复制的实体 (例如，) 文件或目录。 如果省略了 *Source* 或指定了空字符串，则将复制当前 [记录](./record-object-ado.md) 所表示的文件或目录。  
  
 *目标*  
 可选。 一个 **字符串** 值，该值包含指定将复制 *源* 的位置的 URL。  
  
 *UserName*  
 可选。 一个 **字符串** 值，该值包含用户 ID，该 ID 在需要时授予访问 *目标* 的权限。  
  
 *密码*  
 可选。 一个包含密码的 **字符串** 值，如果需要，将验证 *用户名*。  
  
 *选项*  
 可选。 默认值为 **adCopyUnspecified** 的 [CopyRecordOptionsEnum](./copyrecordoptionsenum.md)值。 指定此方法的行为。  
  
 *异步*  
 可选。 一个 **布尔** 值，如果 **为 True，则** 将此操作指定为异步。  
  
## <a name="return-value"></a>返回值  
 通常返回 *Destination* 值的 **字符串** 值。 但是，返回的确切值与提供程序相关。  
  
## <a name="remarks"></a>备注  
 *源* 和 *目标* 的值不得完全相同;否则，会发生运行时错误。 至少一个服务器、路径或资源名称必须不同。  
  
 所有子级 (例如 *，将以* 递归方式复制子目录) ，除非指定了 **adCopyNonRecursive** 。 在递归操作中， *目标* 不能是 *源* 的子目录;否则，操作将不会完成。  
  
 如果 *Destination* 标识现有实体 (例如，) 文件或目录，则此方法将失败，除非指定了 **adCopyOverWrite** 。  
  
> [!IMPORTANT]
>  谨慎使用 **adCopyOverWrite** 选项。 例如，如果在将文件复制到目录时指定此选项，则会 *删除* 该目录并将其替换为文件。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](./record-object-ado.md)