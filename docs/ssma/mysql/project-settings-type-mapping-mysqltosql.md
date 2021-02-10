---
description: 项目设置（类型映射）(MySQLToSQL)
title: 项目设置 (类型映射)  (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 136fdf6d-657f-447b-af41-49bbc6e0e93e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2f3e077a93d90afccf05023e41f8e5f2ea6fabc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017317"
---
# <a name="project-settings-type-mapping-mysqltosql"></a>项目设置（类型映射）(MySQLToSQL)
"类型映射项目设置" 允许您为 SSMA 项目设置默认的类型映射。  

"项目设置" 和 "默认项目设置" 对话框中提供了类型映射：  
  
-   使用 "项目设置" 对话框可以设置当前项目的配置选项。 若要访问类型映射设置，请在 "工具" 菜单上选择 "项目设置"，然后单击左窗格中的 "类型映射"。  
  
-   使用 "默认项目设置" 对话框可以为所有项目设置配置选项。 若要访问类型映射设置，请在 "工具" 菜单上，选择 "默认项目设置"，从 " **迁移目标版本** " 下拉框中选择需要查看其设置的 "迁移项目类型"，然后单击左窗格中的 "类型映射"。  
  
## <a name="options"></a>选项  
  
##### <a name="source-type"></a>源类型  
它是 MySQL 数据类型，必须映射到目标数据库数据类型。  
  
##### <a name="target-type"></a>目标类型  
指定 MySQL 数据类型的目标数据库数据类型。  
  
##### <a name="add"></a>添加  
单击此可将数据类型添加到 "映射" 列表中。  
  
##### <a name="edit"></a>编辑  
单击此项可在 "映射" 列表中编辑所选的数据类型。  
  
##### <a name="remove"></a>删除  
单击此选项可从 "映射" 列表中删除所选的数据类型映射。  
  
##### <a name="reset-to-default"></a>重置为默认值  
单击可将类型映射列表重置为 SSMA 默认值。  
  
## <a name="type-mappings"></a>类型映射  
下表显示源和目标数据类型之间的默认映射  
  
|MySQL 数据类型|SQL Server 数据类型|  
|-|-|  
|bigint|bigint|  
|bigint [*。255]|bigint|  
|binary|binary [1]|  
|binary [0 .0]|binary [1]|  
|binary [2 .0]|binary [*]|  
|bit|binary [1]|  
|bit [0 ... 8]|binary [1]|  
|位 [17. 24]|二进制 [3]|  
|位 [25. 32]|二进制 [4]|  
|位 [33。|binary [5]|  
|位 [41.. 48]|二进制 [6]|  
|位 [49. 56]|二进制 [7]|  
|位 [57. 64]|二进制 [8]|  
|位 [9. 16]|二进制 [2]|  
|blob|varbinary(max)|  
|blob [0 .0]|varbinary [1]|  
|blob [2. 8000]|varbinary [*]|  
|blob [8001]|varbinary(max)|  
|bool|bit|  
|boolean|bit|  
|char|nchar [1]|  
|char 字节|binary [1]|  
|char byte [0 .0]|binary [1]|  
|char byte [2 .0]|binary [*]|  
|char [0 .0]|nchar [1]|  
|char [2 .0]|nchar [*]|  
|character|nchar [1]|  
|字符变化 [0 .0]|nvarchar [1]|  
|字符变化 [2 .0]|nvarchar|  
|字符 [0 .0]|nchar [1]|  
|字符 [2 .0]|nchar [*]|  
|date|date|  
|datetime|datetime2 [0]|  
|dec|Decimal|  
|dec [*。65]|decimal [*] [0]|  
|dec [*。65] [ \* .。为期|decimal [*] [ \* ]|  
|decimal|decimal|  
|decimal [*。65]|decimal [*] [0]|  
|decimal [*。65] [ \* .。为期|decimal [*] [ \* ]|  
|double|float [53]|  
|双精度|float [53]|  
|双精度 [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|double [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|fixed|numeric|  
|fixed [* .。。65] [ \* .。为期|数值 [*] [ \* ]|  
|FLOAT|float [24]|  
|float [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|float [*。53]|float [53]|  
|int|int|  
|int [*。255]|int|  
|整型|int|  
|integer [*。255]|int|  
|longblob|varbinary(max)|  
|longtext|nvarchar(max)|  
|mediumblob|varbinary(max)|  
|mediumint|int|  
|mediumint[*..255]|int|  
|mediumtext|nvarchar(max)|  
|国家字符|nchar [1]|  
|国家标准字符 [0 .0]|nchar [1]|  
|国家字符 [2 .0]|nchar [*]|  
|国家字符|nchar [1]|  
|国家字符可变|nvarchar [1]|  
|国家字符可变 [0 .0]|nvarchar [1]|  
|国家字符可变 [2.. 4000]|nvarchar [*]|  
|国家字符变化 [4001 ... *]|nvarchar(max)|  
|国家字符 [0 .0]|nchar [1]|  
|国家字符 [2 .0]|nchar [*]|  
|民族 varchar|nvarchar [1]|  
|国家 varchar [0 .0]|nvarchar [1]|  
|国家 varchar [2.. 4000]|nvarchar [*]|  
|国家 varchar [4001 ... *]|nvarchar(max)|  
|nchar|nchar [1]|  
|nchar varchar|nvarchar [1]|  
|nchar varchar [0 .0]|nvarchar [1]|  
|nchar varchar [2.. 4000]|nvarchar [*]|  
|nchar varchar [4001 ... *]|nvarchar(max)|  
|nchar [0 .0]|nchar [1]|  
|nchar [2 .0]|nchar [*]|  
|numeric|numeric|  
|数值 [*。65]|数值 [*] [0]|  
|数值 [*。65] [ \* .。为期|数值 [*] [ \* ]|  
|nvarchar|nvarchar [1]|  
|nvarchar [0 .0]|nvarchar [1]|  
|nvarchar [2.. 4000]|nvarchar [*]|  
|nvarchar [4001 ... *]|nvarchar(max)|  
|real|float [53]|  
|real [* .。。255] [ \* .。为期|数值 [*] [ \* ]|  
|serial|bigint|  
|smallint|smallint|  
|smallint [* .。。255]|smallint|  
|text|nvarchar(max)|  
|文本 [0 .0]|nvarchar [1]|  
|文本 [2.. 4000]|nvarchar [*]|  
|文本 [4001 ... *]|nvarchar(max)|  
|time|time|  
|timestamp|datetime|  
|tinyblob|varbinary [255]|  
|tinyint|smallint|  
|tinyint [*。255]|smallint|  
|tinytext|nvarchar [255]|  
|无符号 bigint|bigint|  
|无符号 bigint [*。255]|bigint|  
|无符号 dec|Decimal|  
|无符号 dec [*。65]|decimal [*] [0]|  
|无符号 dec [*。65] [ \* .。为期|decimal [*] [ \* ]|  
|无符号小数|Decimal|  
|无符号十进制 [*。65]|decimal [*] [0]|  
|无符号十进制 [*。65] [ \* .。为期|decimal [*] [ \* ]|  
|无符号 double|float [53]|  
|无符号双精度|float [53]|  
|无符号双精度 [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|无符号 double [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|未签名固定|numeric|  
|未签名固定 [*。65] [ \* .。为期|数值 [*] [ \* ]|  
|无符号 float|float [24]|  
|无符号 float [*。255] [ \* .。为期|数值 [*] [ \* ]|  
|无符号 float [*。53]|float [53]|  
|unsigned int|bigint|  
|无符号 int [*。255]|bigint|  
|无符号整数|bigint|  
|无符号整数 [*。255]|bigint|  
|无符号 mediumint|int|  
|未签名的 mediumint [*。255]|int|  
|无符号数字|numeric|  
|无符号数值 [*。65]|数值 [*] [0]|  
|无符号数值 [*。65] [ \* .。为期|数值 [*] [ \* ]|  
|无符号 real|float [53]|  
|无符号实际 [*。255 [[ \* 。为期|数值 [*] [ \* ]|  
|无符号 smallint|int|  
|无符号 smallint [*。255]|int|  
|无符号 tinyint|tinyint|  
|无符号 tinyint [*。255]|tinyint|  
|varbinary [0 .0]|varbinary [1]|  
|varbinary [2 ... 8000]|varbinary [*]|  
|varbinary [8001]|varbinary(max)|  
|varchar [0 .0]|nvarchar [1]|  
|varchar [2. 4000]|nvarchar [*]|  
|varchar [4001 ... *]|nvarchar(max)|  
|year|smallint|  
|year [2. 2]|smallint|  
|year [4. 4. 4]|smallint|  
  
##### <a name="add"></a>添加  
单击此可将数据类型添加到 "映射" 列表中。  
  
##### <a name="edit"></a>编辑  
单击此项可在 "映射" 列表中编辑数据类型。  
  
##### <a name="remove"></a>删除  
单击此选项可从 "映射" 列表中删除所选的数据类型映射。  
  
##### <a name="reset-to-default"></a>重置为默认值  
单击此设置可将所有数据类型映射重置为 SSMA 默认值。  
  
