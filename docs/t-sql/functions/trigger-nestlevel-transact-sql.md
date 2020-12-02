---
description: TRIGGER_NESTLEVEL (Transact-SQL)
title: TRIGGER_NESTLEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRIGGER_NESTLEVEL
- TRIGGER_NESTLEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- triggers [SQL Server], number executed
- number of triggers
- TRIGGER_NESTLEVEL function
ms.assetid: 6a33e74a-0cf9-4ae1-a1e4-4a137a3ea39d
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 86554070c90b2c946e79359b561da324abbf700a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379532"
---
# <a name="trigger_nestlevel-transact-sql"></a>TRIGGER_NESTLEVEL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回为激发触发器的语句执行的触发器数。 TRIGGER_NESTLEVEL 在 DML 和 DDL 触发器中用以确定当前的嵌套级别。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TRIGGER_NESTLEVEL ( [ object_id ] , [ 'trigger_type' ] , [ 'trigger_event_category' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 object_id  
 触发器的对象 ID。 如果指定了 object_id，则返回为该语句执行指定触发器的次数。 如果未指定 object_id，则返回为该语句执行全部触发器的次数。  
  
 **'** *trigger_type* **'**  
 指定将 TRIGGER_NESTLEVEL 应用于 AFTER 触发器还是 INSTEAD OF 触发器。 为 AFTER 触发器指定 AFTER。 为 INSTEAD OF 触发器指定 IOT。 如果指定了 trigger_type，则必须指定 trigger_event_category。  
  
 **'** *trigger_event_category* **'**  
 指定将 TRIGGER_NESTLEVEL 应用于 DML 触发器还是 DDL 触发器。 为 DML 触发器指定 DML。 为 DDL 触发器指定 DDL。 如果指定了 trigger_event_category，则必须指定 trigger_type。 注意，由于 DDL 触发器只能是 AFTER 触发器，因此仅 AFTER 可以使用 DDL 指定。  
  
## <a name="remarks"></a>注解  
 如果未指定参数，则 TRIGGER_NESTLEVEL 返回调用堆栈上的触发器总数。 这包括它本身。 当触发器所执行的命令导致其他触发器激发，或导致触发器的连续激发时，可省略参数。  
  
 若要针对特殊触发器类型和事件类别返回调用堆栈上的触发器总数，请指定 object_id = 0。  
  
 如果 TRIGGER_NESTLEVEL 在触发器的外部执行，且任何参数均不为 NULL，则 TRIGGER_NESTLEVEL 返回 0。  
  
 如果将任何参数显式指定为 NULL，则无论在触发器内部还是外部使用 TRIGGER_NESTLEVEL，都将返回值 NULL。  
  
## <a name="examples"></a>示例  
  
### <a name="a-testing-the-nesting-level-of-a-specific-dml-trigger"></a>A. 测试特定 DML 触发器的嵌套级别  
  
```sql
IF ( (SELECT TRIGGER_NESTLEVEL( OBJECT_ID('xyz') , 'AFTER' , 'DML' ) ) > 5 )  
   RAISERROR('Trigger xyz nested more than 5 levels.',16,-1)  
```  
  
### <a name="b-testing-the-nesting-level-of-a-specific-ddl-trigger"></a>B. 测试特定 DDL 触发器的嵌套级别  
  
```sql
IF ( ( SELECT TRIGGER_NESTLEVEL ( ( SELECT object_id FROM sys.triggers  
WHERE name = 'abc' ), 'AFTER' , 'DDL' ) ) > 5 )  
   RAISERROR ('Trigger abc nested more than 5 levels.',16,-1)  
```  
  
### <a name="c-testing-the-nesting-level-of-all-triggers-executed"></a>C. 测试执行的所有触发器的嵌套级别  
  
```sql
IF ( (SELECT trigger_nestlevel() ) > 5 )  
   RAISERROR  
      ('This statement nested over 5 levels of triggers.',16,-1)  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
  
  
