---
description: “创建 InfoSource”
title: 创建 InfoSource | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e7db233b-5464-43de-9d26-6dd24c7ac1b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c4672ce48890af7202445d283d15c2c167550f5a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127249"
---
# <a name="create-infosource"></a>“创建 InfoSource”

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“创建 InfoSource”** 对话框创建一个新 InfoSource。 要创建新 InfoSource，请使用 **“创建事务数据的 InfoSource”** 或 **“创建主数据的 InfoSource”** 对话框。  
  
 从 **“SAP BW 目标编辑器”** 的 **“连接管理器”** 页可以打开 **“创建 InfoSource”** 对话框。 若要了解有关 SAP BW 目标的详细信息，请参阅 [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“创建 InfoSource”对话框**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 SAP BW 目标。  
  
3.  在 **“SAP BW 目标编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
4.  在 **“连接管理器”** 页的 **“创建 SAP BW 对象”** 组框中，选择 **“InfoSource”**，然后单击 **“创建”**。  
  
## <a name="options"></a>选项  
 **事务数据**  
 创建事务数据的新 InfoSource。  
  
 如果您选择此选项，将打开 **“创建事务数据的 InfoSource”** 对话框。 您可使用 **“创建事务数据的 InfoSource”** 对话框来创建新的 InfoSource。 有关此对话框的详细信息，请参阅 [Create InfoSource for Transaction Data](../../integration-services/data-flow/create-infosource-for-transaction-data.md)。  
  
 **主数据**  
 创建主数据的新 InfoSource。  
  
 如果您选择此选项，将打开 **“创建主数据的 InfoSource”** 对话框。 您可使用 **“创建主数据的 InfoSource”** 对话框来创建新的 InfoSource。 有关此对话框的详细信息，请参阅 [Create InfoSource for Master Data](../../integration-services/data-flow/create-infosource-for-master-data.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
