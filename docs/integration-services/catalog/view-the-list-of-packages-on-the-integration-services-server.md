---
description: 查看 Integration Services 服务器上的包列表
title: 查看 Integration Services 服务器上的包列表 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 67a992d1-7524-4f4b-b3d8-ebd9e5ea82a8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a63c6b374377029d09df340f6888c1535d5b2012
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339265"
---
# <a name="view-the-list-of-packages-on-the-integration-services-server"></a>查看 Integration Services 服务器上的包列表

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用以下两种方式之一来查看存储在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上的包列表。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 若要查看在服务器上存储的包的列表，请查询视图 [catalog.packages（SSISDB 数据库）](../../integration-services/system-views/catalog-packages-ssisdb-database.md)。  
  
 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中  
 若要使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的对象资源管理器查看服务器上存储的包，请遵循以下过程。  
  
### <a name="to-view-packages-using-ssmanstudiofull"></a>使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。 也就是说，连接到承载 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 数据库的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 实例。  
  
2.  在对象资源管理器中，展开树以便显示 **“Integration Services 目录”** 节点。  
  
3.  展开 **“Integration Services 目录”** 节点以显示 **SSISDB** 节点。  
  
4.  展开 **SSISDB** 节点以显示一个或多个文件夹的列表。 每个文件夹包含 **“项目”** 文件夹中的一个或多个项目，而每个项目包含 **“包”** 文件夹中的一个或多个包。  
  
  
