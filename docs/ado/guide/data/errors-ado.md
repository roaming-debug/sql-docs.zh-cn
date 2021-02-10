---
description: 错误 (ADO)
title: ADO)  (错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 8ae6611b-3069-4155-b014-c0c9da37be39
author: rothja
ms.author: jroth
ms.openlocfilehash: b26899566d428a1b04c918c2552972448d996597
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037507"
---
# <a name="errors-ado"></a>错误 (ADO)
涉及 ADO 对象的任何操作都可能会生成一个或多个提供程序错误。 出现每个错误时，会将一个或多个 **错误** 对象放入 **连接** 对象的 **错误** 集合中。 有关在 ADO 应用程序中处理警告和错误的详细信息，请参阅 [错误处理](./error-handling.md)。  
  
 应用程序错误可以由单独的机制引发。 例如，在 Visual Basic 中， **Err** 对象将包含应用程序级别的错误。