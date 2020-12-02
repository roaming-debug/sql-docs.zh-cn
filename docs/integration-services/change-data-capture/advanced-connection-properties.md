---
description: 高级连接属性
title: 高级连接属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 4edfab68-7e68-45e8-a3f3-a0049ff7eb9e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fd7ed8863356ff3e6b67d456628bd3dbc1ab6a5b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123743"
---
# <a name="advanced-connection-properties"></a>高级连接属性

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“高级连接属性”** 对话框可以将更多连接参数添加到连接字符串。  
  
 其他连接参数可以是您正在使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库实例支持的任何 ODBC 连接参数。  
  
 使用 **“高级连接属性”** 对话框添加的参数将添加到在 **“连接到 SQL Server”** 对话框中选择的参数上。  
  
 所提供的各个参数的最后一个实例均覆盖该参数的任何以前的实例。 使用 **“高级连接参数”** 添加的参数将跟踪并替换 **“SQL Server 连接”** 对话框中提供的参数。 例如，如果“SQL Server 连接”对话框将服务器名称指定为 SERVER1，而“其他连接参数”页包含 ;SERVER=SERVER2，则会连接到 SERVER2。  
  
 使用 **“高级连接属性”** 对话框添加的参数将作为纯文本传递。  
  
> [!IMPORTANT]  
>  不要在 **“高级连接属性”** 对话框中包含登录凭据。 否则，它们将在没有加密的情况下通过网络传递。  
  
## <a name="see-also"></a>另请参阅  
 [访问 CDC 设计器控制台](../../integration-services/change-data-capture/access-the-cdc-designer-console.md)   
 [用于实例创建的 SQL Server 连接](../../integration-services/change-data-capture/sql-server-connection-for-instance-creation.md)  
  
  
