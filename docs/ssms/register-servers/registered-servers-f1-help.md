---
description: “已注册的服务器”组件的 F1 帮助
title: “已注册的服务器”组件的 F1 帮助
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], Help
- Registered Servers [SQL Server], help
- SQL Server Management Studio Help [SQL Server], registered servers
ms.assetid: 59f76b28-ba78-4a1a-b5d5-8b581f30114d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d4b3a2e75a00e3688ec53c5e64e6ab7727375a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341046"
---
# <a name="registered-servers-f1-help"></a>“已注册的服务器”组件的 F1 帮助
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  本节包含 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中“已注册的服务器”组件的 F1 帮助。 它介绍了各种选项。
  
 若要了解“已注册的服务器”并获取指向如何处理它们的链接，请转到 [注册服务器](./register-servers.md) 主题。 
 

 单击此项可保存已注册服务器的设置。 
 
 ## <a name="reporting-services-new-or-edit-server-registration-general-tab"></a>Reporting Services 新建或编辑服务器注册（“常规”选项卡） 
  在注册 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例时，使用此选项卡可以指定选项。  
  
 若要访问此页，请在已注册的服务器中，在“已注册的服务器”工具栏上单击“Reporting Services”，右键单击任意已注册的服务器组（如“Reporting Services”），指向“新建”，再单击“服务器注册”。  
  
### <a name="options"></a>选项  
 **服务器类型**  
 从已注册的服务器中注册某服务器时，“服务器类型”框是只读的，它与“已注册的服务器”窗格中显示的服务器类型相匹配。 若要注册其他类型的服务器，请在开始注册新服务器之前，在 **“已注册的服务器”** 工具栏上单击所需的服务器。  
  
 服务器名称  
 指定要连接到的报表服务器实例。 在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中，可以通过报表服务器的实例名访问该服务器。 您安装的每个 SQL Server 实例都可以有一个报表服务器实例。 如果使用默认实例，请键入 SQL Server 实例的名称。 如果使用命名实例，请以 MSSQL$InstanceName 格式指定要连接到报表服务器的命名实例。  
  
 **身份验证**  
 连接到报表服务器时进行的身份验证是通过 Internet 信息服务 (IIS) 完成的。 请从以下身份验证模式中选择一个，以便在连接到 Reporting Services 时使用：  
  
 **Windows 身份验证模式（Windows 身份验证）**  
 使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据连接到报表服务器实例。  
  
 **基本身份验证**  
 如果将 Reporting Services 安装配置为使用基本身份验证，则使用“基本身份验证”进行连接。  
  
 **窗体身份验证**  
 如果将 Reporting Services 安装配置为使用自定义身份验证扩展插件，则使用“窗体身份验证”进行连接。  
  
 **用户名**  
 输入用于连接的登录名。 只有在选择了 **“基本身份验证”** 或 **“窗体身份验证”** 时，此选项才可用。  
  
 **密码**  
 输入用户名的密码。 只有在选择了 **“基本身份验证”** 或 **“窗体身份验证”** 时，此选项才可编辑。  
  
 **记住密码**  
 存储已经输入的密码。 只有在单击了 **“基本身份验证”** 或 **“窗体身份验证”** 时，此选项才可用。  
  
> **注意：** 如果已经存储了此密码，而现在要放弃存储，请清除此复选框，再单击“保存”。  
  
 **已注册的服务器名称**  
 希望在“已注册的服务器”中显示的名称。 此名称不必与 **“服务器名称”** 框中的名称相匹配。  
  
 **已注册的服务器说明**  
 输入服务器的说明（可选）。  
  
 **Test**  
 单击此项可测试与“服务器名称”  中所选服务器的连接。  
  
 
 ## <a name="analysis-services---multidimensional-data-new-or-edit-server-registration-general-tab"></a>Analysis Services – 多维数据新建或编辑服务器注册（“常规”选项卡）
 
  在注册 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例时，使用此选项卡可以指定选项。  
  
 若要访问此页，请在“已注册的服务器”中，在“已注册的服务器”工具栏上单击 **Analysis Services**，右键单击任意已注册的服务器组（例如 **Analysis Services**），指向“新建”，再单击“服务器注册”。  
  
### <a name="options"></a>选项  
 **服务器类型**  
 从“已注册的服务器”中注册某服务器时，“服务器类型”框是只读的，它与“已注册的服务器”窗格中显示的服务器类型相匹配。 若要注册其他类型的服务器，请在开始注册新服务器之前，在 **“已注册的服务器”** 工具栏上单击所需的服务器。  
  
 **服务器名称**  
 选择要连接到的服务器实例。 默认情况下，显示上次连接到的服务器实例。  
  
 **身份验证**  
 通过 Windows 身份验证，用户可以使用其 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据以 Windows 用户或 Windows 组成员的身份进行连接。  
  
 **用户名**  
 此选项在此版本中不可用。  
  
 **密码**  
 此选项在此版本中不可用。  
  
 **记住密码**  
 此选项在此版本中不可用。  
  
 **已注册的服务器名称**  
 希望在“已注册的服务器”中显示的名称。 此名称不必与 **“服务器名称”** 框中的内容匹配。  
  
 **已注册的服务器说明**  
 输入服务器的说明（可选）。  
  
 **Test**  
 单击此项可测试与“服务器名称”  中所选服务器的连接。 
 
 ## <a name="ssis-new-or-edit-server-registration-general-tab"></a>SSIS 新建或编辑服务器注册（“常规”选项卡） 
 
 使用此选项卡可以指定注册 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]时的选项。  
  
 若要访问此页，请在“已注册的服务器”中，在“已注册的服务器”工具栏上单击“Integration Services”，右键单击任意已注册的服务器组，指向“新建”，然后单击“服务器注册”。  
  
### <a name="options"></a>选项  
 **服务器类型**  
 在“已注册的服务器”中注册服务器时，“服务器类型”框是只读的，它与“已注册的服务器”窗格中显示的服务器类型相匹配。 若要注册其他类型的服务器，请在开始注册新服务器前，先依次单击“已注册服务器”工具栏上的“数据库引擎”、“Analysis Server”、“Reporting Services”、“SQL Server Compact Edition” 或“Integration Services”。  
  
 服务器名称  
 选择要连接的服务器。 默认情况下，显示上次连接的服务器。  
  
 **身份验证**  
 Windows 身份验证模式允许用户通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户帐户进行连接。  
  
 **用户名**  
 此选项在此版本中不可用。  
  
 **密码**  
 此选项在此版本中不可用。  
  
 **记住密码**  
 此选项在此版本中不可用。  
  
 **已注册的服务器名称**  
 希望在“已注册的服务器”中显示的名称。 此名称不必与 **“服务器名称”** 框中的内容匹配。  
  
 **已注册的服务器说明**  
 输入服务器的说明（可选）。  
  
 **Test**  
 单击此项可测试与“服务器名称”  中所选服务器的连接。 
  

 
 
