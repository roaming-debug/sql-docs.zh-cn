---
description: DROP EXTERNAL LIBRARY (Transact-SQL)
title: DROP EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- DROP EXTERNAL LIBRARY
- DROP_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3b7e6509cabb65a9e6dd09a6ccbe76f33bff5bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346596"
---
# <a name="drop-external-library-transact-sql"></a>DROP EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

删除现有包库。 包库供受支持的外部运行时（例如 R、Python 或 Java）使用。

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> 在 SQL Server 2017 中，支持 R 语言和 Windows 平台。 SQL Server 2019 及更高版本支持 Windows 和 Linux 平台上的 R、Python 和 Java。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> 在 Azure SQL 托管实例中，支持 R 和 Python 语言。
::: moniker-end

## <a name="syntax"></a>语法

```syntaxsql
DROP EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ];
```

### <a name="arguments"></a>参数

**library_name**

指定现有包库的名称。

库的应用范围限定为用户。 在特定用户或所有者的上下文中，库名称必须是唯一的。

**owner_name**

指定拥有外部库的用户或角色的名称。

数据库所有者可以删除由其他用户创建的库。

## <a name="permissions"></a>权限

必须拥有 ALTER ANY EXTERNAL LIBRARY 权限，才能删除库。 默认情况下，任何数据库所有者或对象所有者也可以删除外部库。

### <a name="return-values"></a>返回值

如果语句成功，则返回信息性消息。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注

不同于 SQL Server 中的其他 `DROP` 语句，此语句支持指定一个可选的授权子句。 这允许 db_owner 角色中的 dbo 或用户删除由数据库的常规用户上传的包库 。

许多包（称为“系统包”）都预安装在 SQL 实例中。 用户无法添加、更新或删除系统包。

## <a name="examples"></a>示例

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
将自定义 R 包 `customPackage` 添加到数据库：

```sql
CREATE EXTERNAL LIBRARY customPackage 
FROM (CONTENT = 'C:\temp\customPackage_v1.1.zip')
WITH (LANGUAGE = 'R');
GO
```
::: moniker-end

删除 `customPackage` 库。

```sql
DROP EXTERNAL LIBRARY customPackage;
```

## <a name="see-also"></a>另请参阅

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  
