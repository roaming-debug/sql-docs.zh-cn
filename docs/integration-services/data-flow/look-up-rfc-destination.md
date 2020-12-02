---
description: 查找 RFC 目标
title: 查找 RFC 目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: db9404d8-4c42-45e5-a100-c7a84b056109
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d938b3d742dd9d4b43f0a9b3704281392d89049d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495797"
---
# <a name="look-up-rfc-destination"></a>查找 RFC 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“查找 RFC 目标”** 对话框查找在 SAP Netweaver BW 系统中定义的 RFC 目标。 当可用 RFC 目标列表显示时，选择您需要的目标，然后组件将使用需要的值填充关联的选项。  
  
 SAP BW 源和 SAP BW 目标都使用 **“查找 RFC 目标”** 对话框。 有关这些 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 组件的详细信息，请参阅 [SAP BW 源](../../integration-services/data-flow/sap-bw-source.md) 和 [SAP BW 目标](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“查找 RFC 目标”对话框**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 源或目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”选项卡上，双击 SAP BW 源或目标。  
  
3.  在 **“SAP BW 源编辑器”** 或 **SAP BW 目标编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
4.  在 **“连接管理器”** 页上的 **“RFC 目标”** 组框中，单击 **“查找”** 显示 **“查找 RFC 目标”** 对话框。  
  
     在“SAP BW 源编辑器”中，仅当“执行模式”为“P - 触发进程链”或“W - 等待通知”时，“RFC 目标”组框才会显示。  
  
## <a name="options"></a>选项  
 **目标**  
 查看在 SAP Netweaver BW 系统中定义的 RFC 目标的名称。  
  
 **网关主机**  
 查看网关主机的服务器名称或 IP 地址。 通常，名称或 IP 地址与 SAP 应用程序服务器相同。  
  
 **网关服务**  
 查看网关服务的名称，格式为 **sapgwNN**，其中 **NN** 是系统编号。  
  
 **程序 ID**  
 查看与 RFC 目标关联的程序 ID。  
  
## <a name="see-also"></a>另请参阅  
 [SAP BW 源编辑器（“连接管理器”页）](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)   
 [SAP BW 目标编辑器（“连接管理器”页）](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
