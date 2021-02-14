---
title: Master Data Services 数据库
description: Master Data Services 数据库包含 Master Data Services 系统的所有信息，并且是 Master Data Services 部署的核心。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], about the database
- database [Master Data Services]
ms.assetid: 5f590cc1-6ec2-4b8c-a598-03de0f6051a0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 59d430ea44a83d3795d7d3e3473917ed1c14345f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338205"
---
# <a name="master-data-services-database"></a>Master Data Services 数据库

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  该数据库包含 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 系统的所有信息。 它是 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 部署的关键。 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库：  
  
-   存储 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 系统所需的设置、数据库对象和数据。  
  
-   包含用于处理源系统的数据的临时表。  
  
-   提供架构和数据库对象来存储源系统的主数据。  
  
-   支持版本控制功能，包括业务规则验证和电子邮件通知。  
  
-   为需要从数据库中检索数据的订阅系统提供视图。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [叶成员临时表 (Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [合并成员临时表 (Master Data Services)](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [关系临时表 (Master Data Services)](../master-data-services/relationship-staging-table-master-data-services.md)  
  
-   [临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [创建 Master Data Services 数据库](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [数据库对象安全 &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)   
 [数据库登录名、用户和角色 &#40;Master Data Services&#41;](../master-data-services/database-logins-users-and-roles-master-data-services.md)  
  
  
