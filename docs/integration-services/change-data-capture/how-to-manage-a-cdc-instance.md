---
description: How to Manage a CDC Instance
title: 如何管理 CDC 实例 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 5d9e677f-b872-497d-9cde-472184a214ab
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c4c09c590d92d97ea56c795db6fe8676d5771277
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88426049"
---
# <a name="how-to-manage-a-cdc-instance"></a>How to Manage a CDC Instance

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本过程介绍如何使用 CDC 设计器控制台在运行时管理 CDC 实例操作。  
  
### <a name="to-manage-cdc-instance-operations"></a>管理 CDC 实例操作  
  
1.  从 **“开始”** 菜单上，选择 **“CDC 设计器控制台”**。  
  
2.  在左侧的窗格中，展开 **“变更数据捕获”** ，然后展开包含您要查看的实例的服务。  
  
3.  选择要使用的实例的名称。  
  
4.  从 CDC 设计器控制台右侧的 **“操作”** 窗格中，单击要执行的操作。  
  
     您还可以在左侧窗格中右键单击实例的名称，然后选择要执行的操作。  
  
     您可以执行以下任务：  
  
    -   **开始**：开始捕获更改。  
  
    -   **停止**：停止捕获更改。  
  
    -   **重置**：单击“重置”可将 CDC 实例重置为其初始（空）状态。 此选项在 CDC 实例停止后可用。 更改表中的所有更改以及 CDC 实例内部状态将被删除。 当 CDC 实例在以后启动时，变更捕获将从该时间点开始，并且仅包含在 CDC 实例启动后开始的事务。  
  
    -   **删除**：删除 CDC 实例。  
  
    -   **Oracle 日志记录脚本**：单击“Oracle 日志记录脚本”将显示具有 Oracle 补充日志记录脚本的“Oracle 日志记录脚本”对话框。 有关可以在此对话框中执行的操作的信息，请参阅 [Oracle Supplemental Logging Script](../../integration-services/change-data-capture/oracle-supplemental-logging-script.md)。  
  
         **注意**：在您运行补充日志记录脚本时，“用于运行脚本的 Oracle 凭据”对话框将打开，您可以在其中提供有效的 Oracle 用户名和密码。 有关如何提供适当的 Oracle 凭据的信息，请参阅 [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md)。  
  
    -   **CDC 实例部署**：为 CDC 实例生成部署脚本。 有关此对话框的信息，请参阅 [CDC Instance Deployment Script](../../integration-services/change-data-capture/cdc-instance-deployment-script.md)。  
  
     有关这些任务的详细信息，请参阅 [Manage a CDC Instance](../../integration-services/change-data-capture/manage-a-cdc-instance.md)。  
  
 您还可以选择 **“属性”** 来编辑 CDC 实例的配置属性。 有关编辑 CDC 实例属性的详细信息，请参阅 [Edit Instance Properties](../../integration-services/change-data-capture/edit-instance-properties.md)。  
  
  
