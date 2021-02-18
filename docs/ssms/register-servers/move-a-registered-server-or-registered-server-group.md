---
description: 移动已注册的服务器或已注册的服务器组
title: 移动已注册的服务器或服务器组
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- moving registered server or server group
- Registered Servers [SQL Server], server groups
- server groups [SQL Server]
- Registered Servers [SQL Server], moving server or server group
- groups [SQL Server], server
ms.assetid: 4438ca98-3abe-4dea-a760-48a9dad63c2e
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.openlocfilehash: 2891d4dcfcae8689dda1aaf78cad9c4c43bae220
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350471"
---
# <a name="move-a-registered-server-or-registered-server-group"></a>移动已注册的服务器或已注册的服务器组

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何通过在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中移动已注册服务器或服务器组来整理服务器。 服务器组可以包括注册服务器，也可以包含其他服务器组。 服务器和服务器组都可以从一个服务器组移动到另一个服务器组。  

## <a name="SSMSProcedure"></a>  

### <a name="to-move-a-registered-server-or-server-group"></a>移动注册服务器或服务器组  

1. 在注册服务器中，右键单击某服务器或服务器组，再单击“移动到”。  
  
2. 在 **“移动服务器注册”** 对话框中，展开服务器组列表，单击您要显示的服务器或服务器组的节点，再单击 **“确定”**。  

## <a name="see-also"></a>另请参阅

[注册服务器](./register-servers.md)

[创建或编辑服务器组 (SQL Server Management Studio)](./create-or-edit-a-server-group-sql-server-management-studio.md)