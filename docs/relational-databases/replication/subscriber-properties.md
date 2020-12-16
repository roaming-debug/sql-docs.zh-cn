---
description: 订阅服务器属性
title: 订阅服务器属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.subscribers.f1
helpviewer_keywords:
- Subscriber Properties dialog box
ms.assetid: 32aa0347-64e4-4aa4-ac57-6bd3e5d52070
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: b2bd65605a719408ac08454584601cb5636faa4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479628"
---
# <a name="subscriber-properties"></a>订阅服务器属性
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  “订阅服务器属性”对话框包含与运行 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 之前的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的订阅服务器有关的信息。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

  
## <a name="options"></a>选项  
 **到订阅服务器的代理连接**  
 分发代理和合并代理从分发服务器连接到订阅服务器时所处的上下文。此选项只适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]以前的版本。  
  
 选择 **“模拟代理进程帐户”** ，可以使用分发服务器上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理帐户的上下文连接到订阅服务器；也可以指定 **“SQL Server 身份验证”**，再输入 **“登录名”** 和 **“密码”** 的值。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您选择 **“模拟代理进程帐户”** 。  
  
 对于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本，可以在新建订阅向导中为每个订阅指定连接信息，并可以在 **“订阅属性”** 对话框中更改连接信息。  
  
 **默认代理计划**  
 对于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]版本的订阅服务器，为新建订阅向导中所使用的默认计划。  
  
 **杂项**  
 包含有关订阅服务器和订阅服务器类型的信息。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [订阅发布](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
