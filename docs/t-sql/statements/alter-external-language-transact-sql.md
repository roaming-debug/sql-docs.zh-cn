---
description: ALTER EXTERNAL LANGUAGE (Transact-SQL) - SQL Server
title: ALTER EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: bca91a8745a36c845fd3ef7e08e1d0139b37bd1a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464148"
---
# <a name="alter-external-language-transact-sql"></a>ALTER EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

修改数据库中现有外部语言扩展的内容。

## <a name="syntax"></a>语法

```syntaxsql
ALTER EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]
{
    SET <file_spec>
    | ADD <file_spec>
    | REMOVE <file_spec>
}
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = {<external_lang_specifier> | <content_bits>,
    FILE_NAME = <external_lang_file_name>
    [, PLATFORM = <platform> ]
    [, PARAMETERS = <external_lang_parameters> ]
    [, ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
   | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'

<platform> :: =
{
   WINDOWS
  | LINUX
}

< external_lang_parameters > :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>参数

**language_name**

语言是数据库范围的对象。 语言名称在数据库中必须唯一。

**owner_name**

指定拥有外部语言的用户或角色的名称。 如果未指定，则所有权授予当前用户。 可能需要为其他用户授予使用特定语言执行脚本的显式权限，具体取决于用户权限。

**file_spec**

指定语言扩展的内容。每个平台每种特定语言只允许使用一个 filespec。 

**external_lang_specifier**

包含扩展代码的 .zip 或 tar.gz 文件的完整文件路径。 此内容可以是 .zip 文件（在 Windows 上）或 tar.gz（在 Linux 上）的路径。

**content_bits**

将语言的内容指定为十六进制文字，类似于程序集。
如果需要创建语言或更改现有语言（并具有执行此操作的所需权限），但服务器上的文件系统受限，无法将库文件复制到服务器可以访问的位置，此选项会非常有用。

**external_lang_file_name**

扩展 .dll 或 .so 文件的名称。 如果 <external_lang_specifier> .zip 或 tar.gz 中有多个 .dll 或 .so 文件，需要使用名称来识别正确的文件。

**external_lang_parameters**

这提供了向外部语言运行时提供一组参数的可能性。 外部进程启动后，会将参数值提供给外部运行时。 但是，在外部进程启动之前，语言扩展可以访问环境变量。

**external_lang_env_variables**

这提供了在外部进程启动之前向外部语言运行时提供一组环境变量的可能性。 环境变量的一个例子是运行时本身的主目录。 例如：JRE_HOME。

**平台**

混合 OS 方案需要此参数。 在混合体系结构中，语言需要按平台进行一次注册。 平台和语言名称对每种外部语言是唯一键。 如果未指定平台，则假定为当前 OS。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注

目前不支持 PARAMETERS 和 ENVIRONMENT_VARIABLES   。

## <a name="permissions"></a>权限

需要 `ALTER ANY EXTERNAL LANGUAGE` 权限。 默认情况下，dbo 用户或担任 db_owner 角色的任何成员都有权更改外部语言   。 对于其他所有用户，必须使用 [GRANT](./grant-database-permissions-transact-sql.md) 语句显式授予他们权限，同时将 ALTER ANY EXTERNAL LANGUAGE 指定为特权。

## <a name="examples"></a>示例

### <a name="alter-an-external-language-in-a-database"></a>更改数据库中的外部语言  

以下示例将一种名为 Java 的外部语言添加到 Windows 上的 SQL Server 的数据库中。

```sql
ALTER EXTERNAL LANGUAGE Java 
SET (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

## <a name="see-also"></a>另请参阅

[CREATE EXTERNAL LANGUAGE (Transact-SQL)](create-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)