---
title: 新增功能
description: 了解 Master Data Services SQL Server 2019 版中的更改、改进和更新。
ms.custom: ''
ms.date: 07/08/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad530f60-d480-4457-ba7a-93a10c8a1695
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7e731d14967d808aa57f3fa0e001e5e593238ffd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170429"
---
# <a name="what39s-new-in-master-data-services-mds"></a>Master Data Services (MDS) 中的新增功能

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本主题汇总了 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]版本中的更改和更新。 
  
 有关如何在 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]中组织数据的概述，请参阅 [Master Data Services 概述](../master-data-services/master-data-services-overview-mds.md)。 
  
 **若要安装 Master Data Services，请设置数据库和网站，然后部署示例模型，请参阅** [Master Data Services 概述 (MDS)](../master-data-services/master-data-services-overview-mds.md)版本中的更改和更新。  
  
 **下载**  
  
-   若要下载 [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]，请转到  **[评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** 。  
  
-   已经拥有 Azure 帐户？  然后转到 **[此处](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview)** 启动装有 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 的虚拟机。  
  
##  <a name="improved-performance"></a>改进的性能  
  
 改进了性能，可让你创建更大的模型，更有效地加载数据，并获得更好的整体性能。 这包括改进了 Microsoft Excel 外接程序的性能，可以缩短数据加载时间，并使外接程序能够处理更大的实体。  
  
 有关用于 Microsoft Excel 的外接程序的详细信息，请参阅 [用于 Microsoft Excel 的 Master Data Services 外接程序](../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)。  
  
 包括以下功能改进。  
  
-   按默认启用实体级数据压缩。 启用数据压缩后，将使用 SQL 行级压缩功能来压缩与表和索引相关的所有实体。 这极大地减少了读取或更新主数据时的磁盘 I/O，尤其是当主数据占用数百万行和/或占用大量的 NULL 值列时。  
  
     因为 SQL Server 引擎端的 CPU 使用率稍有增加，如果你的 CPU 绑定在服务器上，可以通过编辑实体来关闭数据压缩。  
  
     有关详细信息，请参阅[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md) 和[数据压缩](../relational-databases/data-compression/data-compression.md)。  
  
-   默认情况下，已启用动态内容压缩 IIS 功能。 这极大地减少了 xml 响应的大小，并可节省网络 I/O，不过会增加 CPU 使用率。 如果你的 CPU 绑定在服务器上，可以通过在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web.config 文件中添加以下设置来关闭数据压缩。  
  
    ```  
    <configuration>  
       \<system.webServer>  
          <urlCompression doStaticCompression="true" doDynamicCompression="false " />  
       \</system.webServer>  
    </configuration>  
  
    ```  
  
     有关详细信息，请参阅 [URL 压缩](https://www.iis.net/configreference/system.webserver/urlcompression)。  
  
-   以下新的 SQL Server 代理作业可执行索引和日志维护。  
  
    -   MDS_MDM_Sample_Index_Maintenace  
  
    -   MDS_MDM_Sample_Log_Maintenace  
  
 默认情况下，MDS_MDM_Sample_Index_Maintenance 作业每周运行一次。 你可以修改计划。 你也可以使用 udpDefragmentation 存储过程随时手动运行该作业。 每当插入或更新大量的主数据，或者从现有版本创建新版本后，建议你运行该存储过程。  
  
 碎片率超过 30% 的索引将在线重新生成。 重新生成期间，对同一个表的 CRUD 操作的性能将受影响。 如果性能下降会造成问题，建议你在非工作时间运行该存储过程。 有关索引碎片的详细信息，请参阅 [Reorganize and Rebuild Indexes](../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。  
  
 有关详细信息，请参阅这篇有关 Master Data Services 的博客文章： [Performance and Scale Improvement in SQL Server 2016](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-and-scale-improvement-in-sql-server-2016/ba-p/388225)（SQL Server 2016 中的性能和缩放性改进）。  
  
##  <a name="improved-security"></a>提高了安全性  
  
 新的超级用户功能权限可向用户或组授予 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]前一版本中服务器管理员具有相同的权限。 可将超级用户权限分配给多个用户和组。 在以前的版本中，最初安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 的用户就是服务器管理员，并且很难将此权限转移到另一个用户或组。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
 现在，可以在模型级别显式为用户分配管理员权限。 这意味着，如果以后在模型子树（例如实体级别）中为该用户分配了权限，该用户不会失去此管理员权限。  
  
 在此版本的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中，我们通过引入以下新权限来提供等级更高的权限：“读取”、“创建”、“更新”和“删除”。 例如，只拥有“更新”权限的用户现在无需创建或删除数据，即可更新主数据。 当你向用户分配“创建”、“更新”或“删除”权限时，系统会自动为该用户分配“读取”权限。 你还可以组合“读取”、“创建”、“更新”和“删除”权限。  
  
 升级到 [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时，旧权限将转换为下表中所示的新权限。  
  
|以前版本中的权限|新权限|  
|------------------------------------|--------------------|  
|最初安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 的用户拥有服务器管理员权限。|用户拥有超级用户功能权限|  
|用户在模型级别拥有“更新”权限，但在模型子树中没有权限，因此也是隐式的模型管理员。|用户在模型级别拥有显式管理员权限。|  
|用户拥有只读权限。|用户拥有“读取”访问权限。|  
|用户拥有“更新”权限。|用户拥有所有四种访问权限：“创建”、“更新”、“删除”和“读取”。|  
|用户拥有“拒绝”权限|用户拥有“拒绝”权限|  
  
 有关权限的详细信息，请参阅[安全性 (Master Data Services)](../master-data-services/security-master-data-services.md)。  
  
##  <a name="improved-transaction-log-maintenance"></a>改进的事务日志维护  
  
 现在，你可以按预定的间隔或按计划，使用“系统”设置以及在模型级别清理事务日志。 对于包含大量数据更改和 ETL 进程的 MDS 系统，这些表可能呈指数级增长，并导致性能下降和存储空间问题。  
  
 可以从日志中删除以下类型的数据。  
  
-   超过指定天数的事务历史记录。  
  
-   超过指定天数的验证问题历史记录。  
  
-   在指定天数以前运行的临时批处理。  
  
 你可以使用“系统”设置以及在模型级别配置从事务日志中删除数据的频率。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md) 和[创建模型 (Master Data Services)](../master-data-services/create-a-model-master-data-services.md)。 有关事务的详细信息，请参阅[事务 (Master Data Services)](../master-data-services/transactions-master-data-services.md)。  
  
 SQL Server 代理作业 (MDS_MDM_Sample_Log_Maintenace) 每晚运行，可触发清除事务日志。 你可以使用 SQL Server 代理修改此作业的时间表。  
  
 你也可以调用存储过程来清理事务日志。 有关详细信息，请参阅[事务 (Master Data Services)](../master-data-services/transactions-master-data-services.md)。  
  
## <a name="improved-troubleshooting"></a>提高了故障排除方便性  
  
 [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中增加了更多的功能来改进调试，并使问题排查更简便。 有关详细信息，请参阅[跟踪 (Master Data Services)](../master-data-services/tracing-master-data-services.md)。  
  
## <a name="improved-manageability"></a>提高了可管理性  
  
 可管理性的改进可帮助降低维护成本，并对投资回报 (ROI) 产生积极影响。 这些改进包括事务日志维护和安全性改进，以及以下新增功能。  
  
-   使用长度超过 50 个字符的属性名称。  
  
-   重命名和隐藏 Name 和 Code 属性。  
  
 有关详细信息，请参阅以下主题。  
  
-   [模型 (Master Data Services)](../master-data-services/models-master-data-services.md)  
  
-   [实体 (Master Data Services)](../master-data-services/entities-master-data-services.md)  
  
-   [事务 (Master Data Services)](../master-data-services/transactions-master-data-services.md)  
  
-   [安全性 (Master Data Services)](../master-data-services/security-master-data-services.md)  

## <a name="business-rule-improvements"></a>业务规则改进
 **管理业务规则（Excel 的 MDS 外接程序）**  
  
 在 Excel 的 Master Data Services 外接程序中，你可以管理业务规则，例如创建和编辑业务规则。 业务规则用于验证数据。  
 
 **业务规则扩展**  
  
 你可以将用户定义的 SQL 脚本应用为业务规则条件和操作的扩展。 可将 SQL 函数用作条件。 可将 SQL 存储过程用作操作。 有关详细信息，请参阅[业务规则扩展 (Master Data Services)](../master-data-services/business-rules-extension-master-data-services.md)。 
 
 **已重新设计业务规则管理体验**  
  
 MDS 中的业务规则管理体验经过完全重新设计，现已得到改进。 有关此功能的详细信息，请参阅[业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)。  
  
 **已从 Excel 的 MDS 外接程序中删除业务规则管理功能**  
  
 由于我们重新设计了体验，业务规则管理功能已从 Excel 的 MDS 外接程序中删除。    

 **新的业务规则条件**  
  
 添加了七个新的业务规则条件，以提供一套完整的条件。 有关详细信息，请参阅[业务规则条件 (Master Data Services)](../master-data-services/business-rule-conditions-master-data-services.md)。  

## <a name="derived-hierarchy-improvements"></a>派生层次结构改进

 **派生层次结构中的多对多关系**  
  
 现在，你可以创建显示多对多关系的派生层次结构。 两个实体之间的多对多关系可通过使用第三实体进行建模，第三实体用于在这两个实体之间提供映射。 映射实体是指具有两个或更多个引用其他实体的、基于域的属性的实体。  
  
 例如，实体 M 有一个基于域的属性引用 A，有一个基于域的属性引用 B。你可以使用映射实体创建从 A 到 B 的层次结构。  
  
 有关详细信息，请参阅[显示派生层次结构中的多对多关系 (Master Data Services)](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md)  
 
 **编辑派生层次结构中的多对多关系**  
  
 可以通过修改映射实体成员来编辑多对多关系。 有关详细信息，请参阅 [显示派生层次结构中的多对多关系 &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md)。  
 
 **改进了派生层次结构管理体验**  
  
 MDS 中的派生层次结构管理体验已得到改进。 有关此功能的详细信息，请参阅[创建派生层次结构 (Master Data Services)](../master-data-services/create-a-derived-hierarchy-master-data-services.md)。  
  
 由于我们重新设计了体验，业务规则管理功能已从 Excel 的 MDS 外接程序中删除。  
 
## <a name="attribute-improvements"></a>属性改进   
    
 **自定义索引**  
  
 您可以 (单个索引) 或实体中 (组合索引) 的特性列表创建非聚集索引，以帮助提高查询性能。 有关详细信息，请参阅[自定义索引 (Master Data Services)](../master-data-services/custom-index-master-data-services.md)。  
 
  **属性筛选器**  
  
 对于叶成员的基于域的属性，你可以使用筛选器父属性来约束该基于域的属性的允许值。 有关详细信息，请参阅[创建基于域的属性 (Master Data Services)](../master-data-services/create-a-domain-based-attribute-master-data-services.md)。  
 
## <a name="entity-and-member-improvements"></a>实体和成员的改进 
  
 **实体同步关系**  
  
 可以通过创建实体同步关系在不同的模型之间共享实体数据。 有关详细信息，请参阅[实体同步关系 (Master Data Services)](../master-data-services/entity-sync-relationship-master-data-services.md)。  
  
 **清除软删除的成员**  
  
 现在，你可以在模型版本中清除（永久删除）所有软删除的成员。 删除某个成员只是将其停用或软删除。 有关详细信息，请参阅[清除版本成员 (Master Data Services)](../master-data-services/purge-version-members-master-data-services.md)。  
 
## <a name="improvements-for-managing-changes"></a>管理更改的改进 
  
 **成员修订历史记录**  
  
 当成员发生变化时，系统都会记录成员修订历史记录。 你可以回滚修订历史记录，以及查看和批注修订。 使用“日志保留天数”属性可以指定历史数据的保留期限。  有关详细信息，请参阅[成员修订历史记录 (Master Data Services)](../master-data-services/member-revision-history-master-data-services.md)。  
  
 **合并冲突**  
  
 如果你尝试发布已由另一个用户更改的数据，发布将会失败并出现冲突错误。 若要解决此错误，可以执行合并冲突，然后重新发布更改。 有关详细信息，请参阅 [合并冲突 (Master Data Services)](../master-data-services/merge-conflicts-master-data-services.md) 和 [合并冲突（Excel 的 MDS 外接程序）](../master-data-services/microsoft-excel-add-in/merge-conflicts-mds-add-in-for-excel.md)。  
  
 **变更集**  
  
 你可以使用变更集将挂起的更改保存到实体，并可以查看和修改挂起的更改。 如果实体要求对更改进行审批，必须将挂起的更改保存到变更集并提交以供管理员审批。 有关详细信息，请参阅[变更集 (Master Data Services)](../master-data-services/changesets-master-data-services.md)。  
  
 **变更集电子邮件和管理**  
  
 在此版本中，你可以按模型与版本来查看和管理所有更改。 每当实体的变更集发生了需要审批的状态更改时，你还会收到电子邮件通知。 有关详细信息，请参阅[管理变更集 (Master Data Services)](../master-data-services/manage-changesets-master-data-services.md) 和[通知 (Master Data Services)](../master-data-services/notifications-master-data-services.md)。  
  
 **查看和管理修订历史记录**  
  
 你可以按实体与成员来查看和管理修订历史记录。 如果具有更新权限，可以将成员回退到以前的版本。 有关详细信息，请参阅[成员修订历史记录 (Master Data Services)](../master-data-services/member-revision-history-master-data-services.md)。  
 
## <a name="tool-and-sample-improvements"></a>工具和示例改进 
  
 **在 Excel 的 MDS 外接程序中保存或打开查询文件**  
  
 在“实体资源管理器”页中，你可以单击“Excel”来保存快捷查询文件。  或者，你可以在 Excel 的 MDS 外接程序中打开存储在计算机上的查询文件。 可以使用 QueryOpener 应用程序打开保存的文件。 有关详细信息，请参阅[快捷查询文件（Excel 的 MDS 外接程序）](../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)。  
  
 查询文件包含资源管理器页中的筛选器和层次结构信息。  
   
 **更新了示例模型部署包**  
  
 示例包已更新，可支持新的方案。 有关详细信息，请参阅 [SQL Server 示例：模型部署包 (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md)。  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 各个版本支持的 Master Data Services 和 Data Quality Services 功能](../master-data-services/master-data-services-and-data-quality-services-features-support.md)  
 [弃用的 Master Data Services 功能](../master-data-services/deprecated-master-data-services-features.md)  
 [弃用的 Master Data Services 功能](../master-data-services/discontinued-master-data-services-features.md)
