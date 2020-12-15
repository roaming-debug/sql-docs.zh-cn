---
description: SMO 对象模型命名空间
title: SMO 命名空间 |Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- object models [SMO]
- SMO [SQL Server], namespaces
- namespaces [SMO]
- SQL Server Management Objects, namespaces
ms.assetid: 7bfabe4d-9f4c-4bc9-b998-93bd2b50ee8a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fd47c2867303eee6fa991d9932486f93918253f9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439829"
---
# <a name="smo-object-model-namespaces"></a>SMO 对象模型命名空间
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象 (SMO) 具有不同的命名空间。 不同的命名空间表示 SMO 中的不同功能区域。  
  
 在中 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] ，SMO 程序集位于 C:\Program FILES\MICROSOFT SQL Server\130\SDK\Assemblies 文件夹中。  
  
## <a name="namespaces"></a>命名空间  
 SMO 命名空间包括：  
  
|类|函数|  
|-----------|--------------|  
|<xref:Microsoft.SqlServer.Management.Smo>|包含用于以编程方式操作的实例类、实用工具类和枚举 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|<xref:Microsoft.SqlServer.Management.Common>|包含复制管理对象 (RMO) 和 SMO 共有的类，例如连接类。|  
|<xref:Microsoft.SqlServer.Management.Smo.Agent>|包含表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的类。|  
|<xref:Microsoft.SqlServer.Management.Smo.Wmi>|包含表示 WMI 提供程序的类。|  
|<xref:Microsoft.SqlServer.Management.Smo.RegisteredServers>|包含表示已注册的服务器的类。|  
|<xref:Microsoft.SqlServer.Management.Smo.Mail>|包含表示数据库邮件的类。|  
|<xref:Microsoft.SqlServer.Management.Smo.Broker>|包含表示 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的类。|  
  
  
