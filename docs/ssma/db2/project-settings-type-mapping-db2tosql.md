---
description: " (类型映射的项目设置)  (DB2ToSQL) "
title: 项目设置 (类型映射)  (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: cf426c69-6a8e-4d19-951d-6661d5ae2562
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b562e886bcb09eae4c02607f17eb7419b313a759
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072029"
---
# <a name="project-settings-type-mapping-db2tosql"></a> (类型映射的项目设置)  (DB2ToSQL) 
" **项目设置** " 对话框的 "类型映射" 页包含用于自定义 SSMA 将 DB2 数据类型转换为数据类型的方式的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
"类型映射" 页在 " **项目设置** " 和 " **默认项目设置** " 对话框中可用。  
  
-   若要指定所有未来 SSMA 项目的设置，请在 " **工具** " 菜单上单击 " **默认项目设置**"，从 " **迁移目标版本** " 下拉框中选择需要查看或更改其设置的 "迁移项目类型"，然后单击左窗格底部的 " **类型映射** "。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上单击 " **项目设置**"，然后单击左窗格底部的 " **类型映射** "。  
  
若要指定当前对象或对象类的设置，请使用主 SSMA 窗口中的 " **类型映射** " 选项卡。  
  
## <a name="options"></a>选项  
下表显示了 " **类型映射** " 选项卡选项：  
  
**源类型**  
映射的 DB2 数据类型。  
  
**目标类型**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]指定 DB2 数据类型的目标数据类型。  
  
请参阅下一节中的表，了解用于 DB2 类型映射的默认 SSMA。  
  
**添加**  
单击此可将数据类型添加到 "映射" 列表中。  
  
**编辑**  
单击此项可在 "映射" 列表中编辑所选的数据类型。  
  
**删除**  
单击此选项可从 "映射" 列表中删除所选的数据类型映射。  
  
重置为默认值  
单击可将类型映射列表重置为 SSMA 默认值。  
  
## <a name="default-type-mappings"></a>默认类型映射  
在 SSMA for DB2 中，可以为自变量、列、局部变量和返回值设置自定义类型映射。 参数和返回类型的默认映射几乎完全相同。  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>默认参数类型和返回值类型映射  
下表包含自变量和返回值的默认数据类型映射。  
  
|DB2 数据类型|默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|  
|-----------------|-------------------------------------------------------------------------|  
|bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_integer|int|  
|blob|varbinary(max)|  
|boolean|bit|  
|char|varchar(max)|  
|char varying|varchar(max)|  
|character|varchar(max)|  
|character varying|varchar(max)|  
|clob|varchar(max)|  
|date|datetime2 [0]|  
|dec|dec [38] [0]|  
|Decimal|float [53]|  
|双精度|float [53]|  
|FLOAT|float [53]|  
|int|int|  
|整型|int|  
|long|varchar(max)|  
|长整型|varbinary(max)|  
|长整型 [ \* 。8000]<sup>\*</sup>|varbinary [ \* ]|  
|long raw [8001 \* ]<sup>\*</sup>|varbinary(max)|  
|国家字符|nvarchar(max)|  
|国家字符不同|nvarchar(max)|  
|国家字符|nvarchar(max)|  
|国家字符可变<sup>\*\*</sup>|nvarchar(max)|  
|国家字符可变<sup>\*</sup>|nvarchar(max)|  
|nchar|nvarchar(max)|  
|nclob|nvarchar(max)|  
|number|float [53]|  
|numeric|float [53]|  
|nvarchar2|nvarchar(max)|  
|pls_integer|int|  
|raw|varbinary(max)|  
|real|float [53]|  
|rowid|uniqueidentifier|  
|signtype|smallint|  
|smallint|smallint|  
|字符串|varchar(max)|  
|timestamp|datetime2|  
|带有本地时区的时间戳|datetimeoffset|  
|带时区的时间戳|datetimeoffset|  
|urowid|uniqueidentifier|  
|varchar|varchar(max)|  
|varchar2|varchar(max)|  
|xmltype|xml|  
  
<sup>\*</sup> 仅适用于返回值类型映射。  
  
