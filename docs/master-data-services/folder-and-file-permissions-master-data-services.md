---
description: 文件夹和文件权限 (Master Data Services)
title: 文件夹和文件权限
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- security [Master Data Services], file and folder
- folders [Master Data Services]
- files [Master Data Services]
ms.assetid: 6402d81d-7349-47b1-95ca-99b0c0f4f373
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 5877e1837d0f132a3a4bc8b2cd36a46f3c331311
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344778"
---
# <a name="folder-and-file-permissions-master-data-services"></a>文件夹和文件权限 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在您安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时，文件夹和文件将安装在您为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 共享功能指定的安装路径处的文件系统中。 如果你使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 共享功能的默认安装路径，则 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 的安装路径为驱动器:\Program Files\Microsoft SQL Server\130\Master Data Services。 尽管您可以更改共享功能安装路径，但要注意从父文件夹继承的权限以及为 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]显式设置的权限。  
  
## <a name="inherited-permissions"></a>继承的权限  
 **Microsoft SQL Server** 文件夹、 **Master Data Services** 文件夹以及大多数子文件夹和文件从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装程序中指定的父文件夹继承权限。 如果选择默认安装位置，则继承其权限的父文件夹是驱动器:\Program Files。 下表描述针对 **“程序文件”** 的默认权限。  
  
> [!NOTE]  
>  如果修改针对“程序文件”的默认权限，或者选择不同的安装位置，则 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 文件夹和文件将相应继承其父文件夹的权限，并且这些权限可能不同于在下表中描述的权限。  
  
###### <a name="program-files-default-permissions"></a>程序文件默认权限  
  
|组或帐户名称|权限|  
|---------------------------|-----------------|  
|CREATOR OWNER|特殊权限|  
|SYSTEM|特殊权限|  
|管理员|特殊权限|  
|用户|读取和执行、列出文件夹内容、读取|  
|TrustedInstaller|列出文件夹内容、特殊权限|  
  
## <a name="explicit-permissions"></a>显式权限  
 **MDSTempDir** 文件夹和 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web.config 文件（位于 **WebApplication** 文件夹中）不继承权限。 它们具有在您安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时显式设置的权限，而与您选择的安装路径无关。 不要修改这些权限。  
  
###### <a name="mdstempdir-permissions"></a>MDSTempDir 权限  
  
|组或帐户名称|权限|  
|---------------------------|-----------------|  
|SYSTEM|修改、读取和执行、列出文件夹内容、读取、写入|  
|管理员|修改、读取和执行、列出文件夹内容、读取、写入|  
|MDS_ServiceAccounts|修改、读取和执行、列出文件夹内容、读取、写入|  
  
###### <a name="webconfig-permissions"></a>Web.config 权限  
  
|组或帐户名称|权限|  
|---------------------------|-----------------|  
|SYSTEM|完全控制、修改、读取和执行、读取、写入|  
|管理员|完全控制、修改、读取和执行、读取、写入|  
|MDS_ServiceAccounts|读取和执行、读取|  
  
 有关 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web.config 文件内容的详细信息，请参阅 [Web 配置参考 (Master Data Services)](../master-data-services/web-configuration-reference-master-data-services.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../master-data-services/install-windows/install-master-data-services.md)  
  
  
