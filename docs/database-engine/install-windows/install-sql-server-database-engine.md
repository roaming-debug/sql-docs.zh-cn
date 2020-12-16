---
title: 安装 SQL Server 数据库引擎 | Microsoft Docs
description: 了解从 SQL Server 安装向导的“要安装的组件”中选择 SQL Server 数据库引擎时可以安装的功能。
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], installing
ms.assetid: d0876e7f-aa52-4dd7-bd5c-029e2ffded5f
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 052ff4a8c3d47ba702db0e60aaee5d47817de734
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438785"
---
# <a name="install-sql-server-database-engine"></a>安装 SQL Server 数据库引擎

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

## <a name="overview"></a>概述
[!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件是用于存储、处理数据和保证数据安全的核心服务。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 提供受控的访问和快速事务处理，以满足企业中要求极高、大量使用数据的应用程序的要求。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持在同一台计算机上最多存在 50 个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例。 若要创建典型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，请参阅[使用安装向导安装 SQL Server（安装程序）](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)。  
  
>[!IMPORTANT]
>对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。  

## <a name="features"></a>功能
在 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导的“要安装的组件”页上选择** 数据库引擎” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，将安装下列功能：  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
  
-   [SQL Server 复制](../../relational-databases/replication/sql-server-replication.md) - 可选组件  

::: moniker range=">=sql-server-ver15"
-   [机器学习服务](../../machine-learning/install/sql-machine-learning-services-windows-install.md)（R 和 Python）以及[语言扩展](../..//language-extensions/install/windows-java.md) (Java) - 是一个可选组件
::: moniker-end

::: moniker range=">=sql-server-2017 <=sql-server-2017"
-   [机器学习服务（数据库内）](../../machine-learning/install/sql-machine-learning-services-windows-install.md)（R 和 Python）- 是一个可选组件
::: moniker-end

::: moniker range=">=sql-server-2016 <=sql-server-2016"
-   [R Services（数据库内）](../../machine-learning/install/sql-r-services-windows-install.md)- 是一个可选组件
::: moniker-end

-   全文搜索 - 是一个可选组件  
  
-   Data Quality Services - 一个可选组件  
  
    > [!NOTE]  
    >  在此版本中，在安装程序中选中“Data Quality Services”复选框不会安装 Data Quality Services (DQS) 服务器。 您必须执行其他安装后步骤以安装 DQS 服务器。 有关详细信息，请参阅 [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)。  
    
- [针对外部数据的 PolyBase 查询服务](../../relational-databases/polybase/polybase-guide.md) - 可选组件。 从 SQL Server 2019 开始，还可以使用用于 HDFS 数据源的 Java 连接器。

  
 下列附加功能是许多典型用户应用场景的可选项：  
  
-   数据质量客户端
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]
-   连接组件
-   编程模型
-   管理工具
-   [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]
-   文档组件  
  

> [!NOTE]  
>  默认情况下，不会将示例数据库和示例代码作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的一部分进行安装。 若要安装示例数据库和示例代码，请参阅 [Microsoft SQL Server 示例](../../samples/sql-samples-where-are.md)。 请参阅 [CodePlex](https://go.microsoft.com/fwlink/?LinkId=87843) 上的旧示例。  

  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2017 的各版本和支持的功能](~/sql-server/editions-and-components-of-sql-server-2017.md)   
 [计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)   
 [高可用性解决方案 (SQL Server)](../sql-server-business-continuity-dr.md)   
 [使用安装向导升级 SQL Server（安装程序）](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
