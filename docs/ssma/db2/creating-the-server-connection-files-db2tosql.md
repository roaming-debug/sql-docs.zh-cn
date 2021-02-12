---
description: " (DB2ToSQL 创建服务器连接文件) "
title: " (DB2ToSQL) 创建服务器连接文件 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 685419f6-8606-462c-be12-8bace45bede6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 6a03c33bac6415ed4916b5bf7d95cfcff38183be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076640"
---
# <a name="creating-the-server-connection-files-db2tosql"></a> (DB2ToSQL 创建服务器连接文件) 

服务器信息可以在脚本文件的 "服务器" 部分中指定，也可以在单独的服务器连接文件中指定。 服务器连接文件的命令行参数为， `-c <serverconnectionfile>` 。 如果脚本文件和服务器连接文件中同时存在相同的服务器 ID，则考虑脚本文件中的服务器定义。

**示例：1**

```xml
<!-- Sample of server connection file commands -->
<db2 name="<source-server-unique-name>">
  <standard-mode>

    <connection-provider value="OleDB Provider" />

    <!-- Defines server manager to connect.
         Available manager attribute values
           • zOs - DB2 for z/OS
           • LUW - DB2 for Linux, Unix and Windows
           • DBi - DB2 for i -->
    <database-manager value="$Db2Manager$" />

    <server-name value="$Db2HostName$" />

    <port value="$Db2Port$" />

    <initial-catalog value="$Db2Instance$" />

    <user-id value="$Db2UserName$" />

    <password value="$Db2Password$"/>

  </standard-mode>
</db2>
```

```xml
<sql-server name="<target-server-unique-name>">
  <sql-server-authentication>

    <server value="<server-name>" />

    <database value="<database-name>" />
  
    <user-id value="<user-name>"/>
  
    <password value="<password>"/>

  </sql-server-authentication>
</sql-server>
```

## <a name="next-step"></a>下一步

操作控制台的下一步是 [执行 SSMA 控制台 &#40;DB2ToSQL&#41;](../../ssma/db2/executing-the-ssma-console-db2tosql.md)

## <a name="see-also"></a>另请参阅

- [执行 SSMA 控制台](./executing-the-ssma-console-db2tosql.md)