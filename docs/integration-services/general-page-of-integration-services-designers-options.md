---
description: Integration Services 设计器选项的“常规”页
title: Integration Services 设计器选项的“常规”页 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Business_Intelligence_Designers.Data_Transformation_Designers.General
ms.assetid: d695690a-923b-4036-945e-7621e8651deb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 76be801b071c85d26b8b18ed23e2c478641fa0cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347804"
---
# <a name="general-page-of-integration-services-designers-options"></a>Integration Services 设计器选项的“常规”页

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“选项”** 对话框中 **“Integration Services 设计器”** 页的 **“常规”** 页，可以指定用于加载、显示和升级包的选项。  
  
 若要打开 **“常规”** 页，请在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，单击 **“工具”** 菜单上的 **“选项”**，展开 **“商业智能设计器”**，然后选择 **“Integration Services 设计器”**。  
  
## <a name="options"></a>选项  
 **加载包时检查数字签名**  
 选择让 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 在加载包时检查数字签名。 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 仅会检查数字签名是否存在、是否有效，并且是否来自可靠来源。 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 不会检查包经过签名后是否发生过更改。  
  
 如果你设置 **BlockedSignatureStates** 注册表值，则此注册表值会替代“加载包时检查数字签名”  选项。 有关详细信息，请参阅 [通过设置注册表值实现签名策略](./security/identify-the-source-of-packages-with-digital-signatures.md)。  
  
 有关数字证书和包的详细信息，请参阅 [使用数字签名标识包的源](../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md)。  
  
 **如果包未经签名则显示警告**  
 选择此项将在加载未经签名的包时显示警告。  
  
 **显示优先约束标签**  
 选择在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 中查看包时，显示哪种优先约束标签（“成功”、“失败”或“完成”）。  
  
 **脚本语言**  
 选择新脚本任务和脚本组件的默认脚本语言。  
  
 **更新连接字符串以使用新的提供程序名称**  
 对于当前版本的 [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] ，打开或升级 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]包时，请将连接字符串更新为使用下列提供程序的名称：  
  
-   [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] OLE DB 访问接口  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] 包升级向导仅更新存储在连接管理器中的连接字符串。 向导不会更新通过使用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 表达式语言或在脚本任务中使用代码动态构造的连接字符串。  
  
 **创建新的包 ID**  
 升级 [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] 包时，为包的升级版本创建新的包 ID。  
  
## <a name="see-also"></a>另请参阅  
 [安全性概述 (Integration Services)](../integration-services/security/security-overview-integration-services.md)   
 [用脚本扩展包](../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
  
