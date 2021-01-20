---
description: '连接到 DB2 (DB2ToSQL) '
title: 连接到 DB2 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: c590a76696f2729bf5d1c6c5d4ca5b0242a63df6
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596729"
---
# <a name="connect-to-db2-db2tosql"></a>连接到 DB2 (DB2ToSQL) 
使用 " **连接到 db2** " 对话框连接到要迁移的 DB2 数据库。  
  
若要访问此对话框，请在 " **文件** " 菜单上选择 " **连接到 DB2**"。 如果以前已连接，则该命令将 **重新连接到 DB2**。  
  
## <a name="options"></a>选项  
**提供程序**  
选择用于连接到 DB2 数据库的数据访问接口。 可用的提供程序是 DB2 客户端提供程序和 OLE DB 提供程序。 默认值为 DB2 客户端提供程序。  
  
**模式**  
选择 "标准"、"TNSNAME" 或 "连接字符串" 模式。  
  
-   在 "标准" 模式下，为提供程序、服务器名称、服务器端口、DB2 SID、用户名和密码输入或选择值。  
  
-   在 TNSNAME 模式下，输入连接标识符 (DB2 数据库的 TNS 别名) 、用户名和密码。  
  
-   在连接字符串模式下，你将提供一个连接字符串。  
  
    > [!IMPORTANT]  
    > 建议您不要使用连接字符串模式，因为文本可能包含密码，并以明文形式发送。  
  
默认值为 "标准模式"。  
  
**服务器名称**  
输入 DB2 服务器的名称。 默认服务器名称与计算机名称相同。 这是一个标准模式选项。  
  
**服务器端口**  
如果使用的端口号不是 1521 (默认) 连接到 DB2，请输入端口号。 这是一个标准模式选项。  
  
**连接标识符**  
输入 DB2 连接标识符。 这是在本地 tnsnames.ora. tnsnames.ora 文件中定义的数据库的别名。  
  
这是一个 TNSNAME 模式选项。  
  
**DB2 SID**  
输入数据库的 SID。 SID 是用于区分计算机上 DB2 数据库的标识符。 数据库的默认 SID 是数据库名称的前八个字符。  
  
这是一个标准模式选项。  
  
**用户名**  
输入 SSMA 将用于连接到 DB2 数据库的用户名。  
  
**密码**  
输入用户名的密码。  
  
**连接字符串**  
> [!IMPORTANT]  
> 建议您不要使用连接字符串模式，因为文本可能包含密码，并以明文形式发送。  
  
如果使用连接字符串模式，请输入与 DB2 的连接的完整连接字符串。  
  
连接字符串包含参数名称和值对。  
  
-   有关 OLE DB 连接字符串的信息，请参阅 MSDN Library 上的 [DB2 的 Microsoft OLE DB 提供程序](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) 文章。  
  
对于 SSMA 连接字符串，请始终包含提供程序参数。 此外，请确保在连接到 DB2 时包含 Port 参数。  
