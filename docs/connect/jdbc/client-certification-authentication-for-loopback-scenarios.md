---
description: 环回方案的客户端证书身份验证
title: 环回方案的客户端证书身份验证 | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: peterbae
ms.author: v-hyba
ms.openlocfilehash: 168034034e5f2abe53bd47029b99fa78c250336b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836964"
---
# <a name="client-certificate-authentication-for-loopback-scenarios"></a>环回方案的客户端证书身份验证

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

在 SQL Server 2016 中添加了名为 sp_execute_external_script (SPEES) 的新存储过程。 作为扩展性工作量的一部分，此存储过程允许 SQL Server 在 SQL Server 之外启动和执行外部脚本。 它还提供了对 R 和 Python 脚本的支持，两者都具有可以使用 JDBC 驱动程序连接到 SQL Server 的库。 虽然 Windows Box 上的 SQL Server 可以使用 Windows 集成身份验证，以使用与启动查询的用户相同的凭据对这些环回连接进行身份验证，但 Linux SQL Server 不能执行相同操作。 因此，将添加客户端证书身份验证，以允许用户使用证书和密钥进行身份验证。

## <a name="connecting-using-client-certificate-authentication"></a>使用客户端证书身份验证进行连接

JDBC 驱动程序为此功能添加了三个连接属性：

* clientCertificate – 指定用于身份验证的证书。 JDBC 驱动程序将支持 PFX、PEM、DER 和 CER 文件扩展名。

格式
```
clientCertificate=<file_location>
``` 
驱动程序使用证书文件。 对于 PEM、DER 和 CER 格式的证书，clientKey 属性是必需的。 文件位置可以是相对的，也可以是绝对的。
 
* clientKey – 指定由 clientCertificate 属性指定的 PEM、DER 或 CER 证书的私钥的文件位置。

格式
```
clientKey=<file_location>
```
指定私钥文件的位置。 如果私钥文件受密码保护，则需要 password 关键字。 文件位置可以是相对的，也可以是绝对的。

* clientKeyPassword – 提供用于访问 clientKey 文件私钥的可选密码字符串。

此功能仅在针对 Linux SQL Server 2019 及更高版的环回身份验证方案中正式受支持。

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序连接到 SQL Server](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
