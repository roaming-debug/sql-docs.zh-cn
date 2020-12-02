---
description: 第 6-2 课：将项目转换为项目部署模型
title: 步骤 2：将项目转换为项目部署模型 | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 80964293-f1f5-4da7-b1fb-00ab8c30c1c5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fc0e6111542e73ad64a2e82a7f82b3b8bfbf41c4
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88487735"
---
# <a name="lesson-6-2-convert-the-project-to-the-project-deployment-model"></a>第 6-2 课：将项目转换为项目部署模型

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本任务中，使用 Integration Services 项目转换向导将项目转换为项目部署模型。  
  
1.  在“项目”菜单上，选择“转换为项目部署模型”。  
  
2.  在“Integration Services 项目转换向导”的“简介”页面上，查看步骤，然后选择“下一步” 。  
  
3.  在“选择包”页上的“包”列表中，清除所有复选框（除了 Lesson 6.dtsx），然后选择“下一步”。  
  
4.  在“指定项目属性”页上，选择“下一步”。  
  
5.  在“更新执行包任务”页上，选择“下一步”。  
  
6.  在“选择配置”页上，确保在“配置”列表中选择 Lesson 6.dtsx 包，然后选择“下一步”。  
  
7.  在“创建参数”页上，确保选择了 Lesson 6.dtsx 包。  在“配置属性”列表中，验证“范围”是否为“包”，然后选择“下一步”。  
  
8.  在“配置参数”页上，请验证“名称”和“值”的值是否与第 5 课中为变量和配置值指定的值相同，然后选择“下一步”。  
  
9. 在“检查”页上的“摘要”窗格中，请注意向导使用了配置文件中的信息来设置转换的“属性”。  
  
10. 选择“转换”。  
  
    转换完成后，会看到一条警告消息，警告在保存项目后才会保存更改。 选择“确定”，关闭警告对话框。  
  
11. 在“Integration Services 项目转换向导”上，选择“关闭”。  
  
12. 在 SQL Server Data Tools 中，选择“文件”菜单，然后选择“保存”以保存转换的包。  
  
13. 选择“参数”选项卡，验证该包现在是否包含 VarFolderName 的参数。 该参数值与第 5 课的配置文件中为 New Sample Data 文件夹指定的路径相同。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 3：测试第 6 课包](../integration-services/lesson-6-3-testing-the-lesson-6-package.md)  
  
  
  
