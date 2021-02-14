---
description: 显示派生层次结构中的多对多关系 (Master Data Services)
title: 显示派生层次结构中的多对多关系
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8b2a9c43-40e0-48f7-a6a9-325beb9f27da
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f550a109d5271a216614d0566ed6f3a4d87be056
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336296"
---
# <a name="show-many-to-many-relationships-in-derived-hierarchies-master-data-services"></a>显示派生层次结构中的多对多关系 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  派生层次结构 (DH) 不仅可显示一对多关系，现在还可以显示多对多关系。  
  
## <a name="many-to-many-m2m-relationships"></a>多对多 (M2M) 关系  
 两个实体之间的多对多 (M2M) 关系可通过使用第三实体进行建模，第三实体用于在这两个实体之间提供映射：  
  
 ![mds_hierarchies_manytomany](../master-data-services/media/mds-hierarchies-manytomany.png "mds_hierarchies_manytomany")  
  
 在上面的示例中， **Employee** 和 **TrainingClass** 实体之间存在 M2M 关系，由映射实体 **ClassRegistration** 提供。 一个员工可以在多个类中注册为学生，而每个类可以包含多个学生。  
  
 在以前，派生层次结构无法创建 M2M 关系模型。 自 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]开始，现在可以创建如按类显示学生、或反转关系并显示按学生分组的类之类的派生层次结构。  
  
 首先，转到“派生层次结构”管理页并新建一个派生层次结构：  
  
 ![mds_hierarchies_add_derived_hierarchy](../master-data-services/media/mds-hierarchies-add-derived-hierarchy.png "mds_hierarchies_add_derived_hierarchy")  
  
 接下来，为新的派生层次结构添加级别（自下而上开始添加）。 在此示例中，我们想要显示按类分组的学生（员工）。 因此， **Employee** 实体将作为该层次结构中的叶级别并将作为第一个进行添加：  
  
 ![mds_hierarchies_edit_derived_hierarchy_one](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-one.PNG "mds_hierarchies_edit_derived_hierarchy_one")  
  
 请注意，在上面的屏幕截图中， **Employee** 实体显示在“当前级别”  下方的中间位置，作为唯一级别。 派生层次结构右侧的“预览”  只是显示了 **Employee** 实体的所有成员列表。 左侧的“可用级别”部分将显示除当前顶部级别 (**Employee**) 外可能会添加的级别。 大多数这些级别都是 **Employee** 实体上基于域的属性 (DBA)，包括 **Department** DBA。  
  
 从 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]开始，便提供了一种可用于创建 M2M 关系模型的新级别类型，例如 **Class（通过 ClassRegistration.Student 映射）**。 级别名称比其他名称更为详细，用以反映明确描述映射关系所需的额外信息。 将此级别拖放到“当前级别”部分中的 **Employee** 级别  ：  
  
 ![mds_hierarchies_edit_derived_hierarchy_two](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-two.PNG "mds_hierarchies_edit_derived_hierarchy_two")  
  
 现在预览将显示按注册的定型类进行分组的员工。 由于这是一个 M2M 关系，因此每个子成员可以拥有多个父级。 在上面的示例中，员工 **6 {Hillman，Reinout N}** 在 **1 {Master Data Services 101}** 和 **4 {Career-Limiting Moves}** 这两个类中注册为了学生。  
  
 此映射关系也可以反转显示，即按学生将类进行分组：  
  
 ![mds_hierarchies_available_entities_and_hierarchies](../master-data-services/media/mds-hierarchies-available-entities-and-hierarchies.PNG "mds_hierarchies_available_entities_and_hierarchies")  
  
 同样，我们会看到子级在多个父级下的显示方式：定型类 **1 {Master Data Services 101}** 同时显示在 **6 {Hillman, Reinout N}** 和 **40 {Ford, Jeffrey L}** 下方。  
  
 映射实体 **ClassRegistration** 的成员未在派生层次结构内的任何位置显示。 它们只用于定义层次结构中父级和子级成员之间的关系。  
  
 可以通过修改映射实体成员来编辑 M2M 关系，具体做法是执行以下任一操作。 M2M 关系在“派生层次结构资源管理器”页为只读。  
  
-   使用 Excel 中的 Master Data Services 外接程序或通过使用数据暂存修改“实体资源管理器”页上的映射实体成员。  
  
