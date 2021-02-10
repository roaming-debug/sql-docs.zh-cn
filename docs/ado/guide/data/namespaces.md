---
description: 命名空间
title: 命名空间 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- namespaces in ADO
ms.assetid: efff5569-db52-451d-a039-2e74870534da
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a7657e9758ebaa3507a35aef65d6a077e4dcf35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032669"
---
# <a name="namespaces"></a>命名空间
ADO 中的 XML 持久性格式使用以下四个命名空间。  
  
## <a name="remarks"></a>备注  
 ADO 中的 XML 持久性格式使用以下四个命名空间。  
  
|前缀|说明|  
|------------|-----------------|  
|s|引用 "XML 数据" 命名空间，其中包含定义当前记录集的架构的元素和属性。|  
|dt|引用数据类型定义规范。|  
|rs|引用包含特定于 ADO 记录集属性和属性的元素和属性的命名空间。|  
|z|引用当前行集的架构。|  
  
 客户端不应根据规范的定义将其自己的标记添加到这些命名空间中。 例如，客户端不应将命名空间定义为 "urn：架构-microsoft com：行集"，然后写出诸如 "rs： MyOwnTag" 之类的内容。 若要了解有关命名空间的详细信息，请参阅 " [XML 中的 W3C 命名空间" 建议](http://www.w3.org/TR/REC-xml-names/)。  
  
> [!IMPORTANT]
>  架构标记的 ID 必须是 "RowsetSchema"，并且用于引用当前行集的架构的命名空间必须指向 "#RowsetSchema"。  
  
 请注意，命名空间的前缀-冒号和等号之间的部分是任意的。  
  
```  
xmlns:rs="urn:schemas-microsoft-com:rowset"  
```  
  
 用户可以将此名称定义为任何名称，只要该名称在整个 XML 文档中始终使用。 ADO 始终写出 "s"、"rs"、"dt" 和 "z"，但这些前缀名称未硬编码为加载组件。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)