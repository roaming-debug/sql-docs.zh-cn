---
description: Microsoft Connector for SAP BW
title: Microsoft Connector for SAP BW | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 5281f080-53d5-4679-aa26-f4cd4ac7a2df
author: chugugrace
ms.author: chugu
ms.openlocfilehash: af1b8c775991581fadbb25bbf62049194cdc74be
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384886"
---
# <a name="microsoft-connector-for-sap-bw"></a>Microsoft Connector for SAP BW

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 由一组三个组件构成，可用于从 SAP Netweaver BW 版本 7 系统中提取数据以及向该系统中加载数据。  
  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW for SQL Server 2016 是 SQL Server 2016 功能包的一个组件。 若要安装 Connector for SAP BW 及其文档，请从 [SQL Server 2016 功能包网页](https://www.microsoft.com/download/details.aspx?id=56833)下载并运行安装程序。  

> [!IMPORTANT]
> Microsoft 预计不会提供 SAP BW 连接器的更新版本。 Microsoft 未拥有由第三方开发的 SAP BW 组件的源代码，因此无法更新它们。 考虑从 Microsoft ISV 合作伙伴（例如 [Theobald 软件](https://theobald-software.com/en/xtract-is-productinfo.html)）购买最新的 SAP 连接组件。 Microsoft 的 ISV 合作伙伴已为 SSIS 调整了其 SAP 连接组件，以便在 Azure 中安装。
 
> [!IMPORTANT]  
>  针对 Microsoft Connector for SAP BW 的文档假定你熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
> [!IMPORTANT]  
>  从 SAP Netweaver BW 提取数据要求额外的 SAP 许可。 请向 SAP 核实以便确认这些要求。  
  
## <a name="components"></a>组件  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 包含以下组件：  
  
-   **SAP BW 源** - SAP BW 源是一个数据流源组件，用于从 SAP Netweaver BW 版本 7 系统中提取数据。  
  
-   **SAP BW 目标** - SAP BW 目标是一个数据流目标组件，用于将数据加载到 SAP Netweaver BW 版本 7 系统中。  
  
-   **SAP BW 连接管理器** - SAP BW 连接管理器将 SAP BW 源或 SAP BW 目标连接到 SAP Netweaver BW 版本 7 系统。  
  
 有关演示如何配置和使用 SAP BW 连接管理器、源和目标的演练，请参阅白皮书 [将 SQL Server Integration Services 与 SAP BI 7.0 一起使用](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100))。 此白皮书还说明如何在 SAP BW 中配置所需的对象。  
  
## <a name="documentation"></a>文档  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 的此帮助文件包含以下主题和部分：  
  
 [安装 Microsoft Connector for SAP BW](../integration-services/installing-the-microsoft-connector-for-sap-bw.md)  
 介绍了 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 的安装要求。  
  
 [Microsoft Connector for SAP BW 组件](../integration-services/microsoft-connector-for-sap-bw-components.md)  
 介绍 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 的各组件。  
  
 [Microsoft Connector for SAP BW F1 帮助](../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
 介绍 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 的各组件的用户界面。  
  
