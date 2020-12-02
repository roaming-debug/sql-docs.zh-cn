---
description: 系统变量
title: 系统变量 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- containers [Integration Services], variables
- tasks [Integration Services], variables
- system variables [Integration Services]
- event handlers [Integration Services], variables
- variables [Integration Services], system
ms.assetid: efecd0d4-1489-4eba-a8fe-275d647058b8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cf44509a479e66175d89c38c42595e418dc6c750
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92192398"
---
# <a name="system-variables"></a>系统变量

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供了一组系统变量，其中存储有关正在运行的包及其对象的信息。 可以在表达式和属性表达式中使用这些变量自定义包、容器、任务和事件处理程序。  
  
 可以在执行 SQL 任务用来将变量映射到参数的参数绑定中使用所有变量（系统和用户定义）。  
  
## <a name="system-variables-for-packages"></a>包的系统变量  
 下表介绍 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为包提供的系统变量。  
  
|系统变量|数据类型|说明|  
|---------------------|---------------|-----------------|  
|**CancelEvent**|Int32|Windows 事件对象的句柄，任务可以向其发送信号以指示任务应停止运行。|  
|**ContainerStartTime**|DateTime|容器的开始时间。|  
|**CreationDate**|DateTime|包的创建日期。|  
|**CreatorComputerName**|String|创建此包使用的计算机。|  
|**CreatorName**|String|包生成者的名称。|  
|**ExecutionInstanceGUID**|String|正在执行的包实例的唯一标识符。|  
|**FailedConfigurations**|String|失败的包配置的名称。|  
|**IgnoreConfigurationsOnLoad**|Boolean|指示在加载包时是否忽略包配置。|  
|**InteractiveMode**|Boolean|指示是否在交互模式中运行包。 如果包正在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中运行，则此属性将设置为 **True**。 如果包正在使用 **DTExec** 命令提示实用工具运行，则此属性设置为 **False**。|  
|**LocaleId**|Int32|包所使用的区域设置。|  
|**MachineName**|String|正在运行包的计算机的名称。|  
|**OfflineMode**|Boolean|指示该包是否处于脱机模式下。 脱机模式不获取与数据源的连接。|  
|**PackageID**|String|包的唯一标识符。|  
|**PackageName**|String|包的名称。|  
|**StartTime**|DateTime|包开始运行的时间。|  
|**ServerExecutionID**|Int64|在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务器上执行的包的执行 ID。<br /><br /> 默认值为零。 仅当该包在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务器上由 ISServerExec 执行时更改该值。 存在子包时，将该值从父包传递到子包。|  
|**UserName**|String|启动包的用户的帐户。 用户名由域名限定。|  
|**VersionBuild**|Int32|包版本。|  
|**VersionComment**|String|包版本的注释。|  
|**VersionGUID**|String|版本的唯一标识符。|  
|**VersionMajor**|Int32|包的主版本。|  
|**VersionMinor**|Int32|包的次版本。|  
  
## <a name="system-variables-for-containers"></a>容器的系统变量  
 下表介绍 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为 For 循环、Foreach 循环和序列容器提供的系统变量。  
  
|系统变量|数据类型|说明|容器|  
|---------------------|---------------|-----------------|---------------|  
|**LocaleId**|Int32|容器使用的区域设置。|For 循环容器<br /><br /> Foreach 循环容器<br /><br /> 序列容器|  
  
## <a name="system-variables-for-tasks"></a>任务的系统变量  
 下表介绍 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为任务提供的系统变量。  
  
|系统变量|数据类型|说明|  
|---------------------|---------------|-----------------|  
|**CreationName**|String|任务的名称。|  
|**LocaleId**|Int32|任务所使用的区域设置。|  
|**TaskID**|String|任务实例的唯一标识符。|  
|**TaskName**|String|任务实例的名称。|  
|**TaskTransactionOption**|Int32|任务使用的事务选项。|  
  
## <a name="system-variables-for-event-handlers"></a>事件处理程序的系统变量  
 下表介绍 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为事件处理程序所提供的系统变量。 并非所有变量对所有事件处理程序都可用。  
  
|系统变量|数据类型|说明|事件处理程序|  
|---------------------|---------------|-----------------|-------------------|  
|**取消**|Boolean|指示在出现错误、警告或查询取消时事件处理程序是否停止运行。|OnError 事件处理程序<br /><br /> OnWarning 事件处理程序<br /><br /> OnQueryCancel 事件处理程序|  
|**ErrorCode**|Int32|错误标识符。|OnError 事件处理程序<br /><br /> OnInformation 事件处理程序<br /><br /> OnWarning 事件处理程序|  
|**ErrorDescription**|String|对错误的说明。|OnError 事件处理程序<br /><br /> OnInformation 事件处理程序<br /><br /> OnWarning 事件处理程序|  
|**ExecutionStatus**|Boolean|当前的执行状态。|OnExecStatusChanged 事件处理程序|  
|**ExecutionValue**|DBNull|执行值。|OnTaskFailed 事件处理程序|  
|**LocaleId**|Int32|事件处理程序所使用的区域设置。|所有事件处理程序|  
|**PercentComplete**|Int32|已完成工作的百分比。|OnProgress 事件处理程序|  
|**ProgressCountHigh**|Int32|64 位值的高位部分，指示由 OnProgress 事件处理的操作的总数。|OnProgress 事件处理程序|  
|**ProgressCountLow**|Int32|64 位值的低位部分，指示由 OnProgress 事件处理的操作的总数。|OnProgress 事件处理程序|  
|**ProgressDescription**|String|进度说明。|OnProgress 事件处理程序|  
|**Propagate**|Boolean|指示是否将该事件传播到较高等级的事件处理程序。<br /><br /> 注意：在包的验证过程中将忽略 **Propagate** 变量的值。 如果在子包中将 **Propagate** 设置为 **False** ，这并不会防止事件向上传播至父包。|所有事件处理程序|  
|**SourceDescription**|String|事件处理程序中引发事件的可执行文件的说明。|所有事件处理程序|  
|**SourceID**|String|引发事件的事件处理程序中可执行文件的唯一标识符。|所有事件处理程序|  
|**SourceName**|String|引发事件的事件处理程序中可执行文件的名称。|所有事件处理程序|  
|**VariableDescription**|String|变量说明。|OnVariableValueChanged 事件处理程序|  
|**VariableID**|String|变量的唯一标识符。|OnVariableValueChanged 事件处理程序|  
  
## <a name="system-variables-in-parameter-bindings"></a>参数绑定中的系统变量  
 通常在包运行时，需要在表中保存系统变量的值。 例如，一个包动态创建一个表，并在表列中写入创建该表的包执行实例的 GUID。  
  
 如果在执行 SQL 任务所使用的 SQL 语句中使用系统变量来映射参数，那么重要的是要将每个参数绑定的数据类型设置为系统变量的数据类型。 否则，系统变量的值可能被错误转换。 例如，如果在具有 GUID 数据类型的参数绑定中使用 **ExecutionInstanceGUID** 系统变量，而该变量具有字符串数据类型，并包含表示包的执行实例的 GUID 的字符串，则包实例的 GUID 将被错误转换。  
  
 此规则也适用于用户定义的变量。 但是，由于无法更改系统变量的数据类型，并且必须调整对这些变量的使用以适应该数据类型，因此用户定义变量更为灵活。 在参数绑定中使用的用户定义变量通常是以与映射到它们的参数的数据类型兼容的数据类型来定义的。  
  
## <a name="related-tasks"></a>Related Tasks  
 [在执行 SQL 任务中将查询参数映射到变量](./control-flow/execute-sql-task.md)  
  
