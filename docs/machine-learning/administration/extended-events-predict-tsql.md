---
title: 用扩展事件监视 T-SQL
description: 了解如何在 SQL Server 机器学习服务中使用扩展事件监视和排除 PREDICT T-SQL 语句。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/24/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6376795beeeddc9ec9c6140c10a0c2831e5368c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471368"
---
# <a name="monitor-predict-t-sql-statements-with-extended-events-in-sql-server-machine-learning-services"></a>在 SQL Server 机器学习服务中使用扩展事件监视 PREDICT T-SQL 语句
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

了解如何在 SQL Server 机器学习服务中使用扩展事件监视和排除 [PREDICT](../../t-sql/queries/predict-transact-sql.md) T-SQL 语句。

## <a name="table-of-extended-events"></a>扩展事件表

以下扩展事件在所有支持 [PREDICT](../../t-sql/queries/predict-transact-sql.md) T-SQL 语句的 SQL Server 版本中都可用。 

| name                       | object_type | description |
|----------------------------|-------------|-------------|
| predict_function_completed | event       | 内置执行时间细分|
| predict_model_cache_hit    | event       | 从 PREDICT 函数模型缓中存检索模型时发生。 可将此事件与其他 predict_model_cache_* 事件结合使用来解决由 PREDICT 函数模型缓存引起的问题。|
| predict_model_cache_insert | event       | 将模型插入 PREDICT 函数模型缓存时发生。 可将此事件与其他 predict_model_cache_* 事件结合使用来解决由 PREDICT 函数模型缓存引起的问题。   |
| predict_model_cache_miss   | event       | 在 PREDICT 函数模型缓存中找不到模型时发生。 频繁出现此事件可能表示 SQL Server 需要更多内存。 可将此事件与其他 predict_model_cache_* 事件结合使用来解决由 PREDICT 函数模型缓存引起的问题。|
| predict_model_cache_remove | event       | 从 PREDICT 函数的模型缓存中删除模型时发生。 可将此事件与其他 predict_model_cache_* 事件结合使用来解决由 PREDICT 函数模型缓存引起的问题。|

## <a name="query-for-related-events"></a>查询相关事件

若要查看为这些事件返回的所有列的列表，请在 SQL Server Management Studio 中运行以下查询：

```sql
SELECT *
FROM sys.dm_xe_object_columns
WHERE object_name LIKE 'predict%'
```

## <a name="examples"></a>示例

要使用 PREDICT 捕获有关评分会话性能的信息，请执行以下操作：

1. 使用 Management Studio 或其他受支持的[工具](../../relational-databases/extended-events/extended-events-tools.md)创建新的扩展事件会话。
2. 将事件 `predict_function_completed` 和 `predict_model_cache_hit` 添加到会话中。
3. 启动扩展事件会话。
4. 运行使用 PREDICT 的查询。

在“结果”中，查看以下列：

+ `predict_function_completed` 的值显示查询在加载模型和评分上花费的时间。
+ `predict_model_cache_hit` 的布尔值指示查询是否使用缓存的模型。 

### <a name="native-scoring-model-cache"></a>本机评分模型缓存

除特定于 PREDICT 的事件之外，还可使用以下查询来获得有关缓存模型和缓存使用情况的更多信息：

查看本机评分模型缓存：

```sql
SELECT *
FROM sys.dm_os_memory_clerks
WHERE type = 'CACHESTORE_NATIVESCORING';
```

查看模型缓存中的对象：

```sql
SELECT *
FROM sys.dm_os_memory_objects
WHERE TYPE = 'MEMOBJ_NATIVESCORING';
```

## <a name="next-steps"></a>后续步骤

有关扩展事件（有时称为 XEvent）以及如何在会话中跟踪事件的更多信息，请参阅以下文章：

+ [在 SQL Server 机器学习服务中使用扩展事件监视 Python 和 R 脚本](extended-events.md)
+ [扩展事件的概念和体系结构](../../relational-databases/extended-events/extended-events.md)
+ [在 SSMS 中设置事件捕获](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)
+ [在对象资源管理器中管理事件会话](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)