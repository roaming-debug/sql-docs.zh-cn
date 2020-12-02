---
description: 以编程方式添加数据流任务
title: 以编程方式添加数据流任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- adding Data Flow task
- SSIS data flow
- data flow task [Integration Services], adding
- MainPipe object
ms.assetid: 0ca03712-a82e-4aa7-949b-f869a8936ddf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c5090cbe224a9ac3f94bff828189d3517c1dd376
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130699"
---
# <a name="adding-the-data-flow-task-programmatically"></a>以编程方式添加数据流任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 包含了一个称为数据流任务的任务，由对象模型中的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper> 命名空间表示。 数据流任务是一个专门用于在包执行期间转换和移动数据的专用高性能任务。 与其他任务相似，数据流任务由 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 对象包装，从运行时引擎的角度出发，此任务只是该包中的另一个任务。 但是，数据流包含称为数据流组件的其他对象。 这些组件将数据从源移到目标，有时候还会经过转换。 这些组件定义数据的移动方向和转换数据的方式。 配置数据流任务涉及向任务添加组件，然后连接这些组件以便建立数据流并完成所需的转换。  
  
 数据流中有三种类型的组件：“数据流源”、“数据流转换”和“数据流目标”，它们以该顺序显示在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的工具箱中。 这些类型又简称为源、转换和目标。 顾名思义，数据流从源到转换，然后到达目标。 这是简化的数据流说明，只为说明概念，但是数据流任务却很灵活很强大，足以处理多个源以及将许多向多个目标发送输出的转换连接在一起。  
  
 将数据流任务添加到包的方法与添加其他任务相同。 添加该任务后，可配置该任务，方法是向数据流任务添加组件，然后配置并连接任务中的组件。  
  
## <a name="sample"></a>示例  
 下面的代码示例演示如何向包添加数据流任务。 此示例需要引用程序集 Microsoft.SqlServer.PipelineHost、Microsoft.SqlServer.DTSPipelineWrap 和 Microsoft.SqlServer.ManagedDTS。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      Executable e = p.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;   
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim e As Executable = p.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As TaskHost = CType(e, TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>外部资源  
 blogs.msdn.com 上的博客文章 [EzAPI - 为 SQL Server 2012 更新](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042)。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式查找数据流组件](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
  
  
