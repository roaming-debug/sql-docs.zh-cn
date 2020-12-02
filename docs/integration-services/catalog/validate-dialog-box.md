---
description: “验证”对话框
title: “验证”对话框 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isprojectvalidate.f1
- sql13.ssis.ssms.ispackagevalidate.f1
ms.assetid: 134e14ce-4f8d-4a20-889a-918014c841d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a65f5d8114907472eb03aa06e428927aa1408b7b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484902"
---
# <a name="validate-dialog-box"></a>“验证”对话框

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“验证”** 对话框检查 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目或包中的常见问题。  
  
 如果有问题，将在该对话框的顶部显示一条消息。 否则，在顶部显示“就绪”一词。  
  
 **您希望做什么？**  
  
-   [打开“验证”对话框](#open_dialog)  
  
-   [设置“常规”页上的选项](#general)  
  
##  <a name="open-the-validate-dialog-box"></a><a name="open_dialog"></a> 打开“验证”对话框  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
     正在连接到承载 SSISDB 数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例。  
  
2.  在对象资源管理器中，展开树以便显示 **“Integration Services 目录”** 节点。  
  
3.  展开 **“SSISDB”** 节点。  
  
4.  展开包含您要验证的项目或包的文件夹。  
  
5.  右键单击该包，然后单击“验证”。  
  
##  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> 设置“常规”页上的选项  
 **环境**  
 选择要用于验证项目或包的环境。  
  
 **32 位运行时**  
 选择使用 32 位运行时来执行包。  
  
 **“参数”** 选项卡列出了用于验证项目或包的参数值。 以下是“参数”选项卡上的选项。  
  
 **容器**  
 列出包含参数的对象。  
  
 **参数**  
 列出参数的名称  
  
 **值**  
 列出参数值。  
  
 **“连接管理器”** 选项卡列出了用于验证项目或包的连接管理器属性值。  
  
 以下是 **“连接管理器”** 选项卡中的选项。  
  
 **容器**  
 列出包含连接管理器的对象。  
  
 **名称**  
 列出连接管理器名称。  
  
 **属性名称**  
 列出连接管理器属性的名称。  
  
 **值**  
 列出分配给连接管理器属性的值。  
  
  
