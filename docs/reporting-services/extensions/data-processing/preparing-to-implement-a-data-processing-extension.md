---
title: 准备实现数据处理扩展插件 | Microsoft Docs
description: 了解如何在 Reporting Services 中实现数据处理扩展插件。 了解可用接口以及所需和可选功能。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- interfaces [Reporting Services]
- data processing extensions [Reporting Services], implementing
ms.assetid: 698817e4-33da-4eb5-9407-4103e1c35247
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f057daa09caf7ba3d690fb0dc0586a5e28afc81e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065398"
---
# <a name="preparing-to-implement-a-data-processing-extension"></a>准备实现数据处理扩展插件
  在实现 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件之前，应定义要实现的接口。 你可能要提供整个接口组的特定于扩展插件的实现，或者只是要针对某一子集（例如 <xref:Microsoft.ReportingServices.DataProcessing.IDataReader> 和 <xref:Microsoft.ReportingServices.DataProcessing.IDbCommand> 接口）提供实现，客户端在其中主要与作为 DataReader 对象的结果集交互，并且使用 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理扩展插件作为结果集和数据源之间的桥梁。  
  
 然后，您可以通过以下两种方式之一实现数据处理扩展插件：  
  
-   数据处理扩展插件类可以实现 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 数据提供程序接口，并且可以选择实现 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的扩展数据处理扩展插件接口。  
  
-   您的数据处理扩展插件类可以实现 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的数据处理扩展插件接口，并且可以选择实现扩展数据处理扩展插件接口。  
  
 如果您的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件将不支持某一特定的属性或方法，则将该属性或方法作为无操作实现。 如果某一客户端期望特定的行为，则引发 NotSupportedException 异常。  
  
> [!NOTE]  
>  某一属性或方法的无操作实现只应用于您选择实现的那些接口的属性和方法。 您选择不实现的可选接口应排除在您的数据处理扩展插件程序集之外。 有关某一接口是必需接口还是可选接口的详细信息，请参阅本节后面的表。  
  
## <a name="required-extension-functionality"></a>必需的扩展插件功能  
 每个 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件都必须提供以下功能：  
  
-   打开与数据源之间的连接。  
  
-   分析查询并返回结果集的字段名的列表。  
  
-   对数据源执行查询，并返回行集。  
  
-   将单值参数传递到查询。  
  
-   遍历行集中的行，并检索数据。  
  
 可以扩展每个数据处理扩展插件以包含以下功能：  
  
-   分析某一查询，并返回该查询中所使用的参数名称的列表。  
  
-   分析某一查询，并返回按该查询分组的字段的列表。  
  
-   分析某一查询，并返回按该查询排序的字段的列表。  
  
-   提供用户名和密码以连接到独立于连接字符串的数据源。  
  
-   遍历行集中的行，并检索与数据值有关的辅助元数据。  
  
-   在服务器聚合数据。  
  
## <a name="available-extension-interfaces"></a>可用的扩展插件接口  
 下表介绍可用接口以及实现是必需的还是可选的。  
  
|接口|描述|实现|  
|---------------|-----------------|--------------------|  
|IDbConnection|表示与某一数据源的唯一会话。 在客户端/服务器数据库系统的情况下，该会话可等效于到服务器的一个网络连接。|必选|  
|IDbConnectionExtension|表示可由 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理扩展插件针对安全性和身份验证实现的附加连接属性。|可选|  
|IDbTransaction|表示本地事务。|必选|  
|IDbTransactionExtension|表示可由 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理扩展插件实现的附加事务属性。|可选|  
|IDbCommand|表示在连接到数据源时使用的查询或命令。|必选|  
|IDbCommandAnalysis|表示用于分析某一查询并返回在该查询中使用的参数名称列表的附加命令信息。|可选|  
|IDataParameter|表示传递到命令或查询的参数或名称/值对。|必选|  
|IDataParameterCollection|表示与某一命令或查询相关的所有参数的集合。|必选|  
|IDataReader|提供从数据源读取数据的只进、只读流的方法。|必选|  
|IDataReaderExtension|提供一种方法来读取一个或多个通过在数据源执行命令所获得的只进结果集流。 此接口为字段聚合提供附加支持。|可选|  
|IExtension|为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件提供基类。 还使实现者可为扩展插件包括本地化的名称并将配置设置从配置文件传递到扩展插件。|必选|  
  
 数据处理扩展插件接口将尽可能与 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 数据访问接口、方法和属性的子集完全相同。 有关实现完整 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 数据访问接口的详细信息，请参阅 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 软件开发包 (SDK) 文档中的“实现 .NET Framework 数据访问接口”。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [实现数据处理扩展插件](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
