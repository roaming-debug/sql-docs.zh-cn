---
description: sp_sequence_get_range (Transact-SQL)
title: sp_sequence_get_range (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sequence_get_range
- sp_sequence_get_range_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sp_sequence_get_range procedure
- sp_sequence_get_range
ms.assetid: 8ca6b0c6-8d9c-4eee-b02f-51ddffab4492
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 23ca83a62a194bb30bbf7a9b1432e42525592f3c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744637"
---
# <a name="sp_sequence_get_range-transact-sql"></a>sp_sequence_get_range (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  从序列对象中返回一系列序列值。 序列对象生成和发出请求的值数目，并为应用程序提供与该系列序列值相关的元数据。  
  
 有关序列号的详细信息，请参阅 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_sequence_get_range [ @sequence_name = ] N'<sequence>'   
     , [ @range_size = ] range_size  
     , [ @range_first_value = ] range_first_value OUTPUT   
    [, [ @range_last_value = ] range_last_value OUTPUT ]  
    [, [ @range_cycle_count = ] range_cycle_count OUTPUT ]  
    [, [ @sequence_increment = ] sequence_increment OUTPUT ]  
    [, [ @sequence_min_value = ] sequence_min_value OUTPUT ]  
    [, [ @sequence_max_value = ] sequence_max_value OUTPUT ]  
    [ ; ]  
```  
  
## <a name="arguments"></a>参数  
`[ @sequence_name = ] N'sequence'` 序列对象的名称。 架构是可选的。 *sequence_name* **(776) 为 nvarchar**。  
  
`[ @range_size = ] range_size` 要从序列中提取的值的数目。 **\@ range_size** 为 **bigint**。  
  
`[ @range_first_value = ] range_first_value` Output 参数返回用于计算所请求范围的序列对象的第一个 (最小值或最大) 值。 与请求中使用的序列对象相同的基类型 **sql_variant** **\@ range_first_value** 。  
  
`[ @range_last_value = ] range_last_value` 可选的输出参数将返回所请求的范围的最后一个值。 与请求中使用的序列对象相同的基类型 **sql_variant** **\@ range_last_value** 。  
  
`[ @range_cycle_count = ] range_cycle_count` 可选的 output 参数返回序列对象为了返回所请求的范围而循环的次数。 **\@ range_cycle_count** 是 **int**。  
  
`[ @sequence_increment = ] sequence_increment` 可选的 output 参数返回用于计算所请求范围的序列对象的增量。 与请求中使用的序列对象相同的基类型 **sql_variant** **\@ sequence_increment** 。  
  
`[ @sequence_min_value = ] sequence_min_value` 可选的输出参数返回序列对象的最小值。 与请求中使用的序列对象相同的基类型 **sql_variant** **\@ sequence_min_value** 。  
  
`[ @sequence_max_value = ] sequence_max_value` 可选的输出参数返回序列对象的最大值。 与请求中使用的序列对象相同的基类型 **sql_variant** **\@ sequence_max_value** 。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 sys 中的 sp_sequence_get_rangeis。 架构，可以作为 sys.sp_sequence_get_range 引用。  
  
### <a name="cycling-sequences"></a>循环序列  
 如果需要，序列对象将循环相应的次数以处理请求的范围。 将通过 `@range_cycle_count` 参数向调用方返回循环次数。  
  
> [!NOTE]  
>  在进行循环时，序列对象从序列对象最小值（升序）或最大值（降序）重新开始，而不是从序列对象的开始值重新开始。  
  
### <a name="non-cycling-sequences"></a>非循环序列  
 如果请求的范围中的值数目大于序列对象中的剩余可用值数目，并不会从序列对象中缩减请求的范围，将返回下面的错误 11732：  
  
 `The requested range for sequence object '%.*ls' exceeds the maximum or minimum limit. Retry with a smaller range.`  
  
## <a name="permissions"></a>权限  
 要求对序列对象或序列对象架构具有 UPDATE 权限。  
  
## <a name="examples"></a>示例  
 下面的示例使用一个名为 RangeSeq 的序列对象。 使用以下语句创建 RangeSeq 序列。  
  
```  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.RangeSeq  
    AS int   
    START WITH 1  
    INCREMENT BY 1  
    MINVALUE 1  
    MAXVALUE 25  
    CYCLE  
    CACHE 10  
;  
```  
  
### <a name="a-retrieving-a-range-of-sequence-values"></a>A. 检索序列值的范围  
 以下语句从 RangeSeq 序列对象中获取四个序列号，并向用户返回第一个编号。  
  
```  
DECLARE @range_first_value_output sql_variant ;  
  
EXEC sys.sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 4  
, @range_first_value = @range_first_value_output OUTPUT ;  
  
SELECT @range_first_value_output AS FirstNumber ;  
  
```  
  
### <a name="b-returning-all-output-parameters"></a>B. 返回所有输出参数  
 下面的示例返回 sp_sequence_get_range 过程中的所有输出值。  
  
```  
DECLARE    
  @FirstSeqNum sql_variant  
, @LastSeqNum sql_variant  
, @CycleCount int  
, @SeqIncr sql_variant  
, @SeqMinVal sql_variant  
, @SeqMaxVal sql_variant ;  
  
EXEC sys.sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 5  
, @range_first_value = @FirstSeqNum OUTPUT   
, @range_last_value = @LastSeqNum OUTPUT   
, @range_cycle_count = @CycleCount OUTPUT  
, @sequence_increment = @SeqIncr OUTPUT  
, @sequence_min_value = @SeqMinVal OUTPUT  
, @sequence_max_value = @SeqMaxVal OUTPUT ;  
  
-- The following statement returns the output values  
SELECT  
  @FirstSeqNum AS FirstVal  
, @LastSeqNum AS LastVal  
, @CycleCount AS CycleCount  
, @SeqIncr AS SeqIncrement  
, @SeqMinVal AS MinSeq  
, @SeqMaxVal AS MaxSeq ;  
  
```  
  
 将 `@range_size` 参数更改为较大的数字（例如 75）将导致循环访问序列对象。 请检查 `@range_cycle_count` 参数以确定是否循环访问序列对象以及循环次数。  
  
### <a name="c-example-using-adonet"></a>C. 使用 ADO.NET 的示例  
 下面的示例使用 ADO.NET 从 RangeSeq 获取一个范围。  
  
```  
SqlCommand cmd = new SqlCommand();  
cmd.Connection = conn;  
cmd.CommandType = CommandType.StoredProcedure;  
cmd.CommandText = "sys.sp_sequence_get_range";  
cmd.Parameters.AddWithValue("@sequence_name", "Test.RangeSeq");  
cmd.Parameters.AddWithValue("@range_size", 10);  
  
// Specify an output parameter to retrieve the first value of the generated range.  
SqlParameter firstValueInRange = new SqlParameter("@range_first_value", SqlDbType.Variant);  
firstValueInRange.Direction = ParameterDirection.Output;  
cmd.Parameters.Add(firstValueInRange);  
  
conn.Open();  
cmd.ExecuteNonQuery();  
  
// Output the first value of the generated range.  
Console.WriteLine(firstValueInRange.Value);  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
