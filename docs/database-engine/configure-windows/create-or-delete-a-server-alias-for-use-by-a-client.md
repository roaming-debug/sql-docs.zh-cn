---
title: 创建或删除供客户端使用的服务器别名 | Microsoft Docs
description: 了解如何创建和删除别名（在连接到 SQL Server 实例时可以使用的备用名称）。 了解别名的优点。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- server alias
helpviewer_keywords:
- aliases [SQL Server], deleting
- aliases [SQL Server], creating
ms.assetid: b687e376-ee33-470d-b65a-87246bfefe6f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bb1c9179151dd41f166c81791078ed8c82dcd5c3
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98782974"
---
# <a name="create-or-delete-a-server-alias-for-use-by-a-client"></a>创建或删除供客户端使用的服务器别名
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 SQL Server 配置管理器在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中创建或删除服务器别名。 别名是可用于进行连接的备用名称。 别名封装了连接字符串所必需的元素，并使用用户所选择的名称显示这些元素。 可对任何客户端应用程序使用别名。 通过创建服务器别名，客户端计算机便可使用不同的网络协议连接到多个服务器，无需针对每台服务器指定协议和连接详细信息。 另外，还可以一直启用各种网络协议，即使只是偶尔会用到它们。 如果已将服务器配置为侦听非默认端口号或命名管道，并且禁用了 SQL Server Browser 服务，请创建一个别名来指定新端口号或命名管道。  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
  
#### <a name="to-create-an-alias"></a>创建别名  
  
1.  在 SQL Server 配置管理器中，展开“SQL Server Native Client 配置”，右键单击“别名”，再单击“新建别名”。  
  
2.  在 **“别名”** 框中，键入别名。 当客户端应用程序进行连接时，它们使用该名称。  
  
3.  在 **“服务器”** 框中，键入服务器的名称或 IP 地址。 对于命名实例，请追加实例名称。  
  
4.  在 **“协议”** 框中，选择用于该别名的协议。 选择某个协议，将可选属性框的标题更改为“端口号”、“管道名称”或“连接字符串”。  
  
 对于创建自己的连接字符串的程序员来说， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器帮助介绍的连接字符串颇为有用。 若要访问此信息，请在 **“新建别名”** 对话框中按 F1，或单击 **“帮助”** 。  
  
> [!NOTE]  
>  如果配置的别名正与错误的服务器或实例进行连接，则请禁用并重新启用相关联的网络协议。 这样做会清除缓存的连接信息，从而允许客户端进行正确连接。  
  
#### <a name="to-delete-an-alias"></a>删除别名  
  
1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中，展开 **“SQL Server Native Client 配置”** ，然后单击 **“别名”** 。  
  
2.  在“详细信息”窗格中，右键单击要删除的别名，然后单击“删除”。  
  
  
