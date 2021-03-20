---
description: SET ANSI_DEFAULTS (Transact-SQL)
title: SET ANSI_DEFAULTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET ANSI_DEFAULTS
- ANSI_DEFAULTS
- SET_ANSI_DEFAULTS_TSQL
- ANSI_DEFAULTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ANSI_DEFAULTS option
- SET ANSI_DEFAULTS statement
ms.assetid: bd721d97-6e23-488b-8c8c-c0453d5b3b86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 552fb5a2de6220f46b2fb2b0ff9aa036b0c10a92
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755997"
---
# <a name="set-ansi_defaults-transact-sql"></a>SET ANSI_DEFAULTS (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  控制一组可共同指定某种 ISO 标准行为的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>[!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 的语法
```syntaxsql
SET ANSI_DEFAULTS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>[!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] 和 [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)] 的语法
```syntaxsql
SET ANSI_DEFAULTS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
ANSI_DEFAULTS 是服务器端设置，可以为所有客户端连接启用此行为。 客户端通常请求连接或会话初始化的设置。 用户不应修改服务器设置。   
若要更改客户端的行为，用户应该使用特定于客户端的方法，如 `SQL_COPT_SS_PRESERVE_CURSORS`。 有关详细信息，请参阅 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)。
  
当启用 (ON) 时，此选项将启用下列 ISO 设置：  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET QUOTED_IDENTIFIER
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

这些 ISO 标准 SET 选项共同定义了用户工作会话、运行触发器或存储过程执行期间的查询处理环境。 不过，这些 SET 选项并未包括遵守 ISO 标准所需的所有选项。  
  
在处理计算列和索引视图上的索引时，必须将这四个默认选项（`ANSI_NULLS`、`ANSI_PADDING`、`ANSI_WARNINGS` 和 `QUOTED_IDENTIFIER`）设置为 ON。 这些默认选项是七个 SET 选项中的一部分，当您在计算列和索引视图上创建和更改索引时，必须给这七个选项分配必需的值。 其他 SET 选项 `ARITHABORT` (ON)、`CONCAT_NULL_YIELDS_NULL` (ON) 和 `NUMERIC_ROUNDABORT` (OFF)。 有关计算列上的索引视图和索引所必需的 SET 选项设置的详细信息，请参阅[使用 SET 语句时的注意事项](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements)。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序在连接时会自动将 ANSI_DEFAULTS 设置为 ON。 然后，驱动程序和访问接口将 CURSOR_CLOSE_ON_COMMIT 和 IMPLICIT_TRANSACTIONS 设置为 OFF。 `CURSOR_CLOSE_ON_COMMIT` 和 `IMPLICIT_TRANSACTIONS` 的 OFF 设置可以在 ODBC 数据源、ODBC 连接属性或 OLE DB 连接属性（它们在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前在应用程序中设置）中进行配置。 对于 `ANSI_DEFAULTS` 应用程序的连接，ANSI_DEFAULTS 默认为 OFF。  
  
当发出 SET ANSI_DEFAULTS 时，QUOTED_IDENTIFIER 将在分析时进行设置，而下列选项则在执行时进行设置：  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::

## <a name="permissions"></a>权限  
要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
以下示例将 ANSI_DEFAULTS 设置为 ON，并使用 `DBCC USEROPTIONS` 语句显示受影响的设置。  
  
```sql  
-- SET ANSI_DEFAULTS ON.  
SET ANSI_DEFAULTS ON;  
GO  

-- Display the current settings.  
DBCC USEROPTIONS;  
GO 

-- SET ANSI_DEFAULTS OFF.  
SET ANSI_DEFAULTS OFF;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DBCC USEROPTIONS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)   
 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
