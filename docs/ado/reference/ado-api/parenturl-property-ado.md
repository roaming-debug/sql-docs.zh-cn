---
description: ParentURL 属性 (ADO)
title: " (ADO) 的 ParentURL 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::ParentURL
helpviewer_keywords:
- ParentURL property [ADO]
ms.assetid: 65120ce6-3900-4cd4-b322-3b9816d74737
author: rothja
ms.author: jroth
ms.openlocfilehash: fe54516f1721cdc24da9fbdeaf72688a6c0960a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041037"
---
# <a name="parenturl-property-ado"></a>ParentURL 属性 (ADO)
指示指向当前 **记录** 对象的父 [记录](./record-object-ado.md)的绝对 URL 字符串。  
  
## <a name="return-value"></a>返回值  
 返回一个 **字符串** 值，该值指示父 **记录** 的 URL。  
  
## <a name="remarks"></a>备注  
 **ParentURL** 属性取决于用于打开 **Record** 对象的源。 例如，可以使用包含 [ActiveConnection](./activeconnection-property-ado.md)属性引用的目录的相对路径名称的源打开 **记录**。  
  
 假设 "second" 是包含在 "first" 下的文件夹。 使用以下语法打开 **Record** 对象：  
  
```  
record.ActiveConnection = "https://first"  
record.Open "second"  
```  
  
 现在， `the` **ParentURL** 属性的值与 `"https://first"` **ActiveConnection** 相同。  
  
 源还可以是绝对 URL，如， `"https://first/second"` 。 然后， **ParentURL** 属性是 `"https://first"` 上述级别 `"second"` 。  
  
 如果以下情况，此属性可能为 null 值：  
  
-   当前对象没有父对象;例如，如果 **Record** 对象表示目录的根。  
  
-   **Record** 对象表示不能使用 URL 指定的实体。  
  
 此属性是只读的。  
  
> [!NOTE]
>  此属性仅受文档源提供程序支持，例如 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [记录和 Provider-Supplied 字段](../../guide/data/records-and-provider-supplied-fields.md)。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
> [!NOTE]
>  如果当前记录包含 ADO **记录集** 的数据记录，则访问 **ParentURL** 属性将导致运行时错误，指示不可能存在 URL。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](./record-object-ado.md)