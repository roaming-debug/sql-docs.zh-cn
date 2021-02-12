---
title: 将 MySQL 和 SQL Server 字符集映射 (MySQLToSQL) |Microsoft Docs
description: 了解如何为 mysql 字符数据类型、表达式和文本指定字符集，以便在 SSMA for MySQL 的字符数据类型转换期间使用。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 20b3f22e-16a2-4a87-b4eb-c277be6bf5c8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 13b2accd6b4dc2ba2bbd8c9c1e6db312650d2d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077618"
---
# <a name="mapping-mysql-and-sql-server-character-set-mysqltosql"></a>映射 MySQL 和 SQL Server 字符集 (MySQLToSQL)
字符集) 可以为 MySQL 字符数据类型、表达式和文本指定 (字符集。  
  
## <a name="charset-mapping"></a>字符集映射  
字符集映射为每个 MySQL 字符集定义，并在字符数据类型转换期间使用。 它指定如何转换特定字符集的字符串数据类型：  
  
-   到 (NCHAR/NVARCHAR) 的国家 SQL Server 字符类型，或  
  
-    (CHAR/VARCHAR) 的常规 SQL Server 字符类型  
  
1.  **国家** 目标数据库字符数据类型为：  
  
    1.  **nchar**  
  
    2.  **nvarchar**  
  
2.  **常规** 目标数据库字符数据类型为：  
  
    1.  **char**  
  
    2.  **varchar**  
  
3.  类型映射仅允许映射到 **国家** 字符数据类型。 根据类型映射转换 MySQL 字符数据类型后，将应用字符集映射。  
  
> [!NOTE]  
> 可以在元数据对象资源管理器的每个节点级别上定义字符集映射，并表示从 MySQL 读取的所有字符集。  
  
## <a name="charset-mapping-on-different-node-levels"></a>不同节点级别上的字符集映射  
字符集映射不同于不同的节点级别，即：  
  
1.  在根元数据节点级别上  
  
2.  "数据库"、"类别" 和 "对象节点" 级别  
  
> [!NOTE]  
> 选择用于编辑字符集映射的选项卡包含三个按钮，而不考虑不同节点级别上的映射。  
>   
> 它们分别是：  
>   
> 1.  **应用：** 应用用户所做的更改，仅在编辑字符集映射后启用。  
> 2.  **取消：** 取消用户所做的更改。 编辑了字符集映射但未保存时，会启用该按钮。  
> 3.  **重置为默认值：** 将所有映射重置为默认值。  
  
1.  **在根元数据节点级别上：**  字符集映射网格包含字符集网格，每个字符集对应一个单独的列。 网格的列包括：  
  
    1.  名为 **字符集名称** 的网格的第一列包含字符集名称。  
  
    2.  第二个名为 **字符集的说明** 包含字符集说明。  
  
    3.  第三列标题为 **Target 字符集类型** ，其中包含特定字符集的映射设置。 此列的值为：  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
    > [!IMPORTANT]  
    > 特定字符集的默认值具有 "CHAR/VARCHAR" 或 "NCHAR/NVARCHAR" 后的前缀 " (默认) "。  
  
    以下提供了 MySQL 数据库与根元数据节点级别上的目标数据库之间的字符集映射：  
  
    |字符集名称|字符集说明|目标字符集类型 (默认值) |  
    |-|-|-|  
    |big5|Big5 繁体中文|NCHAR/NVARCHAR (默认值) |  
    |dec8|12月西部欧洲|CHAR/VARCHAR (默认值) |  
    |cp850|DOS 西欧|CHAR/VARCHAR (默认值) |  
    |hp8|HP 西欧|CHAR/VARCHAR (默认值) |  
    |koi8r|KOI8-RU-R Relcom 俄语|CHAR/VARCHAR (默认值) |  
    |拉丁语1|cp1252 西欧|CHAR/VARCHAR (默认值) |  
    |latin2|ISO 8859-2 中欧|CHAR/VARCHAR (默认值) |  
    |swe7|7位瑞典语|CHAR/VARCHAR (默认值) |  
    |ascii|US ASCII|CHAR/VARCHAR (默认值) |  
    |ujis|EUC-日本日语|NCHAR/NVARCHAR (默认值) |  
    |启动|Shift-jis 日语|NCHAR/NVARCHAR (默认值) |  
    |希伯来语|ISO 8859-8 希伯来语|CHAR/VARCHAR (默认值) |  
    |tis620|TIS620 泰语|CHAR/VARCHAR (默认值) |  
    |euckr|EUC-KR 韩语|NCHAR/NVARCHAR (默认值) |  
    |koi8u|KOI8-RU 乌克兰语|CHAR/VARCHAR (默认值) |  
    |gb2312|GB2312 简体中文|NCHAR/NVARCHAR (默认值) |  
    |希腊|ISO 8859-7 希腊语|CHAR/VARCHAR (默认值) |  
    |cp 1250|Windows 中欧|CHAR/VARCHAR (默认值) |  
    |gbk|GBK 简体中文|NCHAR/NVARCHAR (默认值) |  
    |latin5|ISO 8859-9 土耳其语|CHAR/VARCHAR (默认值) |  
    |armscii8|ARMSCII-8 亚美尼亚语|CHAR/VARCHAR (默认值) |  
    |utf8|UTF-8 Unicode|NCHAR/NVARCHAR (默认值) |  
    |ucs2|UCS-2 Unicode|NCHAR/NVARCHAR (默认值) |  
    |cp866|DOS 俄语|CHAR/VARCHAR (默认值) |  
    |keybcs2|DOS Kamenicky Czech-Slovak|CHAR/VARCHAR (默认值) |  
    |macce|Mac 中欧|CHAR/VARCHAR (默认值) |  
    |macroman|Mac 西欧|CHAR/VARCHAR (默认值) |  
    |cp852|DOS 中欧|CHAR/VARCHAR (默认值) |  
    |latin7|ISO 8859-13 波罗的语|CHAR/VARCHAR (默认值) |  
    |cp 1251|Windows 西里尔语|CHAR/VARCHAR (默认值) |  
    |cp 1256|Windows 阿拉伯语|CHAR/VARCHAR (默认值) |  
    |cp 1257|Windows 波罗的语|CHAR/VARCHAR (默认值) |  
    |binary|二进制伪字符集|CHAR/VARCHAR (默认值) |  
    |geostd8|GEOSTD8 格鲁吉亚语|CHAR/VARCHAR (默认值) |  
    |cp932|适用于 Windows 日语的启动|NCHAR/NVARCHAR (默认值) |  
    |eucjpms|适用于 Windows 日语的 UJIS|NCHAR/NVARCHAR (默认值) |  
  
2.  **在数据库、类别或对象节点级别上：** 在 "数据库"、"类别" 或 "对象节点" 级别上，"字符集映射" 网格包含的行与根元数据节点级别上的相同，即。：  
  
    1.  标题为的网格的第一列 **包含字符集名称。**  
  
    2.  第二列标题为 " **字符集说明** "，其中包含字符集说明。  
  
    3.  唯一的区别是网格第三列中的值。 第三列标题为 " **目标数据类型** "，其中包含特定字符集的映射设置。 列的值为：  
  
        -   继承 (CHAR/VARCHAR 或 NCHAR/NVARCHAR)   
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
> [!IMPORTANT]  
> -   在 "数据库"、"类别" 和 "对象" 节点级别上 MySQL 数据库和目标数据库之间的字符集映射中，列 **目标数据类型** 的根以外的每个级别上的特定字符集的默认值应为 "继承"。  
> -   在网格中， **继承** 的值以 " (CHAR/VARCHAR) " 或 " (NCHAR/NVARCHAR) " 为后缀，具体取决于该特定字符集从此父字符集继承的值。  
  
