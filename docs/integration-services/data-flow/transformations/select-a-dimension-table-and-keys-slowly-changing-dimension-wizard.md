---
description: 选择维度表和键（渐变维度向导）
title: 选择维度表和键（渐变维度向导）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.loaddimwizard.selecttableandkeys.f1
ms.assetid: 01e0495f-de35-4607-ba19-0539e801e8fd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5d49d23a670ae5de5b0084acb90758201cbeff74
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425749"
---
# <a name="select-a-dimension-table-and-keys-slowly-changing-dimension-wizard"></a>选择维度表和键（渐变维度向导）

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“选择维度表和键”** 页可以选择要加载的维度表。 将数据流中的列映射到要加载的列。  
  
 若要了解有关此向导的详细信息，请参阅 [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md)。  
  
## <a name="options"></a>选项  
 **“ODBC 目标编辑器”**  
 从列表中选择现有 OLE DB 连接管理器，或者单击“新建”创建 OLE DB 连接管理器。  
  
> [!NOTE]  
>  渐变维度向导仅支持 OLE DB 连接管理器，并且仅支持与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的连接。  
  
 **新建**  
 使用“配置 OLE DB 连接管理器”对话框可以选择现有的连接管理器，或者单击“新建”创建新的 OLE DB 连接。  
  
 **“表或视图”**  
 从该列表中选择表或视图。  
  
 **输入列**  
 可以从输入列列表中选择要为其指定映射的输入列。  
  
 **维度列**  
 查看所有可用的维度列。  
  
 **键类型**  
 选择一个维度列作为业务键。 您必须具有一个业务键。  
  
## <a name="see-also"></a>另请参阅  
 [使用渐变维度向导配置输出](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
