---
description: Teradata 数据类型支持
title: Teradata 数据类型支持 | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346526"
---
# <a name="data-type-support"></a>数据类型支持

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

SSIS 组件使用 Teradata Parallel Transporter API (TPT API) 从 Teradata 数据库加载数据以及传出/传入数据，因此只能在 SSIS 中使用 TPT API 支持的数据类型。

> [!NOTE]
>
> TPTdata 中的 TIME、TIMESTAMP 和 INTERVAL 数据类型由 TPT API 作为固定大小的字符串处理。 这些数据类型由 Teradata 的 SSIS 组件作为字符串处理。

## <a name="data-type-mapping"></a>数据类型映射

下表列出了 Teradata 数据库数据类型及其与 SSIS 数据类型的默认映射。 此外，还展示了不受支持的 Teradata 数据类型。

|SQL 数据类型|SSIS 数据类型|
|:-|:-|
|DECIMAL/NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|INTEGER|DT_I4|
|FLOAT/REAL/DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TIME<br>TIME(n)|DT_STR|
|TIMESTAMP<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR（对于 Unicode 字符集，则为 DT_WSTR）<br>**注释**：<br> 支持的 VARCHAR 最大长度为 32000。 <br> 允许的 DT_STR 最大长度为 8000 个字符，DT_WSTR 为 4000 个字符。 如果超出，则会截断数据。|
|LONG VARCHAR|不支持|
|CLOB|不支持|
|BYTE|DT_BYTES<br>**注意**：允许的最大长度为 8000 个字节。 如果超出，则会截断数据。|
|VARBYTE|DT_BYTES<br>**注意**：允许的最大长度为 8000 个字节。 如果超出，则会截断数据。|
|BLOB|不支持|

## <a name="next-steps"></a>后续步骤

- 配置 [Teradata 连接管理器](teradata-connection-manager.md)
- 配置 [Teradata 源](teradata-source.md)
- 配置 [Teradata 目标](teradata-destination.md)
- 如有任何疑问，请访问[技术社区](https://aka.ms/AA6iwdw)。
