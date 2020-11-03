---
description: DROP WORKLOAD Classifier (Transact-SQL)
title: DROP WORKLOAD Classifier (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- DROP_WORKLOAD_CLASSIFIER_TSQL
- DROP WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DROP WORKLOAD CLASSIFIER statement
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest||=sqlallproducts-allversions
ms.openlocfilehash: 89fca3ce657b036578eb5f1b90e9b307799f4a95
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067399"
---
# <a name="drop-workload-classifier-transact-sql"></a>DROP WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

删除现有的用户定义的工作负载管理分类器。  如果请求正在运行或位于处于挂起状态的请求队列中，它们的分类保持不变，并且我们可以立即删除分类器。 删除和重新创建具有不同重要性的分类器不会影响已分类的请求。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  

```syntaxsql
DROP WORKLOAD CLASSIFIER classifier_name;
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数

classifier_name  
指定用于标识工作负荷分类器的名称。
  
## <a name="permissions"></a>权限

需要 CONTROL DATABASE 权限。  
  
## <a name="examples"></a>示例

以下示例删除名为 `wgcELTROLE` 的工作负荷分类器。  

```sql
DROP WORKLOAD CLASSIFIER wgcELTRole;
```

> [!NOTE]
> 在没有匹配分类器的情况下提交的请求将被分类到默认工作负荷组。  默认工作负荷组是 smallrc 资源类。
  
## <a name="see-also"></a>另请参阅

[CREATE WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-classifier-transact-sql.md)</br>
[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 工作负载分类](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)
