---
title: 处理大型报表 | Microsoft Docs
description: 了解如何按照配置、呈现、部署和分发建议在 Report Server 中正确运行大型报表。
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report processing [Reporting Services], large reports
- page breaks [Reporting Services]
- large reports
- size [SQL Server], reports
- distributing reports [Reporting Services], large reports
ms.assetid: c5275a9f-c95b-46d7-bc62-633879a8a291
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 022751a3ec4b47a8e7a3a5f5295306265f659512
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594986"
---
# <a name="process-large-reports"></a>处理大型报表
  大型报表将给系统的处理能力带来一定的挑战。若要正确运行大型报表，需要进行特定配置。 除非将大型报表配置为支持分页，否则不应按需运行这些报表。  
  
> [!NOTE]  
>  默认情况下，将启用分页功能。 如果您认为报表将包含大量数据，请不要禁用分页功能。 用于最初呈现报表的 HTML 呈现格式将在浏览器中打开报表。 如果报表未分页，所有数据都将包含在一页中，而大多数浏览器均无法容纳这样的页面。 例如，几乎可以肯定，在浏览器的一页中不能显示包含 5,000 行数据的报表。  
  
 在使用大型报表时，应选择适合大型文档的报表执行、呈现和传递选项。 报表大小在很大程度上取决于从查询返回的行集，以及用于显示报表的呈现扩展插件。  
  
 对于包含可变数据的不同报表，从一个报表运行到下一个报表时，报表大小会有很大变化。 在这种情况下，应监视数据源，以确定数据可变性对报表的影响，决定是否需要按照本主题中规定的步骤进行操作。  
  
 有关如何诊断超时错误和内存不足错误的详细信息与提示，请参阅 blogs.msdn.microsoft.com 上的[如何在报表服务器中生成报表时诊断问题](/archive/blogs/lukaszp/)一文。  
  
## <a name="configuration-recommendations"></a>配置建议  
 关于报表执行、报表呈现和报表访问方面的建议包括以下几点：  
  
-   将报表设计为支持分页。 设置报表服务器每次发送一页报表。 这样，如果报表支持分页，您就可以控制流向浏览器的数据量。 有关详细信息，请参阅[预加载缓存 (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md)。  
  
-   将报表配置为以计划报表快照的形式运行，以避免按需运行。 不要为报表执行设置超时值。 在非高峰期运行报表。  
  
-   如果希望控制是否处理报表，则将报表配置为使用共享数据源。 使用共享数据源的优点之一就是可以禁用数据源。 禁用数据源可防止处理报表。  
  
-   如果希望节省磁盘空间，则禁用报表历史记录功能。 若要禁用报表历史记录功能，请清除“历史记录”属性页上的所有复选框。  
  
-   限制访问报表。 将报表配置为使用项级安全性，并且将默认角色分配替换为新角色分配，以便只有需要访问报表的用户才具有访问权。  
  
     默认情况下，用户可以打开在文件夹层次结构中看到的任意报表。 即便将报表配置为以快照形式运行，能够查看文件夹中报表项用户也可以打开报表。 如果报表非常大，那么当用户在 Web 门户中打开报表时，它可能会导致浏览器停止响应。  
  
## <a name="rendering-recommendations"></a>呈现建议  
 配置报表分发之前，关键是要了解哪些呈现客户端适合大型文档。 建议的格式为具有软分页功能的默认 HTML 呈现扩展插件，不过，您也可以选用支持分页的任何其他格式。  
  
 对于每一种呈现格式，其性能和内存占用各不相同。 根据所选择的格式，同一报表会按不同的速率呈现并需要不同的内存量。 速度最快且占用内存最少的格式包括 CSV、XML 和 HTML。 PDF 和 Excel 的性能最低，但其原因并不相同。 PDF 会占用大量 CPU 资源，而 Excel 会占用大量内存。 图像呈现介于这两者之间。 您可以在定义报表分发方式时指定格式。  
  
## <a name="deployment-and-distribution-recommendations"></a>部署和分发建议  
 如果使用分页功能控制报表呈现，则可以与部署任何其他报表一样来部署大型报表。 可以通过 Web 门户、SharePoint Web 部件或你添加到门户或网站的 URL 来提供对报表的访问。 所有这些部署选项均支持按需访问，以及以前运行的报表快照。  
  
 另一种部署策略是将报表分发给各个用户。 如果在意配置传递选项的方式，则可以通过订阅分发大型报表。 您可以使用标准订阅或数据驱动订阅来传递报表。 关于订阅和传递方面的建议包括以下几点：  
  
-   将订阅配置为使用 Web 存档 (MHTML)、PDF 或 Excel。  
  
-   如果使用的是 PDF 或 Excel，则将订阅配置为使用文件共享传递。 这样，一旦传递完报表，就可以使用桌面应用程序来处理报表。 您必须设置对文件共享位置的权限，以确定哪些用户可以查看报表。  
  
     请注意，一旦将报表放入文件共享位置，它就不再受 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的控制和保护。 如果希望在报表更新时得到通知，请创建第二个订阅，通过电子邮件传递方式专门发送通知。  
  
 如果希望使用电子邮件形式传递报表，请配置订阅以包含相应的链接。 避免以附件形式发送报表。  
  
## <a name="see-also"></a>另请参阅  
 [订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   
 [为报表数据源指定凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [报表服务器内容管理（SSRS 本机模式）](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [预加载缓存 (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md)