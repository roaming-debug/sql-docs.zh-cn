---
description: 捕获 ADO 错误代码
title: ADO 错误代码 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], error codes
ms.assetid: 3aee61c7-a9b7-4596-b78e-5828a00d0281
author: rothja
ms.author: jroth
ms.openlocfilehash: 2d3b0fd0c3a673cc841d7a5318872151bc20b311
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029501"
---
# <a name="capture-ado-error-codes"></a>捕获 ADO 错误代码
除了[错误](../../reference/ado-api/errors-collection-ado.md)集合的[错误](../../reference/ado-api/error-object.md)对象中返回的提供程序错误，ADO 本身还可以将错误返回到运行时环境的异常处理机制。 使用错误捕获机制编程语言（例如 Microsoft®中的 **error** 语句 Visual Basic 或 Microsoft Visual C++®中的 **try-catch** 块）来捕获 ADO 错误。

 有关 ADO 错误代码的列表，请参阅 [ErrorValueEnum](../../reference/ado-api/errorvalueenum.md)。

## <a name="see-also"></a>另请参阅
 [](../../reference/ado-api/error-object.md) [ADO)  (错误对象错误集合](../../reference/ado-api/errors-collection-ado.md)