---
description: AT TIME ZONE (Transact-SQL)
title: AT TIME ZONE (Transact-SQL)
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AT TIME ZONE
- AT_TIME_ZONE_TSQL
helpviewer_keywords:
- AT TIME ZONE function
ms.assetid: 311f682f-7f1b-43b6-9ea0-24e36b64f73a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 391e045d078b2735411c51db73948e416f9e4ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159686"
---
# <a name="at-time-zone-transact-sql"></a>AT TIME ZONE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

将 inputdate 转换为目标时区中相应的 datetimeoffset 值。 如果所提供的 inputdate 没有偏移信息，则函数应用时区偏移时会假设 inputdate 值位于目标时区中 。 如果 inputdate 是作为 datetimeoffset 值提供的，则 AT TIME ZONE 子句会使用时区转换规则将其转换到目标时区中 。  

AT TIME ZONE 实现依赖于 Windows 机制来跨时区转换 datetime 值。  

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

## <a name="syntax"></a>语法

```syntaxsql
inputdate AT TIME ZONE timezone  
```

## <a name="arguments"></a>参数

inputdate  
一个表达式，可解析为 smalldatetime、datetime、datetime2 或 datetimeoffset 值   。  

timezone 目标时区的名称。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 依赖存储在 Windows 注册表中的时区。 计算机上安装的时区存储于以下注册表配置单元中：**KEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones**。 已安装的时区列表也会通过 [sys.time_zone_info (Transact-SQL)](../../relational-databases/system-catalog-views/sys-time-zone-info-transact-sql.md) 视图公开。  

## <a name="return-types"></a>返回类型

返回 datetimeoffset 的数据类型。

## <a name="return-value"></a>返回值

目标时区中的 datetimeoffset 值。
  
## <a name="remarks"></a>备注

AT TIME ZONE 应用特定规则来转换 smalldatetime、datetime 和 datetime2 数据类型中的输入值，这些值位于受 DST 更改影响的区间   ：

- 如果将时钟向前拨，将当地时间出现与时钟调整持续时间相等的时差。 此持续时间通常为 1 小时，但它也可以是 30 或 45 分钟，具体取决于所在的时区。 落入该时差范围的时间点会按 DST 更改后的偏移量进行转换。  

    ```sql
    /*  
        Moving to DST in "Central European Standard Time" zone: 
        offset changes from +01:00 -> +02:00   
        Change occurred on March 29th, 2015 at 02:00:00.   
        Adjusted local time became 2015-03-29 03:00:00.  
    */  

    --Time before DST change has standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T01:01:00', 126)     
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 01:01:00 +01:00   
  
    /*
      Adjusted time from the "gap interval" (between 02:00 and 03:00)
      is moved 1 hour ahead and presented with the summer time offset
      (after the DST change) 
    */
    SELECT CONVERT(DATETIME2(0), '2015-03-29T02:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00

    --Time after 03:00 is presented with the summer time offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T03:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00  
  
    ```

- 如果将时钟向后拨，则 2 个小时的本地时间将有 1 个小时的重叠。  在这种情况下，落入重叠范围的时间点会使用时钟更改之前的偏移量来调整：  
  
    ```sql
    /*  
        Moving back from DST to standard time in
        "Central European Standard Time" zone:
        offset changes from +02:00 -> +01:00.
        Change occurred on October 25th, 2015 at 03:00:00.
        Adjusted local time became 2015-10-25 02:00:00
    */  

    --Time before the change has DST offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T01:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 01:01:00 +02:00  

    /*
      Time from the "overlapped interval" is presented with standard time 
      offset (before the change)
    */
    SELECT CONVERT(DATETIME2(0), '2015-10-25T02:00:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 02:00:00 +02:00  


    --Time after 03:00 is regularly presented with the standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T03:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';
    --Result: 2015-10-25 03:01:00 +01:00
  
    ```

由于某些信息（例如时区规则）是在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 之外维护的，并且偶尔会发生变化，因此 AT TIME ZONE 函数被归类为非确定性函数。 

## <a name="examples"></a>示例

### <a name="a-add-target-time-zone-offset-to-datetime-without-offset-information"></a>A. 将目标时区偏移量添加到 datetime 而不提供偏移信息  

当已知同一时区中已提供原始 datetime 值时，使用 AT TIME ZONE 基于时区规则添加偏移量：  

```sql
USE AdventureWorks2016;
GO  
  
SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST  
FROM Sales.SalesOrderHeader;
```

### <a name="b-convert-values-between-different-time-zones"></a>B. 在不同时区之间转换值  

以下示例在不同时区之间转换值：  

```sql
USE AdventureWorks2016;
GO

SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST,
    OrderDate AT TIME ZONE 'Central European Standard Time' AS OrderDate_TimeZoneCET
FROM Sales.SalesOrderHeader;
```

### <a name="c-query-temporal-tables-using-a-specific-time-zone"></a>C. 使用特定时区查询时态表

以下示例演示使用太平洋标准时间从时态表中选择数据。  

```sql
USE AdventureWorks2016;
GO

DECLARE @ASOF datetimeoffset;  
SET @ASOF = DATEADD (month, -1, GETDATE()) AT TIME ZONE 'UTC';

-- Query state of the table a month ago projecting period
-- columns as Pacific Standard Time
SELECT BusinessEntityID, PersonType, NameStyle, Title,
    FirstName, MiddleName,
    ValidFrom AT TIME ZONE 'Pacific Standard Time'
FROM  Person.Person_Temporal
FOR SYSTEM_TIME AS OF @ASOF;
```

## <a name="next-steps"></a>后续步骤

- [日期和时间类型](../../t-sql/data-types/date-and-time-types.md)
- [日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)
