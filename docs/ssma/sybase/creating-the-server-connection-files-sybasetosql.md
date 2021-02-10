---
description: 创建服务器连接文件 (SybaseToSQL)
title: " (SybaseToSQL) 创建服务器连接文件 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Creating Server Connection Files
- Sybase Console,Server Connection File Validation
ms.assetid: 35ef396f-9f98-429d-9fc5-4f413d08fb37
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 41fa50fba0f6d1398788246422a84b825594a5f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056662"
---
# <a name="creating-the-server-connection-files-sybasetosql"></a>创建服务器连接文件 (SybaseToSQL)
服务器信息可以在脚本文件的 "服务器" 部分中指定，也可以在单独的服务器连接文件中指定。 服务器连接文件的命令行参数为， `-c <serverconnectionfile>` 。 如果脚本文件和服务器连接文件中同时存在相同的服务器 id，则考虑脚本文件中的服务器定义。  
  
**示例：**  
  
```  
1.<!--Sample of server connection file commands -->  
  
<sybase name="<source-server-unique-name>">  
  
  <standard-mode>  
  
    <provider value="Ole DB Provider"/>  
  
    <server-name value="<server-name>"/>  
  
    <server-port value="<port>"/>  
  
    <user-id value="<password>"/>  
  
  </standard-mode>  
  
</sybase>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication>  
  
    <database value="<database-name>"/>  
  
    <server value="<server-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
    <encrypt value="<true/false>"/>  
  
    <trust-server-certificate value="<true/false>"/>  
  
  </sql-server-authentication >  
  
</sql-server>  
```  
  
```  
2.<!-Sample of server connection file commands-->  
<sybase name="<source-server-unique-name>">  
  
  <advanced-mode>  
  
    <connection-string value="User ID=<user-name>;Password=<password>;Provider=ASEOLEDB.1;Server=<server-name>;Port=<port>;OLE DB Services = -2;"/>  
  
  </advanced-mode >  
  
</sybase>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication >  
  
    <database value="<database-name>"/>  
  
    <server value="<server-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
  </sql-server-authentication >  
  
</sql-server>  
```  
  
## <a name="server-connection-file-validation"></a>服务器连接文件验证  
用户可以根据 "架构" 文件夹中提供的架构定义文件 **S2SSConsoleScriptServersSchema** 来轻松地验证其服务器连接文件。  
  
## <a name="next-step"></a>下一步  
操作控制台的下一步是 [执行 SSMA 控制台 &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅  
[执行 SSMA 控制台](executing-the-ssma-console-sybasetosql.md)  
  
