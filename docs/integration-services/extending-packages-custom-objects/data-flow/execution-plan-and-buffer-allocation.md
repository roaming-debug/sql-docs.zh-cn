---
description: 执行计划和缓冲区分配
title: 执行计划和缓冲区分配 | Microsoft Docs
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
- custom data flow components [Integration Services], buffer allocations
- custom data flow components [Integration Services], execution plans
- buffer allocations [Integration Services]
- data flow components [Integration Services], buffer allocations
- data flow components [Integration Services], execution plans
- execution plans [Integration Services]
ms.assetid: 679d9ff0-641e-47c3-abb8-d1a7dcb279dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b9827f4943e21b117353af4aaa4ea825cef19d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343691"
---
# <a name="execution-plan-and-buffer-allocation"></a>执行计划和缓冲区分配

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在执行之前，数据流任务会先检查其组件，并为每一个组件序列生成一个执行计划。 本节提供有关执行计划、如何查看执行计划以及如何基于执行计划分配输入和输出缓冲区的详细信息。  
  
## <a name="understanding-the-execution-plan"></a>了解执行计划  
 执行计划包含源线程和工作线程，每个线程均包含指定源线程的输出工作列表或指定工作线程的输入和输出工作列表的工作列表。 执行计划中的源线程表示数据流中的源组件，并在执行计划中由 SourceThreadn 标识，其中 n 为从零开始的源线程编号 。  
  
 每个源线程都会创建一个缓冲区，设置一个侦听器，并对源组件调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法。 执行从源线程开始，数据也从这里开始出现，因为源组件开始向数据流任务为其提供的输出缓冲区中添加行。 源线程开始运行之后，会在工作线程之间平衡分配工作量。  
  
 工作线程可能同时包含输入工作列表和输出工作列表，并在执行计划中被标识为 WorkThreadn，其中 n 为从零开始的工作线程编号 。 当图形中包含具有异步输出的组件时，这些线程将包含输出工作列表。  
  
 下面的示例执行计划表示了一个数据流，该数据流包含一个源组件、一个具有异步输出的转换和一个目标组件，它们依次连接。 在此示例中，WorkThread0 包含一个输出工作列表，因为转换组件有一个异步输出。  
  
```  
SourceThread0   
    Influences: 72 158   
    Output Work List   
        CreatePrimeBuffer of type 1 for output id 10   
        SetBufferListener: "WorkThread0" for input ID 73   
        CallPrimeOutput on component "OLE DB Source" (1)   
    End Output Work List   
    This thread drives 0 distributors   
End SourceThread0   
WorkThread0   
    Influences: 72 158   
    Input Work list, input ID 73   
        CallProcessInput on input ID 73 on component "Sort" (72) for view type 2   
    End Input Work list for input 73   
    Output Work List   
        CreatePrimeBuffer of type 3 for output id 74   
        SetBufferListener: "WorkThread1" for input ID 171with internal handoff   
        CallPrimeOutput on component "Sort" (72)   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread0   
WorkThread1   
    Influences: 158   
    Input Work list, input ID 171  
        CallProcessInput on input ID 171 on component "OLE DB Destination" (158) for view type 4  
    End Input Work list for input 171   
    Output Work List   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread1  
```  
  
> [!NOTE]  
>  每次执行包时，都会生成一个执行计划，可通过向包中添加日志提供程序、启用日志记录并选择 PipelineExecutionPlan 事件来捕获该执行计划。  
  
## <a name="understanding-buffer-allocation"></a>了解缓冲区分配  
 工作流任务会基于执行计划创建缓冲区，这些缓冲区包含在数据流组件的输出中定义的列。 在数据流通过这一系列组件的过程中，会重用缓冲区，直至遇到具有异步输出的组件为止。 此时将创建新的缓冲区，其中包含异步输出的输出列和下游组件的输出列。  
  
 在执行期间，组件可以访问当前源线程或工作线程中的缓冲区。 缓冲区可以是 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法提供的输入缓冲区，也可以是 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法提供的输出缓冲区。 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.Mode%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 属性还将每个缓冲区标识为输入缓冲区或输出缓冲区。  
  
 具有异步输出的转换组件从 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法接收现有输入缓冲区，并从 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法接收新的输出缓冲区。 具有异步输出的转换组件是唯一一种可同时接收输入缓冲区和输出缓冲区的数据流组件。  
  
 由于提供给组件的缓冲区所包含的列数可能大于组件的输入或输出列集合中的列数，因此，组件开发人员可通过调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> 方法并指定列的 LineageID，在缓冲区中查找列。  
  
