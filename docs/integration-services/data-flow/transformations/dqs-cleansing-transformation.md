---
description: DQS 清除转换
title: DQS 清除转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssdqs.designer.cleansing.f1
- sql13.SSDQS.DESIGNER.DQCONNECTION.F1
helpviewer_keywords:
- data correction
- correct data
ms.assetid: d2ec1b1a-c745-4741-b57c-6fdb524a154c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ce6e3f36d8216f08493933798cdab558274e0cb5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123383"
---
# <a name="dqs-cleansing-transformation"></a>DQS 清除转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  DQS 清除转换使用 Data Quality Services (DQS)，通过应用为连接的数据源或类似数据源创建的已批准规则来更正来自连接的数据源的数据。 有关数据更正规则的详细信息，请参阅 [DQS Knowledge Bases and Domains](../../../data-quality-services/dqs-knowledge-bases-and-domains.md)。 有关 DQS 的详细信息，请参阅 [Data Quality Services Concepts](../../../data-quality-services/data-quality-services-concepts.md)。  
  
 为了确定是否必须更正数据，DQS 清除转换在满足以下条件时处理输入列中的数据：  
  
-   该列已选定用于数据更正。  
  
-   数据更正支持该列数据类型。  
  
-   该列映射到的域具有兼容数据类型。  
  
 该转换还包括您配置为处理行级错误的错误输出。 若要配置错误输出，请使用 **“DQS 清理转换编辑器”**。  
  
 您可以在数据流中包含 [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md) 来标识可能为重复项的数据行。  
  
## <a name="data-quality-projects-and-values"></a>数据质量项目和值  
 使用 DQS 清理转换处理数据时，会在数据质量服务器上创建一个清理项目。 使用数据质量客户端管理项目。 此外，您还可以使用数据质量客户端将项目值导入 DQS 知识库域。 只能将值导入配置为使用 DQS 清理转换的域（或链接域）。  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [在数据质量客户端中打开 Integration Services 项目](../../../data-quality-services/open-integration-services-projects-in-data-quality-client.md)  
  
-   [将清理项目值导入到域中](../../../data-quality-services/import-cleansing-project-values-into-a-domain.md)  
  
-   [将数据质量规则应用于数据源](../../../integration-services/data-flow/transformations/apply-data-quality-rules-to-data-source.md)  
  
## <a name="related-content"></a>相关内容  
  
-   [打开、解锁、重命名和删除数据质量项目](../../../data-quality-services/open-unlock-rename-and-delete-a-data-quality-project.md)  
  
