---
title: 强密码 | Microsoft Docs
description: 了解 SQL Server 中的密码，并找出用于增强部署安全性的强密码的构造。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logins [SQL Server], passwords
- passwords [SQL Server], strong
- symbols [SQL Server]
- security [SQL Server], passwords
- passwords [SQL Server], symbols
- characters [SQL Server], password policies
- strong passwords [SQL Server]
ms.assetid: 338548f4-c4d8-47ca-b597-5c9c0f2fa205
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0594f08c28653e6fd118fa70dd4d0a6dd6debeb0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739567"
---
# <a name="strong-passwords"></a>强密码
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  在服务器安全部署中，密码可能是最薄弱的一个环节。 选择密码时保持高度谨慎。 强密码有以下特征：  
  
-   长度至少为八个字符。  
  
-   密码中组合使用字母、数字和符号字符。  
  
-   字典中查不到。  
  
-   不是命令名。  
  
-   不是人名。  
  
-   不是用户名。  
  
-   不是计算机名。  
  
-   定期更改。  
  
-   与以前的密码不同。  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 密码最多可包含 128 个字符，其中包括字母、符号和数字。 由于在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中经常使用登录名、用户名、角色和密码，所以必须用英文双引号 (") 或方括号 ([ ]) 括起某些符号。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 登录名、用户、角色或密码具有以下特征，请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句中使用以下分隔符：  
  
-   含有空格或以空格开头。  
  
-   以 $ 或 \@ 字符开头。  
  
 如果用于 OLE DB 或 ODBC 连接字符串，则登录名或密码不能包含以下字符：[] () , ; ? * ! \@ =。 这些字符用于初始化连接或分隔连接值。  
  
## <a name="related-content"></a>相关内容  
 [密码策略](../../relational-databases/security/password-policy.md)  
  
  
