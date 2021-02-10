---
description: CommandStream 属性 (ADO)
title: " (ADO) 的 CommandStream 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::CommandStream
helpviewer_keywords:
- CommandStream property [ADO]
ms.assetid: f78f61b6-87e0-48dc-961e-83d0e20da58e
author: rothja
ms.author: jroth
ms.openlocfilehash: b2b7618cae464f8d4fdbafb0ebcab09551749b90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034827"
---
# <a name="commandstream-property-ado"></a>CommandStream 属性 (ADO)
指示用作 [命令](./command-object-ado.md) 对象的输入的流。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回用作 **命令** 对象输入的流。 此流的格式特定于提供程序;有关详细信息，请参阅提供程序的文档。 此属性类似于 [CommandText](./commandtext-property-ado.md) 属性，该属性用于指定 **命令** 输入的字符串。  
  
## <a name="remarks"></a>备注  
 **CommandStream** 和 **CommandText** 互相排斥。 当用户设置 **CommandStream** 属性时， **CommandText** 属性将设置为空字符串 ( "" ) 。 如果用户设置 **CommandText** 属性，则 **CommandStream** 属性将设置为 **Nothing**。  
  
 命令的行为。 **参数** 和 **命令。 Prepare** 方法由提供程序定义。 流中的参数值无法刷新。  
  
 输入流不能用于返回 **命令** 源的其他 ADO 对象。 例如，如果将 [记录集](./recordset-object-ado.md)的 [源](./source-property-ado-recordset.md)设置为将流作为其输入的 **命令** 对象，则 **记录集。源** 继续返回 **CommandText** 属性，该属性包含一个空字符串 ( "" ) ，而不是 **CommandStream** 属性的流内容。  
  
 使用 **CommandStream**) 指定 (的命令流时， [CommandType](./commandtype-property-ado.md)属性的唯一有效 [CommandTypeEnum](./commandtypeenum.md)值为 **adCmdText** 和 **adCmdUnknown**。 其他任何值会导致错误。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO) 的 CommandText 属性 ](./commandtext-property-ado.md)   
 [方言属性](./dialect-property.md)   
 [CommandTypeEnum](./commandtypeenum.md)