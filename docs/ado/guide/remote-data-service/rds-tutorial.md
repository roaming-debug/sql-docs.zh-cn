---
description: RDS 教程
title: RDS 教程 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO]
ms.assetid: 6e3305a0-7bc7-40d1-9122-235c15d23ab2
author: rothja
ms.author: jroth
ms.openlocfilehash: b2b9294b3024a3d4a7dbfd51baadb2474b7715ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036407"
---
# <a name="rds-tutorial"></a>RDS 教程
本教程说明如何使用 RDS 编程模型来查询和更新数据源。 首先，它描述了完成此任务所需的步骤。 然后，Microsoft®中将重复教程，Visual Basic Scripting Edition (采用 ADO for Windows Foundation 类 (ADO/WFC) # A3。  
  
 出于以下两个原因，本教程以不同的语言进行编码：  
  
-   RDS 的文档假定 Visual Basic 中的读取器代码。 这使文档对 Visual Basic 程序员非常方便，但不太适合使用其他语言的编程人员。  
  
-   如果您不确定特定的 RDS 功能，并且您知道另一种语言，则可以通过查找以其他语言表示的同一功能来解决您的问题。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="how-the-tutorial-is-presented"></a>如何显示教程  
 本教程基于 RDS 编程模型。 它单独讨论编程模型的每个步骤。 此外，它还说明了包含 Visual Basic 代码片段的每个步骤。  
  
 此代码示例以其他语言重复，并提供最少的讨论。 给定编程语言教程中的每个步骤都使用编程模型和描述性教程中的相应步骤进行标记。 在描述性教程中，使用步骤号来引用讨论。  
  
 以下部分说明了 RDS 编程模型。 在学习本教程时，请将其用作路线图。  
  
## <a name="rds-programming-model-with-objects"></a>RDS 编程模型和对象  
  
-   指定要在服务器上调用的程序，并获取 (proxy) 从客户端引用该程序的方法。  
  
-   调用服务器程序。 将参数传递给用于标识数据源和要发出的命令的服务器程序。  
  
-   服务器程序从数据源中获取 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象，通常使用 ADO。 或者，在服务器上处理 **Recordset** 对象。  
  
-   服务器程序将最终的 **记录集** 对象返回到客户端应用程序。  
  
-   在客户端上，可以选择将 **记录集** 对象置于可由视觉对象轻松使用的窗体中。  
  
-   对 **Recordset** 对象所做的更改将发送回服务器，并用于更新数据源。  
  
 本教程包含以下主题。  
  
-   [步骤 1：指定服务器程序（RDS 教程）](./step-1-specify-a-server-program-rds-tutorial.md)  
  
-   [步骤 2：调用服务器程序（RDS 教程）](./step-2-invoke-the-server-program-rds-tutorial.md)  
  
-   [步骤 3：服务器获取记录集（RDS 教程）](./step-3-server-obtains-a-recordset-rds-tutorial.md)  
  
-   [步骤 4：服务器返回记录集（RDS 教程）](./step-4-server-returns-the-recordset-rds-tutorial.md)  
  
-   [步骤 5：DataControl 变为可用（RDS 教程）](./step-5-datacontrol-is-made-usable-rds-tutorial.md)  
  
-   [步骤 6：将更改发送到服务器（RDS 教程）](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)  
  
-   [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)  
  
## <a name="see-also"></a>另请参阅  
 [步骤1：指定服务器程序 (RDS 教程) ](./step-1-specify-a-server-program-rds-tutorial.md)   
 [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)