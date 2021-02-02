---
title: 从表导入数据
description: 在 Master Data Services 中为数据创建模型后，从表中导入数据，并对数据进行更改。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], about staging process
- importing data [Master Data Services]
- staging process [Master Data Services]
ms.assetid: 181d1e22-379c-45d1-b03c-e1e22ff14164
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b5290d2e9650acc8ee540e572109b51b377e5235
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236574"
---
# <a name="overview-importing-data-from-tables-master-data-services"></a>概述：导入表中数据 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中创建数据模型后，你便可以开始添加数据，并能更改数据。   你使用 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 临时表、存储过程和主数据管理器。  
  
 有关如何添加和修改数据的说明，请参阅[导入表中的数据 (Master Data Services)](../master-data-services/import-data-from-tables-master-data-services.md)。  
  
> [!NOTE]
>  你还可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]，将 Excel 中的数据添加到 MDS 存储库（[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库）中。 有关详细信息，请参阅[概述：从 Excel 导入数据（用于 Excel 的 MDS 外接程序）](../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)。  
  
 在添加和修改数据时，你可以执行以下操作。  
  
-   加载和更新成员，并更新属性值  
  
-   停用和删除成员  
  
-   移动显式层次结构成员  
  
 添加和更新数据包括以下主要任务。  
  
1.  将数据加载到 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中的临时表中。  
  
2.  将数据从临时表加载到相应的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 表中。  
  
     你使用临时存储过程或 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 来加载数据。  
  
> [!NOTE]  
>  在 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]中，已停止提供对 [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] 临时过程的支持。  
  
## <a name="deactivating-and-deleting-members-mds"></a>停用和删除成员 (MDS)  
 停用意味着可以重新激活成员。 如果您重新激活某成员，可还原成员的属性以及成员在层次结构和集合中的成员身份。 以前的所有事务都将保持不变。 管理员可以在主数据管理器的“版本管理”  功能区域中查看停用事务。  
  
 删除意味着从系统中永久清除成员。 将永久删除该成员的所有事务、所有关系和所有属性。  
  
> [!NOTE]  
>  不能使用临时过程来重新激活成员。 你必须在主数据管理器中手动执行此操作。 有关详细信息，请参阅[重新激活成员或集合 (Master Data Services)](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)。  
>   
>  不能使用临时过程来删除或停用集合。 有关手动停用集合的详细信息，请参阅[删除成员或集合 (Master Data Services)](../master-data-services/delete-a-member-or-collection-master-data-services.md)。  
  
## <a name="moving-explicit-hierarchy-members-mds"></a>移动显式层次结构成员 (MDS)  
 当你批量移动成员在显式层次结构中的位置时，你可以指定以下内容。  
  
-   作为合并成员的父级的合并成员。  
  
-   作为叶成员的父级的合并成员。  
  
-   作为叶成员或合并成员的同级的叶成员。  
  
-   作为叶成员或合并成员的同级的合并成员。  
  
## <a name="staging-tables-and-stored-procedures-mds"></a>临时表和存储过程 (MDS)  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库包含以下类型的临时表，你可以使用你的数据填充它们。  
  
-   [叶成员临时表 (Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [合并成员临时表 (Master Data Services)](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [关系临时表 (Master Data Services)](../master-data-services/relationship-staging-table-master-data-services.md)  
  
 模型中的每个实体都有一个临时表。 表名称指示相应的实体以及实体类型，如叶成员。 下图显示货币、客户和产品实体的临时表。  
  
 ![MDS 数据库中的临时表](../master-data-services/media/mds-staging-tables.png "MDS 数据库中的临时表")  
  
 该表的名称在创建实体时指定，且不可更改。 如果临时表的名称包含 _1 或其他数字，则在创建实体时已存在带此名称的其他表。  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 包括以下类型的临时存储过程。  
  
-   stg.udp_ \<name> _Leaf  
  
-   stg.udp_ \<name> _Consolidated  
  
-   stg.udp_ \<name> _Relationship  
  
 对于模型中的每个实体，有三个对应于叶成员、合并成员和关系临时表的存储过程。  下图显示货币、客户和产品实体的临时存储过程。  
  
 ![MDS 数据库中的临时存储过程](../master-data-services/media/mds-staging-storedprocedures.png "MDS 数据库中的临时存储过程")  
  
 有关存储过程的详细信息，请参阅[临时存储过程 (Master Data Services)](../master-data-services/staging-stored-procedure-master-data-services.md)。  
  
## <a name="logging-transactions-mds"></a>记录事务 (MDS)  
 导入或更新数据或关系时，可以记录发生的所有事务。 存储过程中的选项允许进行此日志记录。 如果使用 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]启动临时过程，则不进行日志记录。  
  
 在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]中， **“记录临时事务”** 设置不应用于暂存数据的此方法。  
  
## <a name="related-content"></a>相关内容  
  
-   [验证 (Master Data Services)](../master-data-services/validation-master-data-services.md)  
  
-   [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  
