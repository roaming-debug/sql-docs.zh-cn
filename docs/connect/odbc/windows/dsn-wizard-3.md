---
description: 了解如何在数据源向导中定义默认数据库之类的内容，以创建与 SQL Server 的新 ODBC 连接。
title: 数据源向导屏幕 3 (ODBC Driver for SQL Server)
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2263cfde2f48da1562e4a23015cb0e575a23cc4e
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673121"
---
# <a name="data-source-wizard-screen-3"></a>数据源向导屏幕 3

指定默认数据库、将由驱动程序使用的各种 ANSI 选项和镜像服务器的名称。

## <a name="options"></a>选项

### <a name="change-the-default-database-to"></a>更改默认数据库为

指定使用该数据源建立的任何连接的默认数据库的名称。 清除此框后，连接将使用在服务器上为登录 ID 定义的默认数据库。 选择此框后，在框中命名的数据库将覆盖为登录 ID 定义的默认数据库。 如果“附加数据库文件名”框中包含主文件的名称，则主文件描述的数据库便附加为使用“更改默认数据库为”框中指定的数据库名称的数据库。

使用登录 ID 的默认数据库比在 ODBC 数据源中指定默认数据库更有效。

### <a name="mirror-server"></a>镜像服务器

指定要镜像的数据库的故障转移伙伴的名称。 如果数据库名称未显示在“更改默认数据库为”框中，或该名称显示为默认数据库，则“镜像服务器”将灰显。

您还可以指定镜像服务器的服务器主体名称 (SPN)。 镜像服务器的 SPN 用于在客户端和服务器之间相互进行身份验证。

### <a name="attach-database-filename"></a>附加数据库文件名

指定可附加数据库的主文件的名称。 该数据库作为数据源的默认数据库附加和使用。 指定主文件的完整路径和文件名。 在“更改默认数据库为”框中指定的数据库名称将用作附加数据库的名称。

### <a name="use-ansi-quoted-identifiers"></a>使用 ANSI 引用的标识符

指定在 ODBC Driver for SQL Server 连接时将 `QUOTED_IDENTIFIERS` 设置为开启。 选中此复选框后，SQL Server 将执行与引号有关的 ANSI 规则。 双引号只能用于标识符，如列和表名称。 字符串必须用单引号引起来：

```sql
SELECT "LastName"
FROM "Person.Contact"
WHERE "LastName" = 'O''Brien'
```

清除此复选框后，使用引用标识符的应用程序（如 Microsoft Excel 附带的 Microsoft Query 实用工具）将在生成带有引用标识符的 SQL 语句时出错。

### <a name="use-ansi-nulls-paddings-and-warnings"></a>使用 ANSI 的空值、填充及警告

指定在 SQL Server 的 ODBC 驱动程序连接时将 ANSI_NULLS、ANSI_WARNINGS 和 ANSI_PADDINGS 选项设置为开启。

在 ANSI_NULLS 设置为开启时，服务器将执行与比较列是否为 NULL 有关的 ANSI 规则。 在所有 NULL 比较中，必须使用 ANSI 语法“IS NULL”或“IS NOT NULL”。 不支持 Transact-SQL 语法“= NULL”。

在 ANSI_WARNINGS 设置为开启时，SQL Server 会对违反 ANSI 规则但并不违反 Transact-SQL 规则的情况发出警告消息。 此类错误的示例为执行 INSERT 或 UPDATE 语句时数据截断，或在执行聚合函数时遇到 Null 值。

在 ANSI_PADDING 设置为开启时，将不自动剪裁 varchar 值的尾随空格和 varbinary 值的尾随零。

### <a name="application-intent"></a>应用程序意向

连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。

### <a name="multi-subnet-failover"></a>多子网故障转移

如果应用程序连接到不同子网上的高可用性、灾难恢复可用性组 (AG)（AlwaysOn 可用性组），那么启用多子网故障转移 将配置适用于 SQL Server 的 ODBC 驱动程序，以便更快地检测和连接到（当前）活动服务器。

### <a name="transparent-network-ip-resolution"></a>透明网络 IP 解析

将多子网故障转移的行为更改为，在故障转移期间更快地重新连接。 有关详细信息，请参阅[使用透明网络 IP 解析](../using-transparent-network-ip-resolution.md)。

### <a name="column-encryption"></a>列加密

在使用 SQL Server 2016 及更高版本中的 [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) 功能加密的列中，对传入和传出的数据启用自动解密和加密。

### <a name="use-fmtonly-metadata-discovery"></a>使用 FMTONLY 元数据发现

连接到 SQL Server 2012 或更高版本时，使用旧 SET FMTONLY 元数据发现方法。 仅当使用 [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 不支持的查询（如包含临时表的查询）时才启用此选项。

### <a name="next"></a>下一步

前进到向导的下一个屏幕。

### <a name="back"></a>返回

返回到向导的上一个屏幕。

## <a name="next-steps"></a>后续步骤

[数据源向导屏幕 2](dsn-wizard-2.md)  
[数据源向导屏幕 4](dsn-wizard-4.md)  
