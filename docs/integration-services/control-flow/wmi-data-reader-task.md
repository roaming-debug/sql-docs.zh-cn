---
description: WMI 数据读取器任务
title: WMI 数据读取器任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmidatareadertask.f1
- sql13.dts.designer.wmidatareadertask.general.f1
- sql13.dts.designer.wmidatareadertask.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Data Reader task [Integration Services]
ms.assetid: dae57067-0275-4ac3-8f34-1b9d169f1112
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d246e590cd54aca1b3a6ac911ec12d2835c8d91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339237"
---
# <a name="wmi-data-reader-task"></a>WMI 数据读取器任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  WMI 数据读取器任务使用 Windows Management Instrumentation (WMI) 查询语言来运行查询，此查询语言从 WMI 返回有关计算机系统的信息。 可以将 WMI 数据读取器任务用于下列目的：  
  
-   查询本地或远程计算机上的 Windows 事件日志并将此信息写入文件或变量。  
  
-   获取有关硬件组件的存在、状态或属性的信息，然后使用此信息决定控制流中的其他任务是否应该运行。  
  
-   获取应用程序的列表，并确定每个应用程序安装的是何种版本。  
  
 可以通过以下方式配置 WMI 数据读取器任务：  
  
-   指定要使用的 WMI 连接管理器。  
  
-   指定 WQL 查询的源。 查询可以存储在任务属性中，也可以存储在任务之外的变量或文件中。  
  
-   定义 WQL 查询结果的格式。 该任务支持表、属性名称/值对或属性值三种格式。  
  
-   指定查询目标。 目标可以是变量或文件。  
  
-   指示覆盖、保留还是追加查询目标。  
  
 如果源或目标是文件，则 WMI 数据读取器任务使用文件连接管理器连接到该文件。 有关详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
 WMI 数据读取器任务使用 WMI 连接管理器连接到该任务从中读取 WMI 信息的服务器。 有关详细信息，请参阅 [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md)。  
  
## <a name="wql-query"></a>WQL 查询  
 WQL 是 SQL 的方言，其扩展插件支持 WMI 事件通知和其他 WMI 特定功能。 有关 WQL 的详细信息，请参阅 [MSDN Library](../../sql-server/index.yml)中的 Windows Management Instrumentation 文档。  
  
> [!NOTE]  
>  WMI 类因 Windows 版本的不同而异。  
  
 下列 WQL 查询返回应用程序日志事件中的项。  
  
```  
SELECT * FROM Win32_NTLogEvent WHERE LogFile = 'Application' AND (SourceName='SQLISService' OR SourceName='SQLISPackage') AND TimeGenerated > '20050117'  
```  
  
 下列 WQL 查询返回逻辑磁盘信息。  
  
```  
SELECT FreeSpace, DeviceId, Size, SystemName, Description FROM Win32_LlogicalDisk  
```  
  
 下列 WQL 查询返回操作系统的快速修补工程 (QFE) 更新列表。  
  
```  
Select * FROM Win32_QuickFixEngineering  
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-data-reader-task"></a>WMI 数据读取器任务可用的自定义日志记录消息  
 下表列出了 WMI 数据读取器任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|指示任务已开始读取 WMI 数据。|  
|**WMIDataReaderOperation**|报告任务所运行的 WQL 查询。|  
  
## <a name="configuration-of-the-wmi-data-reader-task"></a>配置 WMI 数据读取器任务  
 可以采用编程方式或通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关以编程方式设置这些属性的信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiDataReaderTask.WmiDataReaderTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="wmi-data-reader-task-editor-general-page"></a>WMI 数据读取器任务编辑器（“常规”页）
  可以使用 **“WMI 数据读取器任务编辑器”** 对话框的 **“常规”** 页，对 WMI 数据读取器任务进行命名和说明。  
  
  有关 WMI 查询语言 (WQL) 的详细信息，请参阅 MSDN 库中的 Windows Management Instrumentation 主题 [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)（利用 WQL 进行查询）。  
  
### <a name="options"></a>选项  
 **名称**  
 为 WMI 数据读取器任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对 WMI 数据读取器任务的说明。  
  
## <a name="wmi-data-reader-task-editor-wmi-options-page"></a>WMI 数据读取器任务编辑器（“WMI 选项”页）
  可以使用“WMI 数据读取器任务编辑器”  对话框的“WMI 选项”  页，指定 Windows Management Instrumentation 查询语言 (WQL) 查询的源和查询结果的目标。  
  
 有关 WMI 查询语言 (WQL) 的详细信息，请参阅 MSDN 库中的 Windows Management Instrumentation 主题 [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)（利用 WQL 进行查询）。  
  
### <a name="static-options"></a>静态选项  
 **WMIConnectionName**  
 从列表中选择 WMI 连接管理器，或单击 \<**New WMI Connection...**> 创建新的连接管理器。  
  
 **相关主题：** [WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)、[WMI 连接管理器编辑器](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 选择任务运行的 WQL 查询的源类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**直接输入**|为 WQL 查询设置源。 选择此值将显示动态选项 **WQLQuerySourceType**。|  
|**文件连接**|选择包含 WQL 查询的文件。 选择此值将显示动态选项 **WQLQuerySourceType**。|  
|**变量**|将源设置为定义 WQL 查询的变量。 选择此值将显示动态选项 **WQLQuerySourceType**。|  
  
 **OutputType**  
 指定输出是数据表、属性值还是属性名称和值。  
  
 **OverwriteDestination**  
 指定是保留、覆盖还是追加到目标文件或变量中的原始数据。  
  
 **目标类型**  
 选择任务运行的 WQL 查询的目标类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**文件连接**|选择用于保存 WQL 查询结果的文件。 选择此值将显示动态选项 **DestinationType**。|  
|**变量**|设置用于存储 WQL 查询结果的变量。 选择此值将显示动态选项 **DestinationType**。|  
  
### <a name="wqlquerysourcetype-dynamic-options"></a>WQLQuerySourceType 动态选项  
  
#### <a name="wqlquerysourcetype--direct-input"></a>WQLQuerySourceType = 直接输入  
 **WQLQuerySource**  
 提供查询，或单击省略号 (…)，然后使用“WQL 查询”对话框输入查询  。  
  
#### <a name="wqlquerysourcetype--file-connection"></a>WQLQuerySourceType = 文件连接  
 **WQLQuerySource**  
 从列表中选择文件连接管理器，或单击“\<**New connection...**>”创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysourcetype--variable"></a>WQLQuerySourceType = 变量  
 **WQLQuerySource**  
 在列表中选择变量，或单击“\<**New variable...**>创建一个新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)  
  
### <a name="destinationtype-dynamic-options"></a>DestinationType 动态选项  
  
#### <a name="destinationtype--file-connection"></a>DestinationType = 文件连接  
 **目标**  
 从列表中选择文件连接管理器，或单击 \<**New connection...**> 创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)  
  
#### <a name="destinationtype--variable"></a>DestinationType = 变量  
 **目标**  
 在列表中选择变量，或单击 \<**New variable...**> 创建新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
