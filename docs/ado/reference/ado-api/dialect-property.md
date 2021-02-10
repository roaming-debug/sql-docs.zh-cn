---
description: Dialect 属性
title: 方言属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::Dialect
helpviewer_keywords:
- Dialect property
ms.assetid: 329c3a71-ba88-4009-b04f-2f52195a5957
author: rothja
ms.author: jroth
ms.openlocfilehash: 50bca564323d2094dc5e5ceaec5b703fb0a06a23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025432"
---
# <a name="dialect-property"></a>Dialect 属性
指示 [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) 或 [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) 属性的方言。 方言定义了提供程序用于分析字符串或流的语法和一般规则。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 **方言** 属性包含表示命令文本或流的方言的有效 GUID。 此属性的默认值为 {C8B521FB-5CF3-11CE-ADE5-00AA0044773D}，指示提供程序应选择如何解释命令文本或流。  
  
## <a name="remarks"></a>备注  
 当用户读取此属性的值时，ADO 不会查询提供程序;它返回当前存储在 [Command](../../../ado/reference/ado-api/command-object-ado.md) 对象中的值的字符串表示形式。  
  
 当用户设置 **方言** 属性时，ADO 将验证 GUID，如果提供的值不是有效的 guid，则会引发错误。 请参阅提供程序的文档，以确定 **方言** 属性支持的 GUID 值。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Execute 方法（ADO 命令）](../../../ado/reference/ado-api/execute-method-ado-command.md)
