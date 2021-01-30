---
description: SaveToFile 方法
title: SaveToFile 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SaveToFile
- _Stream::SaveToFile
helpviewer_keywords:
- SaveToFile method [ADO]
ms.assetid: 8a8594f2-422b-4d2e-94f8-7fe337445900
author: rothja
ms.author: jroth
ms.openlocfilehash: e52fe6b3b26a8bc4832952940c7e84c736f2da5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166587"
---
# <a name="savetofile-method"></a>SaveToFile 方法
将 [流](./stream-object-ado.md) 的二进制内容保存到文件。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.SaveToFile FileName, SaveOptions  
```  
  
#### <a name="parameters"></a>参数  
 *FileName*  
 一个 **字符串** 值，该值包含 **流** 的内容将保存到的文件的完全限定名称。 可以保存到任何有效的本地位置，也可以通过 UNC 值进行访问。  
  
 *System.xml.linq.saveoptions>*  
 一个 [SaveOptionsEnum](./saveoptionsenum.md) 值，该值指定是否应由 **SaveToFile** 创建新文件（如果它尚不存在）。 默认值为 **adSaveCreateNotExists**。 使用这些选项，可以指定在指定的文件不存在时出现错误。 还可以指定 **SaveToFile** 覆盖现有文件的当前内容。  
  
> [!NOTE]
>  如果在将 **adSaveCreateOverwrite** 设置为) 时覆盖现有文件 (，则 **SaveToFile** 将从新的 [EOS](./eos-property.md)之后的原始现有文件中截断任何字节。  
  
## <a name="remarks"></a>备注  
 **SaveToFile** 可用于将 **Stream** 对象的内容复制到本地文件。 **Stream** 对象的内容或属性没有变化。 在调用 **SaveToFile** 之前，必须打开 **流** 对象。  
  
 此方法不会更改 **流** 对象与基础源的关联。 **流** 对象仍将与打开时源的原始 URL 或 **记录** 相关联。  
  
 **SaveToFile** 操作完成后，流中 ([位置](./position-property-ado.md)) 的当前位置将设置为流 (0) 的开头。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO 流 (打开方法) ](./open-method-ado-stream.md)   
 [Save 方法](./save-method.md)