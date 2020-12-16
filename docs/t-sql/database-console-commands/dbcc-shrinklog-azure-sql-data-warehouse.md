---
description: DBCC SHRINKLOG（并行数据仓库）
title: DBCC SHRINKLOG（并行数据仓库）
ms.custom: ''
ms.date: 03/16/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
author: pmasl
ms.author: umajay
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 58bda1e035c25ae373fef14bfb574a6f2c139835
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480418"
---
# <a name="dbcc-shrinklog-parallel-data-warehouse"></a>DBCC SHRINKLOG（并行数据仓库）

[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

减少当前  *数据库在设备中的事务日志大小*[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]。 已对数据进行碎片整理，以便收缩事务日志。 随着时间推移，数据库事务日志可能会变得零碎且效率低下。 使用 DBCC SHRINKLOG 减少碎片并减小日志大小。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC SHRINKLOG   
    [ ( SIZE = { target_size [ MB | GB | TB ]  } | DEFAULT ) ]   
    [ WITH NO_INFOMSGS ]   
[;]  
```  

## <a name="arguments"></a>参数

SIZE = { target_size [MB \| GB \| TB]} \| DEFAULT 。  
target_size 是 DBCC SHRINKLOG 完成后所有计算节点上所需的事务日志大小  。 它必须是大于 0 的整数。  
日志大小的单位为兆字节 (MB)、千兆字节 (GB) 或兆兆字节 (TB)。 它是所有计算节点上事务日志的组合大小。  
默认情况下，DBCC SHRINKLOG 可将事务日志减小到数据库的元数据中存储的日志大小。 元数据中的日志大小是由 [CREATE DATABASE (Azure Synapse Analytics)](../statements/create-database-transact-sql.md) 或 [ALTER DATABASE (Azure Synapse Analytics)](../statements/alter-database-transact-sql.md) 中的 LOG_SIZE 参数确定的。 已指定 `SIZE=DEFAULT` 或省略 `SIZE` 子句时，DBCC SHRINKLOG 可将事务日志大小减小到默认大小。
  
WITH NO_INFOMSGS  
DBCC SHRINKLOG 结果中不显示信息性消息。  
  
## <a name="permissions"></a>权限

需要 ALTER SERVER STATE 权限。

## <a name="general-remarks"></a>一般备注

DBCC SHRINKLOG 不会更改数据库的元数据中存储的日志大小。 元数据会继续包含 CREATE DATABASE 或 ALTER DATABASE 语句中指定的 LOG_SIZE 参数。
  
## <a name="examples"></a>示例

### <a name="a-shrink-the-transaction-log-to-the-original-size-specified-by-create-database"></a>A. 将事务日志收缩到 CREATE DATABASE 指定的原始大小。  
假设创建 Addresses 数据库时，Addresses 数据库的事务日志已设置为 100 MB。 即 Addresses 的 CREATE DATABASE 语句的 LOG_SIZE = 100 MB。 现在，假设日志大小已增加到 150 MB，而你希望将其收缩到 100 MB。
  
以下每个语句都会尝试将 Addresses 数据库的事务日志收缩到 100 MB 的默认大小。 如果将日志收缩到 100 MB 会导致数据丢失，DBCC SHRINKLOG 会尽可能地将日志收缩到最小（大于 100 MB），而不会丢失数据。

```sql
USE Addresses;  
DBCC SHRINKLOG ( SIZE = 100 MB );  
DBCC SHRINKLOG ( SIZE = DEFAULT );  
DBCC SHRINKLOG;  
```