-   拖放“派生层次结构资源管理器” 页中各父级之间的子节点。  
  
     此方法可修改现有的成员（如可能）并根据需要添加新成员。 将不会删除现有成员。  
  
     例如，使用 ClassRegistration 映射实体将一名学生移动到未使用的节点时，相应的映射实体成员的类属性值将更改为 null，但不会删除该成员。 相反，当将一名学生从未使用的节点移动到某个类时，如果存在对应于该学生的现有映射成员并且类为 null，则将通过将类从 null 更改为新父级来修改该成员。 如果找不到任何此类成员，则会添加一个。  
  
     例如，如果映射实体除了包含用于定义父-子关系的两个属性之外还包含其他属性，则此过程可避免删除成员，以防止意外删除其他用户数据。 用户必须直接在映射实体上显式执行删除操作。  
  
 新的 M2M 级别可以显示在允许基于域的属性 (DBA) 级别的派生层次结构中的任意位置。 如上述示例中一样，M2M 级别可以显示在顶部。 它还可以显示在 DBA 级别（包括递归级别）的上方和/或下方。 也可以显示在显式层次结构（已弃用）顶端级别下方。 多个 M2M 关系可以在同一个派生层次结构中链接在一起。  
  
 正如其他派生层次结构级别一样，也可以隐藏 M2M 级别。  
   
### <a name="m2m-relationship-in-sample-model"></a><a name="M2MSample"></a> 示例模型中的 M2M 关系  
有关 M2M 关系的演示，请查看 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]附带的 Customer 示例模型中的 Region Climate 派生层次结构。   
  
如下图中所示，对此关系进行建模的级别名称是 ![mds_Number1](../master-data-services/media/mds-number1.png)**Climate（通过 RegionClimate.Region 进行映射）**。 ![mds_Number2](../master-data-services/media/mds-number2.png)**Preview** 显示按照与区域关联的气候类型分组的区域。 这是 M2M 关系，因为存在与多个气候（父级）关联的区域（子成员）。 例如， ![mds_Number3](../master-data-services/media/mds-number3.png)**APCR {Asia Pacific}** 与 ![mds_Number4](../master-data-services/media/mds-number4.png)**A {Tropical}** 和 ![mds_Number5](../master-data-services/media/mds-number5.png)**B {Dry}** 关联。  
  
![mds_M2MRelationship_Example_CustomerModel](../master-data-services/media/mds-m2mrelationship-example-customermodel.png)  
  
有关部署 Customer 示例模型以及 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]附带的其他示例模型的说明，请参阅 [部署示例模型和数据](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md)。   
  
## <a name="one-many-relationship"></a>一对多关系  
 DH 的成员可以是许多子成员的父级，但它通常不能有多个父级（有关例外情况，请参阅 [成员安全性](#bkmk_member_security)）。 例如，假设有两个实体：Employee 和 Department，其中每个员工各属于一个部门。 这种关系可通过向 Employee 实体添加基于域的属性 (DBA) 进行建模，该属性可以引用 Department 实体：  
  
 ![mds_hierarchies_onetomany](../master-data-services/media/mds-hierarchies-onetomany.png "mds_hierarchies_onetomany")  
  
 这是一种一对多的关系，因为每个员工都只属于一个部门，但每个部门可以有多个员工。 可以创建派生层次结构，用于显示按部门分组的员工：  
  
 ![mds_hierarchies_dh_screenshot](../master-data-services/media/mds-hierarchies-dh-screenshot.png "mds_hierarchies_dh_screenshot")  
  
##  <a name="member-security"></a><a name="bkmk_member_security"></a> 成员安全性  
 允许成员重复（允许成员具有多个父级）的层次结构不能用于分配成员安全权限。 例如：  
  
-   不能定位 null 递归（递归级别的每个成员都显示在根及其递归父级下）的递归派生层次结构 (RDH)。  
  
-   级别高于递归级别（递归级别的每个成员显示在其非递归父级及其递归父级下）的递归派生层次结构。  
  
-   具有 M2M 级别（子级可以映射到许多父级）的派生层次结构。  
  
## <a name="collections"></a>集合  
 已弃用集合和显式层次结构。 转换存储过程 (udpConvertCollectionAndConsolidatedMembersToLeaf) 可将集合成员转换为叶成员，并创建多对多派生层次结构以捕获集合成员信息。  
  
## <a name="see-also"></a>另请参阅  
 [派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
