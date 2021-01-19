---
description: SQL Server 复制中不推荐使用的功能
title: SQL Server 复制中不推荐使用的功能 | Microsoft Docs
ms.custom: ''
ms.date: 01/22/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server replication]
ms.assetid: 46bd3edd-d6de-40a6-a015-21cce8321feb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 347400c7cf0064015ad5a593edec478f2114906c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170259"
---
# <a name="deprecated-features-in-sql-server-replication"></a>SQL Server 复制中不推荐使用的功能
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题介绍 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中仍然可用但不推荐使用的复制功能。 按照计划， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]未来版本将不再具有这些功能。 在新的应用程序中不应使用这些不推荐使用的功能。  
  
## <a name="items-deprecated-in-sssql15"></a>[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]  
  
|Feature|说明|  
|-------------|-----------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|如果每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端点都处于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本的两个主版本内，则支持复制。 因此， [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 不支持面向或源自 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]的复制。|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)]|如果每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端点都处于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本的两个主版本内，则支持复制。 因此， [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 不支持面向或源自 [!INCLUDE[ssEW](../../includes/ssew-md.md)]的复制。|  
  
## <a name="see-also"></a>另请参阅  
 [复制的向后兼容性](../../relational-databases/replication/replication-backward-compatibility.md)  
  
  
