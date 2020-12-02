---
description: “选择源”对话框
title: “选择源”对话框 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.selectsource.f1
ms.assetid: d664c2e5-dd0c-4da8-b27d-aa4ee4fc0ffd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6ea25aebc5368295f13105ecc58d18e86bfc6071
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88493970"
---
# <a name="select-source-dialog-box"></a>“选择源”对话框
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  此对话框用于选择要运行的策略的来源。 若要选择一个或多个包含策略的 XML 文件，请选择 **“文件”**。 若要运行位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上的策略，请选择 **“服务器”**。  
  
 打开此对话框有以下几种不同的方法。  
  
 **打开此对话框**  
  
-   在“已注册的服务器”中，右键单击“本地服务器组”或“本地服务器组”下面的任一服务器，或右键单击“中央管理服务器”下面的任一服务器，然后选择“评估策略”。 在“评估策略”对话框的“策略选择”页上单击“浏览”(**...**) 按钮。  
  
-   在对象资源管理器中，依次展开“管理”和“策略管理”，右键单击“策略”，然后选择“导入策略”。 在“导入”对话框中，单击“浏览”(**...**) 按钮。  
  
-   在对象资源管理器中，右键单击某个服务器、数据库或数据库对象，然后依次选择“策略”和“评估”。 在“评估策略”对话框的“策略选择”页上单击“浏览”(**...**) 按钮。  
  
## <a name="options"></a>选项  
 **文件**  
 选择一个或多个包含策略的 XML 文件。  
  
 **Server**  
 用于选择包含要运行的策略的服务器。  
  
 **服务器类型**  
 仅 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 服务器包含策略。 此框是只读的。  
  
 **服务器名称**  
 选择要连接到的服务器实例。 默认情况下，显示上次连接到的服务器实例。  
  
 **身份验证**  
 在连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例时，可以使用两种身份验证模式。  
  
 **Windows 身份验证模式（Windows 身份验证）**  
 Windows 身份验证模式允许用户通过 Windows 用户帐户进行连接。  
  
 **SQL Server 身份验证**  
 当用户使用指定的登录名和密码通过不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将自行进行身份验证，即检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码相匹配。 如果未设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户，则身份验证失败，并且用户会收到一条错误消息。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。  
  
 **用户名**  
 输入连接所使用的用户名。 仅当已选择使用 Windows 身份验证进行连接时，才能使用此选项。  
  
 **登录**  
 输入连接所用的登录名。 仅当已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接时，才能使用此选项。  
  
 **密码**  
 输入登录名的密码。 仅当已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接时，才能编辑此选项。  
  
## <a name="see-also"></a>另请参阅  
 [策略管理节点（对象资源管理器）](../../relational-databases/policy-based-management/policy-management-node-object-explorer.md)   
 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
