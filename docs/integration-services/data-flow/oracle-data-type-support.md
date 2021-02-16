---
description: Microsoft Connector for Oracle 数据类型支持
title: Microsoft Connector for Oracle 数据类型支持 | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ef719fc0fe41120fe42d324beb563b06bb81060
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343215"
---
# <a name="microsoft-connector-for-oracle-data-type-support"></a>Microsoft Connector for Oracle 数据类型支持

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

适用于 Oracle 的 SSIS 组件并不支持所有的 Oracle 数据类型。 在 SSDT 中设计包时，如果有数据类型不受支持的列，系统会显示警告，并从映射列中删除这些列。 无法将数据加载到数据类型不受支持的列。

## <a name="data-type-mapping"></a>数据类型映射

下表列出了 Oracle Database 数据类型及其与 SSIS 数据类型的默认映射。 此外，还列出了不受支持的 Oracle 数据类型。

|Oracle 数据库数据类型|SSIS 数据类型|注释|
|:-|:-|:-|
|VARCHAR2|DT_STR||
|NVARCHAR2|DT_WSTR||
|CHAR|DT_STR||
|NUMBER|DT_R8|可以将这更改为具有特定精度和小数位数的 DT_NUMERIC。 精度和小数位数是由用户根据需要定义。 输出的列数据是具有固定精度和小数位数的数字。|
|NUMBER(P, S)| 当小数位数为 0 时，根据精度 (P) <li> DT_I1 <Li> DT_I2 <Li> DT_I4 <Li> DT_NUMBERIC(P,0)||
||DT_NUMERIC(P,S)||
|DATE|DT_DBTIMESTAMP||
|<li>TIMESTAMP <li>TIMESTAMP WITH TIME ZONE <li>INTERVAL YEAR TO MONTH <li>INTERVAL DAY TO SECOND <li>TIMESTAMP WITH LOCAL TIME ZONE|DT_STR||
|RAW|DT_BYTES||
|CLOB|DT_TEXT|仅在数组模式（而不是快速加载模式）下支持 CLOB、NCLOB 和 BLOB 数据类型。|
|NCLOB|DT_NTEXT||
|BLOB|DT_IMAGE||
|UROWID|不支持||
|REF|不支持||
|BFILE|不支持||
|LONG|不支持||
|LONG RAW|不支持||
|ROWID|不支持||
|用户定义类型（对象类型、VARRAY、嵌套表）|不支持||

## <a name="next-steps"></a>后续步骤

- 配置 [Oracle Connection Manager](oracle-connection-manager.md)。
- 配置 [Oracle 源](oracle-source.md)。
- 配置 [Oracle 目标](oracle-destination.md)。
- 若有任何疑问，请访问 [TechCommunity](https://aka.ms/AA5u35j)。
