---
title: SQL Server 属性（“登录”选项卡）
description: 了解 SQL Server“属性”对话框的“登录”选项卡。 使用此选项卡来指定 SQL Server 使用的帐户以及开始或停止服务。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 405073fc-eaa3-43c6-bf82-2cd58cacc1c3
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 63e71e0a0e4c53b77b3c1f11225f49abb250c585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345984"
---
# <a name="sql-server-properties-log-on-tab"></a>SQL Server 属性（“登录”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用 **“SQL Server 属性”** 对话框中的 **“登录”** 选项卡，可以指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务使用的帐户，更改帐户密码，还可以启动和停止该服务。 对帐户密码的更改立即生效。  
  
> [!NOTE]  
>  更改群集实例的服务使用的帐户名时，新帐户必须是安装期间为待更改服务指定的域组的成员，或者您必须具有向该组添加成员的权限。 如果您无权修改组成员身份，请与域管理员联系。  
>   
>  有关选择帐户以运行服务的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机帮助中的“设置 Windows 服务帐户”。  
  
## <a name="options"></a>选项  
 **内置帐户**  
 **Local System**  
 -   指定本地系统帐户。 该帐户不需要密码。 但是，本地系统帐户可能会阻止该服务与其他服务器进行交互，具体取决于该帐户所拥有的权限。  
  
 **Local Service**  
 -   指定本地服务帐户。 该帐户不需要密码。 不过，本地服务帐户可能会阻止该服务与其他服务器进行交互，具体取决于该帐户所拥有的权限。  
  
 **Network Service**  
 -   建议不要将网络服务帐户用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务。 Local User 帐户或 Domain User 帐户更适用于这些服务。  
  
 **本帐户**  
 指定一个使用 Windows 身份验证的本地用户帐户或域用户帐户。 建议使用具有最低服务权限的域用户帐户。  
  
 **帐户名**  
 指定本地用户帐户名或域用户帐户名。  
  
 **密码**  
 键入帐户的密码。  
  
 **确认密码**  
 再次键入帐户的密码。  
  
 **启动**  
 启动服务。  
  
 **Stop**  
 停止服务。  
  
 **暂停**  
 暂停服务。  
  
 **恢复**  
 恢复暂停的服务。  
  
> [!IMPORTANT]  
>  默认情况下，只有本地管理员组的成员能够启动、停止、暂停、继续或重新启动服务。 若要向管理员之外的用户授予管理服务的权限，请参阅 [如何授予用户管理 Windows Server 2003 中的服务的权限](https://support.microsoft.com/kb/325349)。 （此过程在其他 Windows 版本上是类似的。）  
  
> [!NOTE]  
>  启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，如果出现包含短语“未实现 [0x80004001]”的 WMI 错误，则可能指示目标计算机上未安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
  
