---
description: 配置对话框
title: “配置”对话框 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configure.f1
- sql13.SSIS.SSMS.ISPROJECTPROP.REFERENCES.F1
- sql13.SSIS.SSMS.ISPROJECTPROP.PARAMETERS.F1
ms.assetid: 10183c8d-b1be-420f-972a-96ea97d4f4d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0d37b0c08debbd108645c697d53f9136c2a729d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354985"
---
# <a name="configure-dialog-box"></a>配置对话框

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“配置”** 对话框可为包和项目配置参数、连接管理器和对环境的引用。  
  
 **您希望做什么？**  
  
-   [打开“配置”对话框](#open_dialog)  
  
-   [设置“参数”页上的选项](#parameter)  
  
-   [设置“引用”页上的选项](#references)  
  
##  <a name="open-the-configure-dialog-box"></a><a name="open_dialog"></a> 打开“配置”对话框  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
     正在连接到承载 SSISDB 数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例。  
  
2.  在对象资源管理器中，展开树以便显示 **“Integration Services 目录”** 节点。  
  
3.  展开 **“SSISDB”** 节点。  
  
4.  展开包含您要配置的包或项目的文件夹。  
  
5.  右键单击该包或项目，然后单击“配置”。  
  
##  <a name="set-the-options-on-the-parameters-page"></a><a name="parameter"></a> 设置“参数”页上的选项  
 使用 **“参数”** 页查看参数名称和值，然后修改值。  
  
 在“范围”下拉列表中，选择“参数”和“连接管理器”选项卡中显示的参数的范围。  
  
 以下是 **“参数”** 选项卡的选项列表。  
  
 **容器**  
 列出包含参数的对象。  
  
 **名称**  
 列出参数名称。  
  
 **值**  
 列出参数值。 单击省略号按钮可更改 **“设置参数值”** 对话框中的值。  
  
 以下是 **“连接管理器”** 选项卡中的选项列表。使用此选项卡可以更改连接管理器属性的值。 将在 SSIS 服务器上为这些属性自动生成参数。  
  
 **容器**  
 列出包含连接管理器的对象。  
  
 **名称**  
 列出连接管理器名称。  
  
 **属性名称**  
 列出连接管理器属性的名称。  
  
 **值**  
 列出分配给连接管理器属性的值。 单击省略号按钮可更改 **“设置参数值”** 对话框中的值。 您可以输入一个文字值、映射包含您要使用的值的环境变量，或使用包中的默认值。  
  
##  <a name="set-the-options-on-the-references-page"></a><a name="references"></a> 设置“引用”页上的选项  
 使用 **“引用”** 页来添加和删除对环境的引用以及访问环境属性。  
  
 环境可为已部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的项目中包含的包指定运行时值。  
  
 **环境**  
 列出环境。  
  
 **环境文件夹**  
 列出包含环境的文件夹。  
  
 **打开**  
 单击可打开“环境属性”对话框。  
  
 **添加**  
 单击可添加对环境的引用。 在 **“浏览环境”** 对话框中，单击一个环境，然后单击 **“确定”**。  
  
 您可以选择 **“SSISDB”** 节点下的任何项目文件夹中包含的环境。  
  
 **删除**  
 单击“引用”区域中列出的环境，然后单击“删除”。  
  
  
