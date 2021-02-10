---
description: ReadText 方法
title: ReadText 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_ReadText
- _Stream::ReadText
helpviewer_keywords:
- ReadText method [ADO]
ms.assetid: be5a409e-cf87-4859-9ea5-713401755a77
author: rothja
ms.author: jroth
ms.openlocfilehash: b9d82bf7985afebbcbd0a07baeae76cb80af6027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040767"
---
# <a name="readtext-method"></a>ReadText 方法
从文本 [流](./stream-object-ado.md) 对象中读取指定数目的字符。  
  
## <a name="syntax"></a>语法  
  
```  
  
String = Stream.ReadText ( NumChars)  
```  
  
#### <a name="parameters"></a>参数  
 *NumChars*  
 可选。 一个 **长整型** 值，指定要从文件中读取的字符数或 [StreamReadEnum](./streamreadenum.md) 值。 默认值为 **adReadAll**。  
  
## <a name="return-value"></a>返回值  
 **ReadText** 方法从 **流** 对象读取指定数目的字符、整行或整个流，并返回生成的字符串。  
  
## <a name="remarks"></a>备注  
 如果 *NumChar* 大于流中剩余的字符数，则仅返回剩余字符数。 不填充字符串 read 来匹配 *NumChar* 指定的长度。 如果没有要读取的字符，则返回值为 null 的变量。 **ReadText** 不能用于向后读取。  
  
> [!NOTE]
>  **ReadText** 方法用于文本流 ([类型](./type-property-ado-stream.md)为 **adTypeText**) 。 对于二进制流 (**类型** 为 **adTypeBinary**) ，请使用 [Read](./read-method.md)。  
  
 如果查询导致大量的 XML 数据通过 ActiveX 数据对象的 **ReadText** 方法返回 (ADO) 流对象，可能需要很长时间才能执行;如果这是在从 ASP 页调用的 COM + 组件中完成的，则用户的会话可能会超时。ADO 将流对象数据从 UTF-8 编码转换为 Unicode;经常在转换此类大量数据时所涉及的频繁内存重新分配非常耗时。 若要解决此问题，请重复调用 ADO 命令对象的 **ReadText** 方法，并指定较少的字符数。 测试显示，与 128K (131072) 等效的值是最佳的。 由于此值降低，响应时间会减少。 有关详细信息，请参阅 Microsoft 知识库中的知识库文章 280067 "PRB：使用 ADO stream 对象的 ReadText 方法从 SQL Server 2000 检索非常大的 XML 文档可能会很慢" https://support.microsoft.com 。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Read 方法](./read-method.md)