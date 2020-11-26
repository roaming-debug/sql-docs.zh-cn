---
title: 卸载和删除 Master Data Services | Microsoft Docs
description: 此卸载过程会删除 Master Data Services 文件夹和文件，并从本地计算机中卸载 Master Data Services 配置管理器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: efc2431c-588b-42e7-b23b-c875145a33f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: df0e0df9198e1d8abfeb0605a5a13cf0b22059c7
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96120912"
---
# <a name="uninstall-and-remove-master-data-services"></a>卸载和删除 Master Data Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  若要从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中卸载 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 功能，请按照[卸载现有 SQL Server 实例（安装程序）](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)中的步骤执行，并且在“选择功能”页上将 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 指定为要删除的功能。 卸载过程将从本地计算机删除 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 文件夹和文件，然后卸载 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] 。  
  
 为了防止数据丢失或避免影响系统中的其他计算机，卸载过程将不删除或更改某些项。 请查看下表确定是保留还是删除项。  
  
|Item|描述|  
|----------|-----------------|  
|文件夹和文件|卸载过程将从安装路径删除大多数文件夹和文件。<br /><br /> 卸载过程不从安装位置删除 Master Data Services 和 MDSTempDir 文件夹。 卸载过程完成后，您可以手动从文件系统删除这些文件夹。 有关详细信息，请参阅[文件夹和文件权限 (Master Data Services)](../../master-data-services/folder-and-file-permissions-master-data-services.md)。|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 程序集|卸载过程将从全局程序集缓存 (GAC) 中删除 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 程序集。|  
|数据库|卸载过程不影响 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库。 数据库在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中保持不变，这样您不会丢失任何数据，包括主数据、模型对象、用户和组权限、业务规则等。<br /><br /> 如果您不需要数据库，以后不希望将它连接到其他 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 网站或应用程序，则可能想从数据库所在的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中删除它。 有关详细信息，请参阅 [删除数据库](../../relational-databases/databases/delete-a-database.md)。|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 和 Web.config|卸载过程将从文件系统删除 WebApplication 文件夹。 WebApplication 文件夹包含 Web 应用程序文件和 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]的 Web.config 文件。<br /><br /> **\*\* 重要说明： \*\*** 在卸载前，你可能要将 Web.config 文件复制到其他位置，以保留该文件中的所有自定义设置或其他信息。 卸载过程完成后，Web.config 文件将不可恢复。|  
|Internet Information Services (IIS) 项|卸载过程不影响本地计算机上 IIS 中的任何应用程序池、网站或 Web 应用程序。 由于卸载过程删除 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]的 WebApplication 文件夹和 Web.config 文件，因此需要那些文件的所有 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序将不再提供内容。 如果用户尝试访问 Web 应用程序，将收到 HTTP 错误 500.19 - 内部服务器错误：“请求的页面无法访问，因为该页的相关配置数据无效。”<br /><br /> 如果您不再需要该网站或应用程序且应用程序池提供您的网站或应用程序，可以使用 IIS 工具来删除它们。 有关详细信息，请参阅 [TechNet 上的](https://go.microsoft.com/fwlink/?LinkId=184885) IIS 7 Operations Guide [!INCLUDE[msCoName](../../includes/msconame-md.md)] （IIS 7 操作指南）。|  
|**MDS_ServiceAccounts** 组|卸载过程完成后， **MDS_ServiceAccounts** Windows 组和任何已添加到该组的服务帐户将会被保留。 如果您不再需要该组和这些帐户，可以删除它们。|  
|注册表|卸载过程从 Windows 注册表中删除所有 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 注册表项。|  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
