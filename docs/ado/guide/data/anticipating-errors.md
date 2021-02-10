---
description: 预测错误
title: 预测错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- anticipating errors [ADO]
- errors [ADO], preventing
- preventing errors [ADO]
ms.assetid: ea1d4a97-58c3-476b-a496-cc80db2a90d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a8c7c3867d7e133428ff8e17cca8e54e5a54297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037807"
---
# <a name="anticipating-errors"></a>预测错误
错误防护至少与错误处理一样重要。 最后一节包含应用程序可用于帮助减少错误发生次数的简短注意事项列表。  
  
 在尝试使用这些对象执行操作之前，请检查 **状态** 属性中的值，以检查对象的状态。 例如，如果应用程序使用全局 **连接**，请在调用 **open** 方法之前检查其 **状态** 属性以查看它是否已打开。  
  
-   任何接受来自用户的数据的程序都必须包含在将数据发送到数据存储之前验证这些数据的代码。 你不能依赖于数据存储、提供程序、ADO 甚至你的编程语言来向你通知问题。 必须检查用户输入的每个字节，确保数据对于其字段是正确的类型，并且必填字段不为空。  
  
 在尝试将任何数据写入数据存储之前，请检查数据。 要执行此操作，最简单的方法是处理 **WillMove** 事件或 **WillUpdateRecordset** 事件。 有关处理 ADO 事件的更完整的讨论，请参阅 [处理 Ado 事件](./handling-ado-events.md)。  
  
 在尝试移动记录指针之前，请确保 **recordset** 对象不在 **记录集** 的边界之外。 如果尝试在 **EOF** 为 true 时执行 **MoveNext** ，或在 **BOF** 为 true 时进行 **MovePrev** ，则会发生错误。 如果在 **EOF** 和 **BOF** 都为 True 时执行任何 **移动** 方法，则将生成错误。  
  
 如果尝试执行 **查找** 和 **查找** 空 **记录集** 等操作，也会发生错误。