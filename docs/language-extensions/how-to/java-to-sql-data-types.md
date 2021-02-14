---
title: Java 数据类型
titleSuffix: SQL Server Language Extensions
description: 针对输入和输出数据结构以及 sp_execute_external_script 上的输入参数，将数据类型从 Java 映射到 SQL Server。
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 66cec63a05624a219ddbc1eb77fc5f9325626234
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069862"
---
# <a name="java-and-sql-server-supported-data-types"></a>Java 和 SQL Server 支持的数据类型
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何针对数据结构和 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 上的参数将 SQL Server 数据类型映射到 Java 数据类型。

输入/输出数据集和输入/输出参数当前支持以下 SQL 和 Java 数据类型。

| SQL 数据类型        | Java 数据类型 | 注释 |
| ------------- |-------------|-|
| bit      | boolean | |
| Tinyint      | short      | |
| Smallint | short      | |
| Int | int      | |
| Real | FLOAT      | |
| Bigint | long      | |
| FLOAT | double      | |
| nchar(n) | String      | |
| nvarchar(n) | String      | |
| binary(n) | byte[]      | |
| varbinary(n) | byte[]      | |
| nvarchar(max) | String      | |
| varbinary(max) | byte[]      | |
| uniqueidentifier | String | |
| char(n) | String | 仅支持 UTF8 字符串 |
| varchar(n) | String | 仅支持 UTF8 字符串 |
| varchar(max) | String | 仅支持 UTF8 字符串 |
| date | java.sql.date  | |
| numeric | java.math.BigDecimal  | |
| Decimal | java.math.BigDecimal  | |
| money | java.math.BigDecimal  | |
| smallmoney | java.math.BigDecimal  | |
| smalldatetime | java.sql.timestamp  | |
| datetime | java.sql.timestamp  | |
| datetime2 | java.sql.timestamp  | |


## <a name="next-steps"></a>后续步骤

+ [如何在 SQL Server 中调用 Java](../how-to/call-java-from-sql.md)