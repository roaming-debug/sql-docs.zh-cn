---
description: SAP BW 源编辑器（“连接管理器”页）
title: SAP BW 源编辑器（“连接管理器”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwsource.connection.f1
ms.assetid: 2a6dc531-85ca-43c5-a65f-3ad3f7d537c4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b745e73cb7f25ff8936ddc385979c69ad675f8ea
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194741"
---
# <a name="sap-bw-source-editor-connection-manager-page"></a>SAP BW 源编辑器（“连接管理器”页）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“SAP BW 源编辑器”** 的 **“连接管理器”** 页，为 SAP BW 源选择 SAP BW 连接管理器。 在该页中，您还可选择用于从 SAP Netweaver BW 系统提取数据的执行模式和参数。  
  
 若要了解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的 SAP BW 源组件的详细信息，请参阅 [SAP BW 源](../../integration-services/data-flow/sap-bw-source.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
> [!IMPORTANT]  
>  从 SAP Netweaver BW 提取数据要求额外的 SAP 许可。 请向 SAP 核实以便确认这些要求。  
  
 **打开“连接管理器”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 源的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”选项卡上，双击“SAP BW 源”。  
  
3.  在 **“SAP BW 源编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
## <a name="static-options"></a>静态选项  
  
> [!NOTE]  
>  如果您不知道配置源所需的所有值，可能需要询问您的 SAP 管理员。  
  
 **SAP BW 连接管理器**  
 从列表中选择一个现有连接管理器，或通过单击“新建”  创建一个新连接。  
  
 **新建**  
 使用“SAP BW 连接管理器”对话框创建新的连接管理器。  
  
 有关此对话框的详细信息，请参阅 [SAP BW Connection Manager Editor](../connection-manager/sap-bw-connection-manager.md)。  
  
 **OHS 目标**  
 选择用来从源中提取数据的 Open Hub Service (OHS) 目标。  
  
 **执行模式**  
 指定从源提取数据的方法。  
  
|选项|说明|  
|------------|-----------------|  
|**P - 触发进程链**|触发进程链。 此情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包会启动提取进程。|  
|**W - 等待通知**|等待 SAP Netweaver BW 系统发出开始提取数据的通知。 此情况下，SAP Netweaver BW 系统会启动提取进程。|  
|**E - 仅提取**|检索与某个特定请求 ID 关联的数据。 此情况下，SAP Netweaver BW 系统已将数据提取到内部表中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包只需读取这些数据。|  
  
 **预览**  
 打开可在其中预览结果的“预览”对话框。 有关详细信息，请参阅 [Preview](../../integration-services/data-flow/preview.md)。  
  
> [!IMPORTANT]  
>  **“预览”** 选项位于 SAP BW 源编辑器的 **“连接管理器”** 页，实际用来提取数据。 如果您已配置 SAP Netweaver BW 只提取自从上次提取后发生更改的数据，则选择 **“预览”** 将从下次数据提取中排除已经预览过的数据。  
  
 单击 **“预览”** 的同时还会打开 **“请求日志”** 对话框。 您可使用此对话框查看向 SAP Netweaver BW 系统提出样本数据请求的过程中记录的事件。 有关详细信息，请参阅 [Request Log](../../integration-services/data-flow/request-log.md)。  
  
## <a name="execution-mode-dynamic-options"></a>执行模式动态选项  
  
> [!NOTE]  
>  如果您不知道配置源所需的所有值，可能需要询问您的 SAP 管理员。  
  
### <a name="execution-mode--p---trigger-process-chain"></a>执行模式 = P - 触发进程链  
  
#### <a name="rfc-destination-options"></a>RFC 目标选项  
 您无需事先知道并输入这些值。 使用 **“查找”** 按钮查找和选择合适的 RFC 目标。 选定 RFC 目标后，组件会为这些选项输入合适的值。  
  
 **网关主机**  
 输入服务器名称或网关主机的 IP 地址。 通常，名称或 IP 地址与 SAP 应用程序服务器相同。  
  
 **网关服务**  
 输入网关服务的名称，格式为“sapgwNN”，其中 **NN** 是系统编号。  
  
 **程序 ID**  
 输入与 RFC 目标关联的程序 ID。  
  
 **查找**  
 使用“查找 RFC 目标”对话框查找 RFC 目标。 有关此对话框的详细信息，请参阅 [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md)。  
  
#### <a name="process-chain-options"></a>进程链选项  
 您无需事先知道并输入这些值。 使用 **“查找”** 按钮查找和选择合适的进程链。 选定进程链后，组件会为该选项输入合适的值。  
  
 **进程链**  
 输入由源触发的进程链的名称。  
  
 **查找**  
 使用“查找进程链”对话框查找进程链。 有关此对话框的详细信息，请参阅 [Look Up Process Chain](../../integration-services/data-flow/look-up-process-chain.md)。  
  
### <a name="execution-mode--w---wait-for-notify"></a>执行模式 = W - 等待通知  
  
#### <a name="rfc-destination-options"></a>RFC 目标选项  
 您无需事先知道并输入这些值。 使用 **“查找”** 按钮查找和选择合适的 RFC 目标。 选定 RFC 目标后，组件会为该选项输入合适的值。  
  
 **网关主机**  
 输入服务器名称或网关主机的 IP 地址。 通常，名称或 IP 地址与 SAP 应用程序服务器相同。  
  
 **网关服务**  
 输入网关服务的名称，格式为“sapgwNN”，其中 **NN** 是系统编号。  
  
 **程序 ID**  
 输入与 RFC 目标关联的程序 ID。  
  
 **查找**  
 使用“查找 RFC 目标”对话框查找 RFC 目标。 有关此对话框的详细信息，请参阅 [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md)。  
  
### <a name="execution-mode--e---extract-only"></a>执行模式 = E - 仅提取  
 **请求 ID**  
 输入与提取关联的请求 ID。  
  
## <a name="see-also"></a>另请参阅  
 [SAP BW 源编辑器（“列”页）](../../integration-services/data-flow/sap-bw-source-editor-columns-page.md)   
 [SAP BW 源编辑器（“错误输出”页）](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)   
 [SAP BW 源编辑器（“高级”页）](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
