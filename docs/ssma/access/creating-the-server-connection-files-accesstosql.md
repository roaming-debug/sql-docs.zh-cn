---
description: " (AccessToSQL 创建服务器连接文件) "
title: " (AccessToSQL) 创建服务器连接文件 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 829153be-aa8e-4162-87e8-69882feecf19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9d956167cd6a4cea41d9381996a0418b2cd5f4c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044937"
---
# <a name="creating-the-server-connection-files-accesstosql"></a> (AccessToSQL 创建服务器连接文件) 
可以在脚本文件的 "服务器" 部分中指定服务器信息。 还可以在单独的服务器连接文件中指定服务器信息。 服务器连接文件的命令行参数为 `-c <serverconnectionfile>` 。 如果脚本和服务器连接文件中同时存在相同的服务器 id，则考虑脚本文件中的服务器定义。  
  
```xml  
<!--Sample of server connection file commands -->  
<!--Connection to SQL Server-->  
  
<sql-server name="target_3">  
  
  <sql-server-authentication>  
  
    <server value="$TargetServerName$"/>  
  
    <database value="$TargetDB$"/>  
  
    <user-id value="$TargetUserName$"/>  
  
    <password value="$TargetPassword$"/>  
  
    <encrypt value="true"/>  
  
    <trust-server-certificate value="true"/>  
  
  </sql-server-authentication>  
  
</sql-server>  
```  
  
```xml  
<!--Connection to SQL Azure-->  
  
<sql-azure name="target_azure">  
  
  <server value="$TargetAzureServerName$"/>  
  
  <database value="$TargetAzureDB$"/>  
  
  <user-id value="$TargetAzureUserName$"/>  
  
  <password value="$TargetAzurePassword$"/>  
  
</sql-azure>  
```  
  
## <a name="server-connection-file-validation"></a>服务器连接文件验证  
用户可以根据 "架构" 文件夹中提供的架构定义文件 **"A2SSConsoleScriptServersSchema"** 轻松地验证其服务器连接文件。  
  
## <a name="next-step"></a>后续步骤  
操作控制台的下一步是 [执行 SSMA 控制台 &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[执行 SSMA 控制台 (访问) ](./executing-the-ssma-console-accesstosql.md)  