<sup>\*\*</sup> 仅适用于参数类型映射。  
  
### <a name="default-column-type-mapping"></a>默认列类型映射  
下表包含列的默认类型映射。  
  
|DB2 数据类型|默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|  
|-----------------|-------------------------------------------------------------------------|  
|bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|blob|varbinary(max)|  
|char|char|  
|char 改变 [ \* ... \* ]|varchar [ \* ]|  
|char [ \* ... \* ]|char [ \* ]|  
|character|char|  
|字符变化 [ \* ... \* ]|varchar [ \* ]|  
|字符 [ \* ... \* ]|char [ \* ]|  
|clob|varchar(max)|  
|date|datetime2 [0]|  
|dec|dec [38] [0]|  
|dec [ \* ... \* ]|dec [ \* ] [0]|  
|dec [ \* ... \* ][\*..\*]|dec [ \* ] [ \* ]|  
|Decimal|decimal [38] [0]|  
|decimal [ \* ... \* ]|decimal [ \* ] [0]|  
|decimal [ \* ... \* ][\*..\*]|decimal [ \* ] [ \* ]|  
|双精度|float [53]|  
|FLOAT|float [53]|  
|float [ \* .。53]|float [ \* ]|  
|float [54 ... \* ]|float [53]|  
|int|int|  
|整型|int|  
|long|varchar(max)|  
|长整型|varbinary(max)|  
|长整型 [ \* 。8000]|varbinary [ \* ]|  
|long raw [8001 \* ]|varbinary(max)|  
|长 varchar|varchar(max)|  
|long [ \* .。8000]|varchar [ \* ]|  
|long [8001 \* ]|varchar(max)|  
|国家字符|nchar|  
|国家字符变化 [ \* ... \* ]|nvarchar [ \* ]|  
|国家字符 [ \* .. \* ]|nchar [ \* ]|  
|国家字符|nchar|  
|国家字符变化 [ \* ... \* ]|nvarchar [ \* ]|  
|国家字符 [ \* ... \* ]|nchar [ \* ]|  
|nchar|nchar|  
|nchar [ \* ]|nchar [ \* ]|  
|nclob|nvarchar(max)|  
|number|float [53]|  
|number [ \* ... \* ]|数值 [ \* ]|  
|number [ \* ... \* ][\*..\*]|numeric [ \* ] [ \* ]|  
|numeric|numeric|  
|数值 [ \* ... \* ]|数值 [ \* ]|  
|数值 [ \* ... \* ][\*..\*]|numeric [ \* ] [ \* ]|  
|nvarchar2 [ \* ... \* ]|nvarchar [ \* ]|  
|raw [ \* ... \* ]|varbinary [ \* ]|  
|real|float [53]|  
|rowid|uniqueidentifier|  
|smallint|smallint|  
|timestamp|datetime2|  
|带有本地时区的时间戳|datetimeoffset|  
|带有本地时区的时间戳 [ \* ... \* ]|datetimeoffset [ \* ]|  
|带时区的时间戳|datetimeoffset|  
|带有时区的时间戳 [ \* ... \* ]|datetimeoffset [ \* ]|  
|timestamp [ \* ... \* ]|datetime2 [ \* ]|  
|Urowid|uniqueidentifier|  
|urowid [ \* ... \* ]|uniqueidentifier|  
|varchar [ \* ... \* ]|varchar [ \* ]|  
|varchar2 [ \* ... \* ]|varchar [ \* ]|  
|Xmltype|xml|  
  
### <a name="default-local-variable-type-mapping"></a>默认局部变量类型映射  
下表包含本地变量的默认类型映射。  
  
