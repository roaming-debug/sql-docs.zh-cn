---
description: 流对象 (ADO)
title: " (ADO) 的流对象 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream
helpviewer_keywords:
- Stream object [ADO]
ms.assetid: 0514531f-009d-4519-abc3-d727014a39f1
author: rothja
ms.author: jroth
ms.openlocfilehash: b9baaa7e57ca10f1dc0e32d7fc5f477ee4f08100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020366"
---
# <a name="stream-object-ado"></a>流对象 (ADO)
表示二进制数据或文本的流。  
  
 在树状层次结构（例如文件系统或电子邮件系统）中， [记录](./record-object-ado.md) 可能具有与之关联的默认二进制位流，其中包含文件或电子邮件的内容。 **流** 对象可用于操作包含这些数据流的字段或记录。 可以通过以下方式获取 **流** 对象：  
  
-   从指向对象的 URL (通常是包含二进制或文本数据的文件) 。 此对象可以是一个简单的文档，也可以是一个表示结构化文档的 **记录** 对象，也可以是一个文件夹。  
  
-   打开与 **Record** 对象关联的默认 **流** 对象。 您可以在打开 **记录** 时获取与 **record** 对象关联的默认流，以消除仅用于打开流的往返过程。  
  
-   通过实例化一个 **流** 对象。 这些 **流** 对象可用于存储用于应用程序的数据。 不同于与 URL 关联的 **流** 或 **记录** 的默认 **流**，默认情况下，实例化的 **流** 不与基础源关联。  
  
 使用 **流** 对象的方法和属性，可以执行以下操作：  
  
-   使用 [open](./open-method-ado-stream.md)方法打开 **记录** 或 URL 中的 **流** 对象。  
  
-   使用 [close](./close-method-ado.md)方法关闭 **流**。  
  
-   使用 [Write](./write-method.md)和 [WriteText](./writetext-method.md)方法将字节或文本输入到 **流** 中。  
  
-   用 [read](./read-method.md)和 [ReadText](./readtext-method.md)方法从 **流** 中读取字节。  
  
-   用 [Flush](./flush-method-ado.md)方法将仍在 ADO 缓冲区中的任何 **流** 数据写入基础对象。  
  
-   使用 [CopyTo](./copyto-method-ado.md)方法将 **流** 的内容复制到另一 **流**。  
  
-   通过 [SkipLine](./skipline-method.md)方法和 [LineSeparator](./lineseparator-property-ado.md) 属性控制如何从源文件读取行。  
  
-   用 [EOS](./eos-property.md)属性和 [SetEOS](./seteos-method.md) 方法确定流位置的结尾。  
  
-   用 [SaveToFile](./savetofile-method.md)和 [LoadFromFile](./loadfromfile-method-ado.md) 方法保存和还原文件中的数据。  
  
-   指定用于存储包含 [字符集](./charset-property-ado.md)属性的 **流** 的字符集。  
  
-   使用 [Cancel](./cancel-method-ado.md)方法暂停异步 **流** 操作。  
  
-   确定具有 [Size](./size-property-ado-stream.md)属性的 **流** 中的字节数。  
  
-   使用 [position](./position-property-ado.md)属性控制 **流** 中的当前位置。  
  
-   使用 [type](./type-property-ado-stream.md)属性确定 **流** 中的数据类型。  
  
-   确定 (关闭、打开或执行 [状态](./state-property-ado.md)属性) **流** 的当前状态。  
  
-   使用 [mode](./mode-property-ado.md)属性指定 **流** 的访问模式。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
 **流式** 处理对象是安全的。  
  
 本部分包含以下主题。  
  
-   [流对象属性、方法和事件](./stream-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [记录和流](../../guide/data/records-and-streams.md)