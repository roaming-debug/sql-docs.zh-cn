---
description: SMO 对象模型
title: SMO 对象模型 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- object models [SMO]
- SMO [SQL Server], object model
- SQL Server Management Objects, object model
ms.assetid: bd6e59b6-ca46-42c0-adb2-c9d64cf6e00b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8ed8263bcbee94ea917dbf53a987c63c6318687c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439795"
---
# <a name="smo-object-model"></a>SMO 对象模型
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  SMO 对象模型由对象的层次结构构成。 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象是顶层对象，所有实例类对象都位于 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象之下。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> 类是具有单独的对象层次结构的顶级类。 <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer>对象表示 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可通过 WMI 提供程序使用的服务和网络设置。  
  
 除了 <xref:Microsoft.SqlServer.Management.Smo.Server> 和 <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> 对象外，存在若干表示任务或操作的实用工具类，例如 <xref:Microsoft.SqlServer.Management.Smo.Transfer>、<xref:Microsoft.SqlServer.Management.Smo.Backup> 或 <xref:Microsoft.SqlServer.Management.Smo.Restore>。  
  
 SMO 对象模型由若干命名空间构成。 有关详细信息，请参阅 [SMO 命名空间](../../relational-databases/server-management-objects-smo/smo-object-model-namespaces.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SMO 对象模型关系图](../../relational-databases/server-management-objects-smo/smo-object-model-diagram.md)   
 [SMO 命名空间](../../relational-databases/server-management-objects-smo/smo-object-model-namespaces.md)   
 [用于配置管理的 WMI 提供程序的概念](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md)  
  
  
