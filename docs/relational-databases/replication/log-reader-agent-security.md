---
description: 日志读取器代理安全性
title: 日志读取器代理安全性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.security.LRA.f1
helpviewer_keywords:
- Log Reader Agent Security dialog box
ms.assetid: d6981e74-ddb8-41b8-9ea1-56c2ece63b8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1aa897798a3600ec8bf97f421003d49eefd36c09
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483068"
---
# <a name="log-reader-agent-security"></a>日志读取器代理安全性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  使用“日志读取器代理安全性”对话框可指定以下项：  
  
-   用于在分发服务器上运行日志读取器代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户。 Windows 帐户也称为“进程帐户 ”，因为代理进程是在此帐户下运行。  
  
-   日志读取器代理与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器建立连接时所处的上下文。 通过模拟 Windows 帐户，或在指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户的上下文中可以进行连接。  
  
    > [!NOTE]  
    >  即使发布服务器与分发服务器在同一台计算机上，日志读取器代理也会与发布服务器建立连接。 日志读取器代理还可以与分发服务器建立连接；这些连接始终是通过模拟运行代理的 Windows 帐户来建立的。  
  
     对于 Oracle 发布服务器，请在 **“发布服务器属性”** 对话框（可以通过 **“分发服务器属性”** 对话框访问）中指定日志读取器代理连接发布服务器时所处的上下文。 有关详细信息，请参阅 [View and Modify Replication Security Settings](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 所有帐户必须是有效的，并且为每个帐户指定了正确的密码。 在运行代理之前不会对帐户和密码进行验证。  
  
## <a name="options"></a>选项  
 **进程帐户**  
 输入用于在分发服务器上运行日志读取器代理的 Windows 帐户。 指定的 Windows 帐户必须至少为分发数据库中的 **db_owner** 固定数据库角色的成员。  
  
 **“密码”** 和 **“确认密码”**  
 输入 Windows 帐户的密码。  
  
 **连接到发布服务器**  
 选择日志读取器代理应该通过模拟 **“进程帐户”** 文本框中指定的帐户，还是通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户来建立与发布服务器的连接。 如果选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户，请输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码。  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您选择模拟 Windows 帐户，而不要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户。  
  
 用于连接的 Windows 帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户至少必须是发布数据库中的 **db_owner** 固定数据库角色的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制中的标识和访问控制](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
