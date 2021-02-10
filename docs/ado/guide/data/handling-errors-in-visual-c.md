---
description: 处理 Visual C++ 中的错误
title: 处理 Visual C++ 中的错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- errors [ADO], Visual C++
- Visual C++ error handling [ADO]
ms.assetid: b7576f07-020a-45f7-9e79-b5756f33f7ab
author: rothja
ms.author: jroth
ms.openlocfilehash: 61435c9d66800704e962cd4a399c87e72f6558c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037367"
---
# <a name="handling-errors-in-visual-c"></a>处理 Visual C++ 中的错误
在 COM 中，大多数操作都返回一个 HRESULT 返回代码，该代码指示函数是否已成功完成。 #Import 指令围绕每个 "原始" 方法或属性生成包装代码，并检查返回的 HRESULT。 如果 HRESULT 指示失败，则包装代码会调用 _com_issue_errorex ( # A1，并使用 HRESULT 返回代码作为参数，从而引发 COM 错误。 COM 错误对象可以在 **try-catch** 块中捕获。  (为提高效率，请捕获对 _com_error 对象的引用 )   
  
 请记住，这些是 ADO 错误：它们是由于 ADO 操作失败导致的。 基础提供程序返回的错误在 **连接** 对象的 **错误** 集合中显示为 **错误** 对象。  
  
 #Import 指令只为在 ADO 中声明的方法和属性创建错误处理例程。 但是，您可以通过编写自己的错误检查宏或内联函数来利用这一相同的错误处理机制。 有关示例，请参阅主题 Visual C++®扩展。
