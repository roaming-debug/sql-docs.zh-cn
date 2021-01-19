---
description: DROP EXTERNAL RESOURCE POOL (Transact-SQL)
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: dphansen
ms.author: davidph
manager: cgronlund
ms.openlocfilehash: 08f25755ca4742b0e14379c40e22e99741eedbda
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172389"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

删除用于定义外部进程资源的 Resource Governor 外部资源池。 

::: moniker range="=sql-server-2016||>=sql-server-linux-ver15"
对于 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 中的[!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]，外部池控制 `rterm.exe`、`BxlServer.exe` 以及它们生成的其他进程。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
对于 [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]，外部池控制 `rterm.exe`、`python.exe`、`BxlServer.exe`，以及它们生成的其他进程。
::: moniker-end

外部资源池使用 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md) 创建，使用 [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) 修改。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

pool_name   
要删除的外部资源池的名称。  
  
## <a name="remarks"></a>备注

如果外部资源池包含工作负荷组，则不能删除该资源池。  

不能删除资源调控器默认池或内部池。  

建议您在熟悉资源调控器状态之后再执行 DDL 语句。 有关详细信息，请参阅 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)。  

## <a name="permissions"></a>权限

需要 `CONTROL SERVER` 权限。  

## <a name="examples"></a>示例

以下示例会删除名为 `ex_pool` 的外部资源池。  

```sql
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>另请参阅

+ [“已启用外部脚本”服务器配置选项](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP 工作负荷组 (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)
+ [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)
