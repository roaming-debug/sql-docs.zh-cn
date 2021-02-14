---
title: 从表导入数据
description: 批量导入表中的数据，并对模型进行数据更改。 使用此过程在 Master Data Services 数据库中添加、更新和删除数据。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad5b83b1-8e40-4ef8-9ba8-4ea17a58b672
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e9faf7de8d3e7517d47c61ba3a9b6e00cd2e2b09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347734"
---
# <a name="import-data-from-tables-master-data-services"></a>从表导入数据 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  你可以添加数据并将数据批量更改为 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中的模型。  
  
 **先决条件**  
  
-   您必须有权将数据插入 stg.<name。 \<name>_Leaf stg.<name。 \<name>_Consolidated stg.<name。 \<name>数据库中的 _Relationship 表 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 。  
  
-   您必须有权 \<name> 在数据库中执行 stg.udp_ _Leaf stg.<name \_ \<name> _Consolidated 或 stg.<name \_ \<name> _Relationship 存储过程 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 。  
  
-   模型的状态不能是“已提交” 。  
  
 **添加、更新和删除 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中的数据**  
  
1.  准备要导入 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中相应临时表的成员，包括必需字段的提供值。 有关临时表的概述，请参阅[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
    -   对于叶成员，表是 stg.<name。 \<name>_Leaf，其中 \<name> 指对应的实体。 有关所需字段的信息，请参阅[叶成员临时表 (Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
    -   对于合并成员，表是 stg.<name。 \<name>_Consolidated。 有关所需字段的信息，请参阅[合并成员临时表 (Master Data Services)](../master-data-services/consolidated-member-staging-table-master-data-services.md)。  
  
    -   为了移动显式层次结构中成员的位置，表是 stg.<name。 \<name>_Relationship。 有关所需字段的信息，请参阅[关系临时表 (Master Data Services)](../master-data-services/relationship-staging-table-master-data-services.md)。  
  
         有关移动显式层次结构中成员的概述，请参阅[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
    -   使用 **ImportType** 字段值来指定你要创建新的成员、停用成员或删除成员。 有关这些值的详细信息，请参阅[叶成员临时表 (Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)和[合并成员临时表 (Master Data Services)](../master-data-services/consolidated-member-staging-table-master-data-services.md)。  
  
         有关停用和删除成员的概述，请参阅[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
2.  打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 并为自己的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库连接到数据库引擎实例。  
  
     有关详细信息，请参阅 [SQL Server Management Studio](../ssms/sql-server-management-studio-ssms.md)。  
  
3.  使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导入和导出向导将数据导入到临时表。  
  
     有关更多信息，请参见 [SQL Server Import and Export Wizard](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md)。  
  
4.  通过执行下列任一操作，将数据从临时表加载 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 表  
  
    -   运行临时表（想要移动数据到的目标位置）相对应的临时存储过程。  
  
         有关临时存储过程和临时表的概述，请参阅[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。 有关临时存储过程参数以及代码示例的详细信息，请参阅[临时存储过程 (Master Data Services)](../master-data-services/staging-stored-procedure-master-data-services.md)。  
  
    -   使用主数据管理的“集成管理”  功能区域。  
  
         “临时批处理”  页上，在下拉列表中选择向其添加数据的模型，然后单击“开始批处理” 。 “状态”  字段将指示执行批处理的状态。 有关状态的详细信息，请参阅[导入状态 (Master Data Services)](../master-data-services/import-statuses-master-data-services.md)。  
  
         ![主数据管理器中的“临时批处理”页](../master-data-services/media/mds-stagingbatchespage.png "主数据管理器中的“临时批处理”页")  
  
         按照 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中的“临时批处理间隔”设置所确定的间隔启动临时过程。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
5.  查看在临时过程中出现的错误。 有关详细信息，请参阅[查看临时过程中出现的错误 (Master Data Services)](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)和[临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)。  
  
6.  根据业务规则对数据进行验证。  
  
     在主数据管理器中，导航到模型的“资源管理器”  功能区域，然后应用业务规则以验证数据。 有关详细信息，请参阅[业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)。 同时可以使用存储过程来验证数据。 有关详细信息，请参阅 [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)。  
  
     使用临时表加载数据时，不会自动根据业务规则验证数据。 有关执行何种验证以及何时执行的详细信息，请参阅[验证 (Master Data Services)](../master-data-services/validation-master-data-services.md)。  
  
