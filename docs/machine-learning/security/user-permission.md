---
title: 授予执行 Python 和 R 脚本的权限
description: 了解如何向用户授予在 SQL Server 机器学习服务中运行外部 Python 和 R 脚本的权限，以及向数据库授予读取、写入或数据定义语言 (DDL) 权限。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperfq4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: d07154cdde04c173f1d775df9597f5ccdbc62e61
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470708"
---
# <a name="grant-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>授予用户在 SQL Server 机器学习服务中执行 Python 和 R 脚本的权限
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

了解如何向用户授予在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中运行外部 Python 和 R 脚本的权限，以及向数据库授予读取、写入或数据定义语言 (DDL) 权限。

有关详细信息，请参阅[扩展框架安全性概述](../../machine-learning/concepts/security.md#permissions)中的“权限”部分。

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>运行脚本的权限

对于每个在 SQL Server 机器学习服务中运行 Python 或 R 脚本的非管理员用户，必须授予他们在使用该语言的每个数据库中运行外部脚本的权限。

若要授予执行外部脚本的权限，请运行以下脚本：

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> 权限不特定于受支持的脚本语言。 换句话说，R 脚本和 Python 脚本没有单独的权限级别。

<a name="permissions-db"></a>

## <a name="grant-databases-permissions"></a>Grant 数据库权限

当某个用户运行脚本时，该用户可能需要从其他数据库读取数据。 此外，该用户可能还需要创建新表来存储结果，并将数据写入表中。

对于运行 R 或 Python 脚本的每个 Windows 用户帐户或 SQL 登录名，请确保它们具有特定数据库的适当权限： 

+ `db_datareader` 用以读取数据。
+ `db_datawriter` 将对象保存到数据库。
+ `db_ddladmin` 用以创建对象，例如包含训练数据和序列化数据的存储过程或表。

例如，以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句为 SQL 登录名 MySQLLogin 提供在 ML_Samples 数据库中运行 T-SQL 查询的权限 。 若要运行此语句，SQL 登录名必须已经存在于服务器的安全上下文中。

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>后续步骤

若要详细了解每个角色包括的权限，请参阅 [数据库级别的角色](../../relational-databases/security/authentication-access/database-level-roles.md)。
