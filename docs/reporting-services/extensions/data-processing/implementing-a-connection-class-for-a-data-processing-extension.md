---
title: 为数据处理扩展插件实现 Connection 类 | Microsoft Docs
description: 在 Reporting Services 中为数据处理扩展插件实现 Connection 对象。 查看要实现的接口以及客户端要求。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- connections [Reporting Services], data processing extensions
- Connection class
- data processing extensions [Reporting Services], connections
ms.assetid: 7047d29e-a2c9-4e6f-ad02-635851a38ed7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5b0c6cc93ea6b934be718e88873cf736f31d679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061672"
---
# <a name="implementing-a-connection-class-for-a-data-processing-extension"></a>为数据处理扩展插件实现 Connection 类
  Connection 对象表示数据库连接或类似的资源，它是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件的用户的起点。 它表示与数据库服务器的连接，尽管可以将任何具有类似行为的实体公开为 Connection。  
  
 若要实现 Connection 对象，请创建一个实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> 并可选实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> 的类。  
  
 在实现中，您必须确保先创建和打开一个连接，然后才能执行命令。 确保您的实现要求客户端显式打开和关闭连接，而不是让实现为客户端隐式打开和关闭连接。 在获取连接时，将执行安全检查。 如果在 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理扩展插件中要求将某个现有连接用于其他类，则可以确保在使用数据源时始终执行安全检查。  
  
 所需连接的属性表示为一个连接字符串。 强烈建议 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理扩展插件使用由 OLE DB 定义的所熟悉的名称/值对系统支持 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection.ConnectionString%2A> 属性。  
  
> [!NOTE]  
>  获取 Connection 对象通常需要消耗大量的资源，因此，可能需要考虑通过池连接或其他方法来减轻资源的占用。  
  
 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> 从 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 继承。 您必须将 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 接口作为连接类实现的一部分而实现。 借助于 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 接口，类可以实现本地化的扩展插件名称并处理存储在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置文件中的扩展插件特定的配置信息。  
  
 Connection 对象通过其 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 的实现包含 <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A> 属性。 强烈建议 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件支持 <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A> 属性，以便用户在用户界面（如报表管理器）中对于该扩展插件看到熟悉的本地化名称。  
  
 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 还使 Connection 对象能够检索和处理存储在 RSReportServer.config 文件中的自定义配置数据。 有关处理自定义配置数据的详细信息，请参阅 <xref:Microsoft.ReportingServices.Interfaces.IExtension.SetConfiguration%2A> 方法。  
  
 对于实现 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 的类，当卸载其他数据处理扩展插件类时，不会从内存中卸载该类。 因此，可以使用 Extension 类存储跨连接状态信息或存储内存中可能缓存的数据。 只要报表服务器正在运行，Extension 类就保持在内存中。  
  
 可以通过实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> 来扩展 Connection 类，以便在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中包含对于凭据的支持。 当实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> 接口的 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.IntegratedSecurity%2A>、<xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.UserName%2A> 和 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.Password%2A> 属性时，就在报表设计器的“数据源”对话框中启用了“集成安全性”复选框以及“用户名”和“密码”文本框。 这使报表设计器能够针对支持身份验证的数据源存储和检索凭据。 将以安全的方式存储凭据，当以预览模式呈现报表时将使用这些凭据。  
  
> [!NOTE]  
>  隐式实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> 要求您实现 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> 和 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 接口的成员。  
>   
>  有关示例 Connection 类实现，请参阅 [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889)（SQL Server Reporting Services 产品示例）。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [实现数据处理扩展插件](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
