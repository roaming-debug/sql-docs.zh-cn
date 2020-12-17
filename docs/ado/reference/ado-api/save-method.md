---
description: Save 方法
title: Save 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Recordset::Save
- _Recordset::raw_Save
helpviewer_keywords:
- Save method [ADO]
ms.assetid: ed3d9678-5c28-4e61-8bb3-7dfb66d99cf5
author: rothja
ms.author: jroth
ms.openlocfilehash: a32419db6e4dd04cc57b31b1d9267e80a30db41d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638127"
---
# <a name="save-method"></a>Save 方法
将 [记录集](./recordset-object-ado.md) 保存到文件或 [流](./stream-object-ado.md) 对象中。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Save Destination, PersistFormat  
```  
  
#### <a name="parameters"></a>参数  
 *目标*  
 可选。 一个 **变量** ，表示要保存 **记录集** 的文件的完整路径名称，或者是对 **流** 对象的引用。  
  
 *PersistFormat*  
 可选。 一个 [PersistFormatEnum](./persistformatenum.md) 值，指定将 **记录集** 保存 (XML 或 ADTG) 的格式。 默认值为 **adPersistADTG**。  
  
## <a name="remarks"></a>备注  
 只能对打开的 **记录集** 调用 **Save 方法** 方法。 使用 [Open 方法 (ADO Recordset)](./open-method-ado-recordset.md)方法稍后从 *目标* 还原 **记录集**。  
  
 如果 [筛选器属性](./filter-property.md) 属性对 **记录集** 有效，则仅保存在筛选器下可访问的行。 如果 **记录集** 是分层的，则保存当前子 **记录集** 及其子记录集（包括父 **记录** 集）。 如果调用子记录集的 Save 方法，则会保存子记录集及其所有子级，但不会保存父 **记录集** 。  
  
 第一次保存 **记录集** 时，可以选择指定 *目标*。 如果省略 " *目标*"，则将创建一个新文件，其名称设置为 **记录集** 的 "源" 属性的值。  
  
 当你随后在第一次保存后调用 **Save** 时，忽略 *目标*，否则将发生运行时错误。 如果随后对新 *目标* 调用 **Save** ，则会将 **记录集** 保存到新目标。 但是，新目标和原始目标都将打开。  
  
 **保存** 不会关闭 **记录集** 或 *目标*，因此可以继续使用 **记录集** 并保存最近的更改。 直到 **记录集** 关闭，*目标* 保持打开状态。  
  
 出于安全原因， **Save** 方法只允许使用由 Microsoft Internet Explorer 执行的脚本中的低和自定义安全设置。  
  
 如果在异步 **记录集** 提取、执行或更新操作正在进行时调用 **save** 方法，则 **将** 等待，直到异步操作完成。  
  
 记录从 **记录集** 的第一行开始保存。 当 **Save** 方法完成后，当前行位置将移到 **记录集** 的第一行。  
  
 为获得最佳结果，请将 [CursorLocation 属性 (ADO)](./cursorlocation-property-ado.md)属性设置为 "**保存**" 的 **adUseClient** 。 如果提供程序不支持保存 **记录集** 对象所需的所有功能，则游标服务将提供该功能。  
  
 当 **记录集** 的 **CursorLocation** 属性设置为 **adUseServer** 时， **记录集** 的更新功能会受到限制。 通常，仅允许单个表更新、插入和删除 (依赖提供程序功能) 。 此配置中也不能使用 [Resync 方法](./resync-method.md) 方法。  
  
> [!NOTE]
>  ADO 不支持使用类型为 **adVariant**、 **adIDispatch** 或 **AdIUnknown** 的 **字段** 保存 **记录集**，这可能会导致不可预知的结果。  
  
 只有条件字符串形式的筛选器 (例如，) 日期的 > "12/31/1999" 影响持久 **记录集** 的内容。 使用 **书签** 数组或 [FilterGroupEnum](./filtergroupenum.md) 中的值创建的筛选器不会影响持久 **记录集** 的内容。 这些规则适用于使用客户端或服务器端游标创建的 **记录集**。  
  
 由于 *目标* 参数可以接受支持 OLE DB IStream 接口的任何对象，因此可以将 **记录集** 直接保存到 ASP 响应对象。 有关更多详细信息，请参阅 **XML 记录集持久性方案**。  
  
 你还可以将 XML 格式的 **记录集** 保存到 MSXML DOM 对象的实例，如以下 Visual Basic 代码所示：  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=Northwind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistXML   'Save Recordset directly into a DOM tree.  
...  
```  
  
> [!NOTE]
>  将分层记录集保存 () XML 格式的数据形状时，有两个限制。 如果分层 **记录集** 包含挂起的更新，则无法保存到 XML 中，并且无法保存参数化分层 **记录集**。  
  
 使用 UTF-8 格式保存以 XML 格式保存的 **记录集** 。 将此类文件加载到 ADO 流时，除非将流的字符集属性设置为适用于 UTF-8 格式的适当值，否则 Stream 对象将不会尝试从流中打开 **记录集** 。  
  
## <a name="applies-to"></a>适用于  

:::row:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [流对象 (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [ (VB 保存和打开方法示例) ](./save-and-open-methods-example-vb.md)   
 [保存并打开方法示例 (VC + +) ](./save-and-open-methods-example-vc.md)   
 [ADO 记录集 (打开方法) ](./open-method-ado-recordset.md)   
 [ADO 流 (打开方法) ](./open-method-ado-stream.md)   
 [SaveToFile 方法](./savetofile-method.md)