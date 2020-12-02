---
description: Azure HDInsight 连接管理器
title: Azure HDInsight 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPHDICM.F1
- SQL14.DTS.DESIGNER.AFPHDICM.F1
ms.assetid: 29d01bd9-8b38-43b1-b937-67f8aea57c0f
author: Lingxi-Li
ms.author: lingxl
ms.openlocfilehash: 6acb9fec83da610246001df5bc68591ecab1a225
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130147"
---
# <a name="azure-hdinsight-connection-manager"></a>Azure HDInsight 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Azure HDInsight 连接管理器可实现 SSIS 包与 Azure HDInsight 群集的连接。

Azure HDInsight 连接管理器是[适用 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组件。

若要创建和配置 Azure HDInsight 连接管理器，请执行以下步骤：

1. 在“添加 SSIS 连接管理器” 对话框中，选择“AzureHDInsight”，然后单击“添加” 。
2. 在“Azure HDInsight 连接管理器编辑器”对话框中，为要连接到的 HDInsight 群集指定群集 DNS 名称（不带协议前缀）、用户名和密码。
3. 单击“确定”  关闭对话框。
4. 你可以看到你在“属性”  窗口中创建的连接管理器的属性。
