---
description: Integration Services (SSIS) 服务器和目录
title: Integration Services (SSIS) 服务器和目录 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- packages [Integration Services], managing
- managing packages [Integration Services]
ms.assetid: 6d667bba-7c25-492a-8f4d-70ebaca28f40
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0883def093b7f1500d80c03622be0e9c0a4fd72
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354972"
---
# <a name="integration-services-ssis-server-and-catalog"></a>Integration Services (SSIS) 服务器和目录

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  当您在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中设计和测试包后，可将包含包的项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
 在对象资源管理器中的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器是承载 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 数据库的 **ssDEnoversion** 实例。 该数据库存储以下对象：包、项目、参数、权限、服务器属性和操作历史记录。  
  
 **SSISDB** 数据库在您可以查询的公共视图中公开对象信息。 数据库还提供存储过程，您可以调用这些存储过程以管理该对象。  
  
 在您可以将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器之前，需要创建 **ssDEnoversion** 目录。  
  
 若要大致了解 SSISDB 目录功能，请参阅 [SSIS 目录](../../integration-services/catalog/ssis-catalog.md)。  
  
## <a name="high-availability"></a>高可用性  
 像其他用户数据库一样，SSISDB 数据库不支持数据库镜像和复制。 若要详细了解镜像和复制，请参阅[数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
 还可以利用 SSIS 和 Always On 可用性组，提供高可用性的 SSISDB 及其内容。 有关详细信息，请参阅[对 SSIS 目录 (SSISDB) 使用 Always On](ssis-catalog.md#always-on-for-ssis-catalog-ssisdb)。 另请参阅 blogs.msdn.com 上 Matt Masson 的博客文章[结合使用 SSIS 和 Always On](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-alwayson/ba-p/388091)。  
  
##  <a name="integration-services-server-in-sql-server-management-studio"></a><a name="ssms"></a> SQL Server Management Studio 中的 Integration Services 服务器  
 在您连接到承载 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 数据库的 **ssDEnoversion** 实例时，您将在对象资源管理器中看到下列对象：  
  
-   **SSISDB 数据库**  
  
     在对象资源管理器中的 **“数据库”** 节点下，您可以看到 **SSISDB** 数据库。 您可以查询视图和调用存储过程，这些存储过程既管理 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器，也管理在该服务器上存储的对象。  
  
-   **Integration Services 目录**  
  
     在 **“Integration Services 目录”** 节点下，有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目和环境的文件夹。  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [查看 Integration Services 服务器上的包列表](../../integration-services/catalog/view-the-list-of-packages-on-the-integration-services-server.md)  
  
-   [部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)  
  
-   [运行 Integration Services (SSIS) 包](../../integration-services/packages/run-integration-services-ssis-packages.md)  
  
## <a name="related-content"></a>相关内容  
 blogs.msdn.com 上的博客文章[结合使用 SSIS 和 Always On](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-alwayson/ba-p/388091)。  
  
  
