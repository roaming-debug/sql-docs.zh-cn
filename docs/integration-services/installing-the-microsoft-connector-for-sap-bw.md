---
description: 安装 Microsoft Connector for SAP BW
title: 安装 Microsoft Connector for SAP BW | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3bfb9023-9597-4f59-9085-4b9057e7702e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3244ffbd690d7f2ea79862eb1963df60607ac2d6
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384615"
---
# <a name="installing-the-microsoft-connector-for-sap-bw"></a>安装 Microsoft Connector for SAP BW

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW for SQL Server 2016 是 SQL Server 2016 功能包的一个组件。 若要安装 Connector for SAP BW 及其文档，请从 [SQL Server 2016 功能包网页](https://www.microsoft.com/download/details.aspx?id=56833)下载并运行安装程序。  

> [!IMPORTANT]
> Microsoft 预计不会提供 SAP BW 连接器的更新版本。 Microsoft 未拥有由第三方开发的 SAP BW 组件的源代码，因此无法更新它们。 考虑从 Microsoft ISV 合作伙伴（例如 [Theobald 软件](https://theobald-software.com/en/xtract-is-productinfo.html)）购买最新的 SAP 连接组件。 Microsoft 的 ISV 合作伙伴已为 SSIS 调整了其 SAP 连接组件，以便在 Azure 中安装。

> [!IMPORTANT]  
>  针对 Microsoft Connector for SAP BW 的文档假定你熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
> [!IMPORTANT]  
>  从 SAP Netweaver BW 提取数据要求额外的 SAP 许可。 请向 SAP 核实以便确认这些要求。  
  
## <a name="required-sap-files"></a>必需的 SAP 文件  
 若要使用 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW，你不必在本地计算机上安装 SAP 前端软件 (SAP GUI)。  
  
 但是，必须将 SAP .NET 连接器文件 librfc32.dll 复制到 Windows 文件夹的系统子文件夹中。 （通常，此文件夹位置为 **C:\Windows\system32** 。）  
  
## <a name="considerations-for-64-bit-computers"></a>针对 64 位计算机的注意事项  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 完全支持 [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows 的 64 位版本。 在 64 位计算机上， [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector for SAP BW 具有以下附加要求：  
  
-   若要在任何 64 位 Windows 操作系统上的 64 位模式中运行包，请将 SAP GUI 文件 librfc32.dll 的 64 位版本复制到 Windows 文件夹的 **system32** 文件夹中。 （通常，此文件位置为 **C:\Windows\system32** 。）  
  
-   若要在任何 64 位 Windows 操作系统上的 32 位模式中运行包，请将 SAP GUI 文件 librfc32.dll 复制到 Windows 文件夹的 **SysWow64** 文件夹中。 （通常，此文件夹位置为 **C:\Windows\SysWow64** 。）  
  
  
