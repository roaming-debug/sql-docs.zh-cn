---
description: 在脚本组件中引发事件
title: 在脚本组件中引发事件 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], raising events
ms.assetid: bb389073-e1d0-4794-8d29-c8b293b6a5e3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 812a2db47243f48d13dd2adf6f5b8098046f1c40
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193067"
---
# <a name="raising-events-in-the-script-component"></a>在脚本组件中引发事件

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  事件提供向包含包报告错误、警告和其他信息（如任务进度或状态）的方式。 包为管理事件通知提供事件处理程序。 脚本组件可通过对 ScriptMain 类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> 属性调用方法来引发事件。 有关 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包如何处理事件的详细信息，请参阅 [Integration Services (SSIS) 事件处理程序](../../../integration-services/integration-services-ssis-event-handlers.md)。  
  
 事件可以记录到包中已启用的任何日志提供程序中。 日志提供程序在数据存储区中存储有关事件的信息。 脚本组件还可以使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> 方法将信息记录到日志提供程序中而不引发事件。 有关如何使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> 方法的详细信息，请参阅以下内容。  
  
 为了引发事件，脚本任务将调用由 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 属性公开的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> 接口的以下方法之一：  
  
|事件|描述|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>|引发包中用户定义的自定义事件。|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A>|将错误情况通知包。|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireInformation%2A>|为用户提供信息。|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireProgress%2A>|将组件的进度通知包。|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A>|向包发出通知：组件处于有必要发出用户通知，但不是错误条件的状态。|  
  
 下面是一个简单的引发错误事件的示例：  
  
 `Dim myMetadata as IDTSComponentMetaData100`  
  
 `myMetaData = Me.ComponentMetaData`  
  
 `myMetaData.FireError(...)`  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 事件处理程序](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [在包中添加事件处理程序](../../integration-services-ssis-event-handlers.md)  
  
