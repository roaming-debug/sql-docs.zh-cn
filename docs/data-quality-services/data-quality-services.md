---
description: “数据库引擎服务”
title: 数据库引擎服务
ms.date: 10/12/2013
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 9c6b996c-e768-4bf5-837f-5436ed9cea1d
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 12e37d3119341ebef2c3592581fd0fada018210e
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765819"
---
# <a name="data-quality-services"></a>数据库引擎服务

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssDQSnoversionLong](../includes/ssdqsnoversionlong-md.md)] (DQS) 是知识驱动型数据质量产品。 DQS 使您可以生成知识库，然后使用它执行各种关键数据质量任务，包括数据的更正、充实、标准化和消除重复。 DQS 通过使用引用数据访问接口提供的基于云的 Reference Data Services，使您可以执行数据清理。 DQS 还可以提供集成到数据质量任务的事件探查功能，使您可以分析数据的完整性。  
  
 DQS 由 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] 和 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]组成，它们都作为 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]的一部分安装。 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]是一个 SQL Server 实例功能，它包含具有数据质量功能和存储的三个 SQL Server 目录。 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]是一个 SQL Server 共享功能，企业用户、信息工作者和 IT 专业人员可以使用它执行计算机辅助的数据质量分析和以交互方式管理其数据质量。 您还可以使用 [!INCLUDE[ssDQSCleansingLong](../includes/ssdqscleansinglong-md.md)] 和 Master Data Services (MDS) 数据质量功能（它们都基于 DQS）来执行数据质量过程。  
  
 有关安装 DQS 的信息，请参阅 [Install Data Quality Services](../data-quality-services/install-windows/install-data-quality-services.md)。 如果要将现有的 DQS 版本升级到最新版本的 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] ，请参阅 [升级 Data Quality Services](../database-engine/install-windows/upgrade-data-quality-services.md)。  
  
 **按区域浏览内容**  
 [Data Quality Client 应用程序](../data-quality-services/data-quality-client-application.md)的![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标")  
  
 ![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标") [DQS 知识库和域](../data-quality-services/dqs-knowledge-bases-and-domains.md)  
  
 ![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标")[数据质量项目](../data-quality-services/data-quality-projects-dqs.md)  
  
 ![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标")[数据清理](../data-quality-services/data-cleansing.md)  
  
 ![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标")[数据匹配](../data-quality-services/data-matching.md)  
  
 "![小文件文件夹" 图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标") [Reference DATA Services in DQS](../data-quality-services/reference-data-services-in-dqs.md)  
  
 "![小文件文件夹" 图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标") [DQS 中的数据事件探查和通知](../data-quality-services/data-profiling-and-notifications-in-dqs.md)  
  
 ![小文件文件夹](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标")" [DQS 管理](../data-quality-services/dqs-administration.md)" 图标  
  
 ![小文件文件夹图标](/analysis-services/analysis-services/media/filefolder-small.png "小文件文件夹图标") [DQS 安全性](../data-quality-services/dqs-security.md)  
  
## <a name="see-also"></a>另请参阅  
 [Data Quality Services 简介](../data-quality-services/introduction-to-data-quality-services.md)   
 [Data Quality Services 概念](../data-quality-services/data-quality-services-concepts.md)   
 [DQS 资源](../sql-server/index.yml)   
 [SQL Server 资源中心](/previous-versions/sql/sql-server-2012/hh231622(v=sql.110))  
  
