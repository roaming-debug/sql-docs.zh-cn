---
title: 转换 Python 和 SQL 数据类型
description: 查看数据科学和机器学习解决方案中 Python 和 SQL Server 之间的隐式和显式数据类型转换。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cadb382861d868a96319483cbc54e847093fbe17
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471038"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Python 与 SQL Server 之间的数据类型映射
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

本文列出了在 SQL Server 机器学习服务中使用 Python 集成功能时所支持的数据类型以及所执行的数据类型转换。

与 SQL Server 相比，Python 支持的数据类型数量有限。 因此，每当在 Python 脚本中使用 SQL Server 中的数据时，SQL 数据都可能会隐式转换为兼容的 Python 数据类型。 但是，通常无法自动执行精确的转换并将返回错误。

## <a name="python-and-sql-data-types"></a>Python 和 SQL 数据类型

下表列出了提供的隐式转换。 不支持其他数据类型。

| SQL 类型             | Python 类型 | 说明 |
|----------------------|-------------|-------------|
| **bigint**           | `float64`   |
| **binary**           | `bytes`     |
| **bit**              | `bool`      |
| **char**             | `str`       |
| **date**             | `datetime`  |
| **datetime**         |`datetime`   | 支持 SQL Server 2017 CU6 及更高版本（具有 `datetime.datetime` 或 Pandas `pandas.Timestamp` 类型的 NumPy 数组 ）。 `sp_execute_external_script` 现在支持使用秒的小数形式的 `datetime` 类型。|
| **float**            | `float64`   |
| **nchar**            | `str`       |
| **nvarchar**         | `str`       |
| **nvarchar(max)**    | `str`       |
| **real**             | `float64`   |
| **smalldatetime**    | `datetime`  |
| **smallint**         | `int32`     |
| **tinyint**          | `int32`     |
| **uniqueidentifier** | `str`       |
| **varbinary**        | `bytes`     |
| **varbinary(max)**   | `bytes`     |
| **varchar(n)**       | `str`       |
| **varchar(max)**     | `str`       |

## <a name="see-also"></a>另请参阅

+ [R 与 SQL Server 之间的数据类型映射](../r/r-libraries-and-data-types.md)
