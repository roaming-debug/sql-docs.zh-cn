---
title: 数据处理扩展插件概述 | Microsoft Docs
description: 查看 Reporting Services 中包含的数据处理扩展插件，并了解如何将自定义数据处理添加到报表服务器。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], about extensions
ms.assetid: 1d652605-9313-4c75-98b4-ba4dcbbb222d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 61c30aa9214b87e9acf74dc12d71dd9e8e6d6fa5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061172"
---
# <a name="data-processing-extensions-overview"></a>数据处理扩展插件概述
  借助于 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的数据处理扩展插件，您可以连接到数据源并检索数据。 它们还可以充当数据源和数据集之间的桥梁。 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件是模仿 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 数据提供程序接口的子集创建的。  
  
 下表列出 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 随附的数据处理扩展插件。  
  
|数据处理扩展插件|说明|  
|-------------------------------|-----------------|  
|用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的数据处理扩展插件|使用用于 SQL Server 的 .NET Framework 数据提供程序可以连接到 [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)] 和从其检索数据。|  
|用于 OLE DB 的数据处理扩展插件|使用用于 OLE DB 的 .NET Framework 数据提供程序。 使用此扩展插件，报表服务器可以查询具有 OLE DB 访问接口的任何数据源。|  
|用于 Oracle 的数据处理扩展插件|使用用于 Oracle 的 .NET Framework 数据提供程序。 使用此扩展插件，报表服务器可以通过 Oracle 客户端连接软件访问 Oracle 数据源。|  
|用于 ODBC 的数据处理扩展插件|使用用于 ODBC 的 .NET Framework 数据提供程序。 使用此扩展插件，报表服务器可以访问存在有关 ODBC 驱动程序的任何数据库中的数据。|  
  
 可以使用 [!INCLUDE[ssRS](../../../includes/ssrs.md)] 数据处理 API 向您的报表服务器添加自定义数据处理。  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 具有对 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 中的数据访问接口的固有支持。 如果您已实现了完整的数据访问接口，则无需实现 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件。 不过，您应该考虑扩展数据访问接口，以便包括特定于 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2005 的功能，这包括安全连接凭据和服务器端聚合。  
  
 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中包括的每个数据处理扩展插件都使用一组通用的接口。 这确保每个扩展插件都实现类似的功能。  
  
 您可为自己的数据源开发数据处理扩展插件，或者可以使用接口向公共数据库基础结构添加用于数据处理的附加层。 您可以部署自定义数据处理扩展插件，实现数据与组织中的现有报表服务器的无缝集成。 还可以将它们用作提供给您的使用者的自定义报表套件的一部分。  
  
 ![数据处理扩展插件体系结构](../../../reporting-services/extensions/data-processing/media/bk-dataprocess-extensions.gif "数据处理扩展插件体系结构")  
Reporting Services 数据处理扩展插件体系结构  
  
 实现自定义 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件的好处包括：  
  
-   简化的数据访问体系结构，通常具有更好的可维护性和改进的性能。  
  
-   能够直接将特定于扩展插件的功能公开给使用者。  
  
-   可供您的使用者访问 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 内的数据源的特定接口。  
  
## <a name="data-extension-process-flow"></a>数据扩展插件处理流程  
 在开发自定义数据扩展插件前，您应该理解报表服务器是如何使用数据扩展插件处理数据的。 还应理解报表服务器调用的构造函数和方法。  
  
 ![数据处理扩展插件的处理流程](../../../reporting-services/extensions/data-processing/media/bk-ext-01.gif "数据处理扩展插件的处理流程")  
报表服务器调用的数据扩展插件的分步处理流程  
  
 该图说明下列事件序列：  
  
1.  报表服务器创建一个连接对象并传递到与报表相关联的连接字符串和凭据中。  
  
2.  用于创建命令对象的报表的命令文本。 在该过程中，数据处理扩展插件可包括用于分析命令文本和创建命令参数的代码。  
  
3.  一旦处理了命令对象和参数后，就将生成一个数据读取器，它返回结果集并使报表服务器能够将报表数据与报表布局相关联。  
  
## <a name="developer-requirements"></a>开发人员要求  
 开发 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据处理扩展插件要求您具备：  
  
-   一台安装了报表设计器或报表服务器的部署计算机。  
  
-   一台安装了 [!INCLUDE[vsprvsext](../../../includes/vsprvsext-md.md)] 或更高版本或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 软件开发包 (SDK) 的开发计算机。  
  
-   深入理解 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 功能。  
  
-   深入了解 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] 体系结构、[!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 数据提供程序、ADO.NET DataSet 对象和常用的 [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] 接口。  
  
-   使用 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 语言（例如，[!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# 或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET）进行开发的经验。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
