---
description: 在 Visual C++ 应用程序中引用 ADO 库
title: 在 Visual C++ 应用程序中引用 ADO 库 |Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual C++ application[ADO]
- ADO, libraries
ms.assetid: d3ea12ec-bca8-48c3-af57-ce14576108c9
author: rothja
ms.author: jroth
ms.openlocfilehash: 596e11a51a093be30fab7d9d3b273d6605b48b75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032069"
---
# <a name="referencing-the-ado-libraries-in-a-visual-c-application"></a>在 Visual C++ 应用程序中引用 ADO 库
若要在 Visual C++ 应用程序中使用最新版本的 ADO，请使用以下 `#import` 指令：  
  
```cpp
#import "msado15.dll" \  
    no_namespace rename("EOF", "EndOfFile")  
```  
  
 若要使用 ADO MD 或 ADOX，必须使用上述语法导入 *msadomd.dll* 或 *msadox.dll*。  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 若要使用 ADO 的任何早期版本，请将上述 *msado15.dll* 替换为以下类型库之一。  
  
-   *msado27*，ADO 2.7 类型库  
  
-   *msado26*，ADO 2.6 类型库  
  
-   *msado25*，ADO 2.5 类型库  
  
-   *msado21*，ADO 2.1 类型库  
  
-   *msado20*，ADO 2.0 类型库
