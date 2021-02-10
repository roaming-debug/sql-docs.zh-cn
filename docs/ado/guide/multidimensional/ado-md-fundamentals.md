---
description: ADO MD 基础知识
title: ADO MD 基础 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO MD, fundamentals
ms.assetid: f6a20d9f-c1ab-474c-b9f3-82277e2a126d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7634d32b35eef7d0289ef778bee9646a050e8755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032189"
---
# <a name="ado-md-fundamentals"></a>ADO MD 基础知识
Microsoft® ActiveX®数据对象 (多维)  (ADO MD) 允许从 Microsoft Visual Basic® Microsoft Visual C++®等语言轻松访问多维数据。 ADO MD 将 Microsoft ActiveX®数据对象扩展 (ADO) ，以包含特定于多维数据的对象，例如 [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) 和 [单元集](../../reference/ado-md-api/cellset-object-ado-md.md) 对象。 使用 ADO MD 可以浏览多维架构、查询多维数据集和检索结果。  
  
 与 ADO 一样，ADO MD 使用底层 OLE DB 提供程序来获取对数据的访问权限。 若要使用 ADO MD，提供程序必须是多维数据提供程序 (MDP) ，如 OLAP 规范的 OLE DB 所定义。 MDP 在多维视图而不是表格视图中呈现数据，表格视图是表格数据访问接口 (TDP) 显示数据的方式。 有关提供程序支持的特定语法和行为的详细信息，请参阅 OLAP OLE DB 提供程序的文档。  
  
 本文档假定你了解 Visual Basic 的编程语言和 ADO 和 OLAP 的一般知识。 有关详细信息，请参阅 [ADO 程序员指南](../ado-programmer-s-guide.md) 和 [OLE DB，了解联机分析处理 (OLAP) ](/previous-versions/windows/desktop/ms717005(v=vs.85))。  
  
 本部分包含以下主题。  
  
-   [多维架构和数据的概述](./overview-of-multidimensional-schemas-and-data.md)  
  
-   [使用多维数据](./working-with-multidimensional-data.md)  
  
-   [使用 ADO 与 ADO MD](./using-ado-with-ado-md.md)  
  
-   [使用 ADO MD 进行编程](./programming-with-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO MD 对象模型](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO 程序员指南](../ado-programmer-s-guide.md)   
 [数据定义语言和安全性的 ADO 扩展 (ADOX) ](../extensions/ado-extensions-for-data-definition-language-and-security-adox.md)   
 [多维架构和数据的概述](./overview-of-multidimensional-schemas-and-data.md)   
 [用 ADO MD 进行编程](./programming-with-ado-md.md)   
 [使用 ADO 与 ADO MD](./using-ado-with-ado-md.md)   
 [使用多维数据](./working-with-multidimensional-data.md)