-   social.technet.microsoft.com 上的文章： [使用复合域清理复杂数据](https://social.technet.microsoft.com/wiki/contents/articles/13324.using-dqs-cleansing-complex-data-using-composite-domains.aspx)。  
  
## <a name="dqs-cleansing-transformation-editor-dialog-box"></a>“DQS 清除转换编辑器”对话框
  可使用 Data Quality Services (DQS) 通过“DQS 清除转换编辑器”对话框来更正数据。 有关详细信息，请参阅 [Data Quality Services Concepts](../../../data-quality-services/data-quality-services-concepts.md)。  
  
 **您希望做什么？**  
  
-   [打开 DQS 清除转换编辑器](#open)  
  
-   [设置“连接管理器”选项卡上的选项](#connection)  
  
-   [设置“映射”选项卡上的选项](#mapping)  
  
-   [设置“高级”选项卡上的选项](#advanced)  
  
-   [设置“DQS 清除连接管理器”对话框中的选项](#manager)  
  
###  <a name="open-the-dqs-cleansing-transformation-editor"></a><a name="open"></a> 打开 DQS 清除转换编辑器  
  
1.  在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中，将“DQS 清除转换”添加到 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]包。  
  
2.  右键单击该组件，然后单击 **“编辑”**。  
  
###  <a name="set-options-on-the-connection-manager-tab"></a><a name="connection"></a> 设置“连接管理器”选项卡上的选项  
 **数据质量连接管理器**  
 从列表中选择现有 DQS 连接管理器，或单击“新建”创建一个新连接。  
  
 **新建**  
 使用“DQS 清除连接管理器”对话框创建新的连接管理器。 请参阅 [设置“DQS 清除连接管理器”对话框中的选项](#manager)  
  
 **数据质量知识库**  
 为所连接的数据源选择现有的 DQS 知识库。 有关 DQS 知识库的详细信息，请参阅 [DQS Knowledge Bases and Domains](../../../data-quality-services/dqs-knowledge-bases-and-domains.md)。  
  
 **加密连接**  
 指定是否对连接加密，以便对 DQS 服务器与 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 之间的数据传输进行加密。  
  
 **可用域**  
 列出可用于选定知识库的域。 存在两种类型的域：单一域和包含两个或两个以上的单一域的复合域。  
  
 有关如何将列映射到复合域的信息，请参阅 [Map Columns to Composite Domains](../../../integration-services/data-flow/transformations/map-columns-to-composite-domains.md)。  
  
 有关域的详细信息，请参阅 [DQS Knowledge Bases and Domains](../../../data-quality-services/dqs-knowledge-bases-and-domains.md)。  
  
 **配置错误输出**  
 指定如何处理行级错误。 在转换更正来自已连接数据源的数据时可能会由于意外的数据值或验证约束而出错。  
  
 有效值如下：  
  
-   **“组件失败”**，指示转换失败并且输入数据未插入 Data Quality Services 数据库中。 这是默认值。  
  
-   **“重定向行”**，指示输入数据未插入 Data Quality Services 数据库中并且重定向到错误输出。  
  
###  <a name="set-options-on-the-mapping-tab"></a><a name="mapping"></a> 设置“映射”选项卡上的选项  
 有关如何将列映射到复合域的信息，请参阅 [Map Columns to Composite Domains](../../../integration-services/data-flow/transformations/map-columns-to-composite-domains.md)。  
  
 **可用输入列**  
 列出所连接数据源中的列。 选择包含要更正的数据的一个或多个列。  
  
 **输入列**  
 列出你在“可用输入列”区域中选定的输入列。  
  
 **Domain**  
 选择要映射到输入列的域。  
  
 **源别名**  
 列出包含原始列值的源列。  
  
 在该字段中单击可修改列名。  
  
 **输出别名**  
 列出“DQS 清除转换”输出的列。 该列包含原始列值或更正的值。  
  
 在该字段中单击可修改列名。  
  
 **状态别名**  
 列出包含已更正数据的状态信息的列。 在该字段中单击可修改列名。  
  
###  <a name="set-options-on-the-advanced-tab"></a><a name="advanced"></a> 设置“高级”选项卡上的选项  
 **标准化输出**  
 指示是否根据为域定义的输出格式以标准化格式输出数据。 有关标准化格式的详细信息，请参阅 [数据清理](../../../data-quality-services/data-cleansing.md)。  
  
 **Confidence**  
 指示是否包括已更正数据的置信度。 置信度指示 DQS 对更正或建议的确信程度。 有关置信度的详细信息，请参阅 [数据清理](../../../data-quality-services/data-cleansing.md)。  
  
 **原因**  
 指示是否包括数据更正的原因。  
  
 **追加的数据**  
 指示是否输出从现有的引用数据访问接口接收的附加数据。 有关详细信息，请参阅 [Reference Data Services in DQS](../../../data-quality-services/reference-data-services-in-dqs.md)。  
  
 **追加的数据架构**  
 指示是否输出数据架构。 有关详细信息，请参阅 [将域或复合域附加到引用数据](../../../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md)。  
  
###  <a name="set-the-options-in-the-dqs-cleansing-connection-manager-dialog-box"></a><a name="manager"></a> 设置“DQS 清除连接管理器”对话框中的选项  
 服务器名称  
 选择或键入要连接的 DQS 服务器的名称。 有关该服务器的详细信息，请参阅 [DQS Administration](../../../data-quality-services/dqs-administration.md)。  
  
 **测试连接**  
 单击该选项可以确认您指定的连接是否有效。  
  
 您还可以通过执行以下操作从连接区域打开 **“DQS 清除连接管理器”** 对话框：  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开现有的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目或者创建一个新项目。  
  
2.  在连接区域中单击右键，依次单击“新建连接”和“DQS”。  
  
3.  单击“添加”  。  
  