|DB2 数据类型|默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|  
|-----------------|-------------------------------------------------------------------------|  
|Bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_interger|int|  
|Blob|varbinary(max)|  
|布尔|bit|  
|Char|char|  
|char 改变 [ \* .。8000]|varchar [ \* ]|  
|char 改变 [8001 \* ]|varchar(max)|  
|char [ \* .。8000]|char [ \* ]|  
|char [8001 \* ]|varchar(max)|  
|字符|char|  
|字符变化 [ \* 。8000]|varchar [ \* ]|  
|字符变化 [8001 \* ]|varchar(max)|  
|字符 [ \* .。8000]|char [ \* ]|  
|字符 [8001 \* ]|varchar(max)|  
|clob|varchar(max)|  
|date|datetime2 [0]|  
|dec|dec [38] [0]|  
|dec [ \* ... \* ]|dec [ \* ] [0]|  
|dec [ \* ... \* ][\*..\*]|dec [ \* ] [ \* ]|  
|Decimal|decimal [38] [0]|  
|decimal [ \* ... \* ]|decimal [ \* ] [0]|  
|decimal [ \* ... \* ][\*..\*]|decimal [ \* ] [ \* ]|  
|双精度|float [53]|  
|Float|float [53]|  
|float [ \* .。53]|float [ \* ]|  
|float [54 ... \* ]|float [53]|  
|int|int|  
|Integer|int|  
|integer [ \* ... \* ]|数值 [ \* ] [0]|  
|Long|varchar(max)|  
|长整型|varbinary(max)|  
|长整型 [ \* 。8000]|varbinary [ \* ]|  
|long raw [8001 \* ]|varbinary(max)|  
|国家字符|nchar|  
|国家字符变化 [ \* 。4000]|nvarchar [ \* ]|  
|国家字符变化 [4001 ... \* ]|nvarchar(max)|  
|国家字符 [ \* .。4000]|nchar [ \* ]|  
|国家字符 [4001 ... \* ]|nvarchar(max)|  
|国家字符|nchar|  
|国家字符 [ \* .。4000]|nvarchar [ \* ]|  
|国家字符 [4001 ... \* ]|nvarchar(max)|  
|国家字符变化 [ \* 。4000]|nvarchar [ \* ]|  
|国家字符变化 [4001 ... \* ]|nvarchar(max)|  
|Nchar|nchar|  
|nchar [ \* .。。4000]|nchar [ \* ]|  
|nchar [4001 ... \* ]|nvarchar(max)|  
|nchar 不同 [ \* 。4000]|nvarchar [ \* ]|  
|nchar 不同 [4001 ... \* ]|nvarchar(max)|  
|Nclob|nvarchar(max)|  
|数字|float [53]|  
|number [ \* ... \* ]|数值 [ \* ]|  
|number [ \* ... \* ][\*..\*]|numeric [ \* ] [ \* ]|  
|Numeric|数值 [38] [0]|  
|数值 [ \* ... \* ]|数值 [ \* ]|  
|数值 [ \* ... \* ][\*..\*]|numeric [ \* ] [ \* ]|  
|nvarchar2 [ \* .。4000]|nvarchar [ \* ]|  
|nvarchar2 [4001 ... \* ]|nvarchar(max)|  
|pls_integer|int|  
|raw [ \* .。。8000]|varbinary [ \* ]|  
|raw [8001 \* ]|varbinary(max)|  
|Real|float [53]|  
|Rowid|uniqueidentifier|  
|Signtype|smallint|  
|Smallint|smallint|  
|string [ \* .。8000]|varchar [ \* ]|  
|string [8001 \* ]|varchar(max)|  
|timestamp|datetime2|  
|带有本地时区的时间戳|datetimeoffset|  
|带时区的时间戳|datetimeoffset|  
|带有本地时区的时间戳 [ \* ... \* ]|datetimeoffset [ \* ]|  
|带有时区的时间戳 [ \* ... \* ]|datetimeoffset [ \* ]|  
|timestamp [ \* ... \* ]|datetime2 [ \* ]|  
|Urowid|uniqueidentifier|  
|urowid [ \* ... \* ]|uniqueidentifier|  
|varchar [ \* .。。8000]|varchar [ \* ]|  
|varchar [8001 \* ]|varchar(max)|  
|varchar2 [ \* .。8000]|varchar [ \* ]|  
|varchar2 [8001 \* ]|varcha (最大) |  
|Xmltype|xml|  
  
## <a name="see-also"></a>另请参阅  
[用户界面参考 &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
