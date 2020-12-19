---
description: Azure 订阅连接管理器
title: Azure 订阅连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpsubscrconn.f1
- sql14.dts.designer.afpsubscrconn.f1
ms.assetid: e1225327-c308-4c50-8f44-c411f52ef378
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 277e92a906acb89960e16c941fbd71df023ddb23
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678992"
---
# <a name="azure-subscription-connection-manager"></a>Azure 订阅连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  通过使用你为以下属性指定的值， **Azure 订阅连接管理器** 使 SSIS 包能够连接到 Azure 订阅：Azure 订阅 ID 和管理证书。  
  
 Azure 订阅连接管理器是[用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组件。
  
1.  在如上所示的“添加 SSIS 连接管理器”  对话框中，选择“Azure 订阅” ，然后单击“添加” 。  你应该会看到  “Azure 订阅连接管理器编辑器”对话框。  
  
    ![显示“Azure 订阅连接管理器编辑器”对话框的屏幕截图。](../../integration-services/connection-manager/media/ssis-azuresubscriptionconnectionmanager.png)
  
2.  为“Azure 订阅 ID” 输入你的 Azure 订阅 ID，它可以唯一标识 Azure 订阅。  可以在“设置”  页下的 [Azure 管理门户](https://manage.windowsazure.com) 上找到这些值：  
  
    ![Azure 管理门户的屏幕截图，其中显示“设置”页面的“订阅”选项卡。](../../integration-services/connection-manager/media/ssis-azuresettings-subscriptionid.png "SSIS-AzureSettings-SubscriptionID")  
  
3.  从下拉列表中选择“管理证书存储位置”和“管理证书存储名称”。  
  
4.  输入“管理证书指纹”或单击“浏览…”，从所选存储中选择一个证书。 必须将证书作为订阅的管理证书上载。 为此，请在以下 Azure 门户页上单击“上传”（请参阅这篇 [MSDN 帖子](/previous-versions/azure/gg551722(v=azure.100))以了解详细信息）。  
  
     ![Azure 管理门户的屏幕截图，其中显示“设置”页面的“管理证书”选项卡。](../../integration-services/connection-manager/media/ssis-azuresettings-managementcertificate.png "SSIS-AzureSettings-ManagementCertificate")  
  
5.  单击“测试连接”  以测试连接。  
  
6.  单击 **“确定”** 关闭对话框。  
  
