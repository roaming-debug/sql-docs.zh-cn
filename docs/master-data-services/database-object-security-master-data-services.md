---
description: 数据库对象安全性 (Master Data Services)
title: 数据库对象安全性
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], object security
- security [Master Data Services], database objects
ms.assetid: dd5ba503-7607-45d9-ad0d-909faaade179
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: dd693c0f1bdbb7651375f6cba1e68a60b889db04
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339066"
---
# <a name="database-object-security-master-data-services"></a>数据库对象安全性 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中，数据存储在多个数据库表中并可以通过视图查看。 您在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序中受保护的信息对于具有 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库访问权限的用户是可见的。  
  
 例如，雇员薪金信息可能包含在 Employee 模型中，或公司财务信息可能包含在 Account 模型中。 您可以拒绝用户在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面中访问这些模型，但是具有数据库访问权限的用户可以查看此数据。  
  
 您可以授予对数据库对象的权限以使特定数据对用户可用。 有关授予权限的详细信息，请参阅 [GRANT 对象权限 (Transact-SQL)](../t-sql/statements/grant-object-permissions-transact-sql.md)。 有关保护 SQL Server 的详细信息，请参阅 [Securing SQL Server](../relational-databases/security/securing-sql-server.md)。  
  
 以下任务需要访问 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库：  
  
-   [暂存数据](#Staging)  
  
-   [根据业务规则验证数据](#rules)  
  
-   [删除版本](#Versions)  
  
-   [立即应用层次结构成员权限](#Hierarchy)  
  
-   [配置系统设置](#SysSettings)  
  
##  <a name="staging-data"></a><a name="Staging"></a> 临时处理数据  
 在下表中，每个安全对象都将“name”作为名称的一部分。 这指示在创建实体时指定的临时表的名称。 有关详细信息，请参阅[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
|操作|安全对象|权限|  
|------------|----------------|-----------------|  
|创建、更新和删除叶成员及其属性。|stg.name_Leaf|必需：INSERT<br /><br /> 可选：SELECT 和 UPDATE|  
|将数据从叶临时表加载到相应的 MDS 数据库表中。|stg.udp_name_Leaf|EXECUTE|  
|创建、更新和删除合并成员及其属性。|stg.name_Consolidated|必需：INSERT<br /><br /> 可选：SELECT 和 UPDATE|  
|将数据从合并临时表加载到相应的 MDS 数据库表中。|stg.udp_name_Consolidated|EXECUTE|  
|在一个显式层次结构中移动成员。|stg.name_Relationship|必需：INSERT<br /><br /> 可选：SELECT 和 UPDATE|  
|将数据从关系临时表加载到相应的 MDS 表中。|stg.udp_name_Relationship|EXECUTE|  
|查看在数据从临时表插入到 MDS 数据库表时发生的错误。|stg.udp_name_Relationship|SELECT|  
  
 有关详细信息，请参阅 [概述：从表导入数据 &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
##  <a name="validating-data-against-business-rules"></a><a name="rules"></a> 根据业务规则对数据进行验证  
  
|操作|安全对象|权限|  
|------------|---------------|-----------------|  
|根据业务规则验证数据版本|mdm.udpValidateModel|EXECUTE|  
  
 有关详细信息，请参阅 [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)。  
  
##  <a name="deleting-versions"></a><a name="Versions"></a> 删除版本  
  
|操作|安全对象|权限|  
|------------|----------------|-----------------|  
|确定要删除的版本的 ID|mdm.viw_SYSTEM_SCHEMA_VERSION|SELECT|  
|删除模型的版本|mdm.udpVersionDelete|EXECUTE|  
  
 有关详细信息，请参阅[删除版本 (Master Data Services)](../master-data-services/delete-a-version-master-data-services.md)。  
  
##  <a name="immediately-applying-hierarchy-member-permissions"></a><a name="Hierarchy"></a> 立即应用层次结构成员权限  
  
|操作|安全对象|权限|  
|------------|----------------|-----------------|  
|立即应用成员权限|mdm.udpSecurityMemberProcessRebuildModel|EXECUTE|  
  
 有关详细信息，请参阅[立即应用成员权限 (Master Data Services)](../master-data-services/immediately-apply-member-permissions-master-data-services.md)。  
  
##  <a name="configuring-system-settings"></a><a name="SysSettings"></a> 配置系统设置  
 可以配置系统设置来控制 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中的行为。 可以在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中调整这些设置，或者如果具有 UPDATE 访问权限，可以直接在 mdm.tblSystemSetting 数据库表中调整这些设置。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安全性 (Master Data Services)](../master-data-services/security-master-data-services.md)  
  
  
