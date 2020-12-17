---
title: CLR 集成安全性 |Microsoft Docs
description: 与 .NET Framework CLR 安全 SQL Server 的集成管理对象之间的访问。 对对象执行的安全检查取决于涉及的调用。
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- security [CLR integration]
- authorization [CLR integration]
- common language runtime [SQL Server], security
- database objects [CLR integration], security
ms.assetid: 05d7a471-c5d5-4730-b903-e4edc8157bb4
author: rothja
ms.author: jroth
ms.openlocfilehash: 7774d83d959bccea3a1de1d7d0a06660d52c191b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641896"
---
# <a name="clr-integration-security"></a>CLR 集成安全性

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 公共语言运行时 (CLR) 集成的 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 的安全模式用于管理和保护 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 内运行的不同类型 CLR 对象和非 CLR 对象之间的访问。 这些对象可能由 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句或服务器上运行的其他 CLR 对象调用。 对象之间的调用称为链接。 对这些对象执行的安全检查类型取决于相关的链接类型。  
  
 CLR 集成安全模式可实现以下目的：  
  
-   默认情况下，在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中运行托管用户代码不应当损害 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的完整性和稳定性。 如果执行有可能损害 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可靠性的操作，则应当受到适当的高级权限的保护。  
  
-   托管用户代码不应当获得对数据库中用户数据或其他用户代码的未经授权访问。 用户定义代码应当在调用该代码的用户会话的安全上下文中运行，且拥有该安全上下文的正确特权。  
  
-   应当有控制来限制用户代码不得访问服务器以外的任何资源，而只能用于本地数据访问和计算。  
  
-   用户定义代码不应能通过在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 进程中运行而获得对系统资源的未经授权访问。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 现在集成了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 基于用户的安全模式和 CLR 基于代码访问的安全模式。 本节将讨论此组合安全方法的某些优势。  
  
 下表列出了本节的主题。  
  
 [CLR 集成代码访问安全性](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)  
 讨论托管代码的代码访问安全性 (CAS) 模式。  
  
 [宿主保护属性和 CLR 集成编程](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)  
 提供有关 SAFE 和 EXTERNAL_ACCESS 程序集中禁止的宿主保护属性 (HPA) 值的信息。  
  
 [CLR 集成安全性中的链接]()  
 介绍在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中用户代码段是如何能相互调用的。  
  
 [模拟和 CLR 集成安全性](../data-access/impersonation-and-credentials-for-connections.md)  
 讨论托管代码是如何使用模拟来访问外部资源的。  
  
 讨论当托管方法调用其他程序集中所包含类的方法时所产生的问题。  
  
 [应用程序域和 CLR 集成安全性](/previous-versions/sql/2014/database-engine/dev-guide/allowing-partially-trusted-callers?view=sql-server-2014&preserve-view=true)  
 描述如何将程序集加载到应用程序域。  
  
## <a name="see-also"></a>另请参阅  
 [管理 CLR 集成程序集](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)  
  
