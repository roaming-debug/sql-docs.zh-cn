---
description: CREATE DIAGNOSTICS SESSION (Transact-SQL)
title: CREATE DIAGNOSTICS SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 662d019e-f217-49df-9e2f-b5662fa0342d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 731c7b77fca5ae7c2cc4f85e3387d3daf93b4bce
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124435"
---
# <a name="create-diagnostics-session-transact-sql"></a>CREATE DIAGNOSTICS SESSION (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  使用诊断会话可以保存有关系统或查询性能的详细用户定义诊断信息。  
  
 诊断会话通常用于针对特定查询调试性能，或在设备操作期间监视特定设备组件的行为。  
  
> [!NOTE]  
>  使用诊断会话需要熟知 XML。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Creating a new diagnostics session:  
CREATE DIAGNOSTICS SESSION diagnostics_name AS N'{<session_xml>}';  
  
<session_xml>::  
<Session>  
   [ <MaxItemCount>max_item_count_num</MaxItemCount> ]  
   <Filter>  
      { \<Event Name="event_name"/>  
         [ <Where>\<filter_property_name Name="value" ComparisonType="comp_type"/></Where> ] [ ,...n ]  
      } [ ,...n ]  
   </Filter> ]   
   <Capture>  
      \<Property Name="property_name"/> [ ,...n ]  
   </Capture>  
<Session>  
  
-- Retrieving results for a diagnostics session:  
SELECT * FROM master.sysdiag.diagnostics_name ;  
  
-- Removing results for a diagnostics session:  
DROP DIAGNOSTICS SESSION diagnostics_name ;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 diagnostics_name  
 诊断会话的名称。 诊断会话名称只能包含 a-z、A-Z 和 0-9 的字符。 此外，诊断会话名称必须以字符开头。 diagnostics_name 限制在 127 个字符以内。  
  
 max_item_count_num  
 要在视图中保留的事件数。 例如，如果指定 100，则将在诊断会话中保留 100 个匹配筛选条件的最新事件。 如果找到的匹配事件少于 100 个，则诊断会话将包含少于 100 个事件。 max_item_count_num 必须至少为 100 且小于或等于 100,000。  
  
 event_name  
 定义要在诊断会话中收集的实际事件。  event_name 是 [sys.pdw_diag_events](../../relational-databases/system-catalog-views/sys-pdw-diag-events-transact-sql.md)（其中 `sys.pdw_diag_events.is_enabled='True'`）中列出的事件之一。  
  
 filter_property_name  
 基于其限制结果的属性名称。 例如，如果想要基于会话 ID 实施限制，则 filter_property_name 应为 SessionId。 有关 filter_property_name 的可能值的列表，请参阅下面的 property_name。  
  
 *value*  
 用于针对 filter_property_name 进行计算的值。 值类型必须与属性类型相匹配。 例如，如果属性类型是十进制，则值类型必须为十进制。  
  
 comp_type  
 比较类型。 可能值包括：Equals、EqualsOrGreaterThan、EqualsOrLessThan、GreaterThan、LessThan、NotEquals、Contains、RegEx  
  
 property_name  
 与事件相关的属性。  属性名称可以是捕获标记的一部分，或用作筛选条件的一部分。  
  
|属性名称|描述|  
|-------------------|-----------------|  
|UserName|用户名（登录名）。|  
|SessionId|会话 ID。|  
|QueryId|查询 ID。|  
|CommandType|命令类型。|  
|CommandText|已处理的命令中的文本。|  
|OperationType|事件的操作类型。|  
|持续时间|事件持续时间。|  
|SPID|服务进程 ID。|  
  
## <a name="remarks"></a>注解  
 允许每个用户最多拥有 10 个并发诊断会话。 请参阅 [sys.pdw_diag_sessions](../../relational-databases/system-catalog-views/sys-pdw-diag-sessions-transact-sql.md) 获取当前会话的列表，并使用 `DROP DIAGNOSTICS SESSION` 删除任何不需要的会话。  
  
 诊断会话将持续收集元数据，直到被删除。  
  
## <a name="permissions"></a>权限  
 需要 ALTER SERVER STATE 权限。  
  
## <a name="locking"></a>锁定  
 对诊断会话表使用共享锁。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-diagnostics-session"></a>A. 创建诊断会话  
 此示例将创建诊断会话，用于记录数据库引擎性能指标。 该示例创建诊断会话，用于侦听引擎查询运行/结束事件和一个阻止的 DMS 事件。 返回的内容为命令文本、计算机名、请求 ID（查询 ID）和创建该事件的会话。  
  
```sql  
CREATE DIAGNOSTICS SESSION MYDIAGSESSION AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:EngineQueryRunningEvent" />  
      <Event Name="DmsCoreInstrumentation:DmsBlockingQueueEnqueueBeginEvent" />  
      <Where>  
         <SessionId Value="381" ComparisonType="NotEquals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Query.CommandText" />  
      <Property Name="MachineName" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Alias" />  
      <Property Name="Duration" />  
      <Property Name="Session.SessionId" />  
   </Capture>  
</Session>';  
```  
  
 创建诊断会话后，运行查询。  
  
```sql  
SELECT COUNT(EmployeeKey) FROM AdventureWorksPDW2012..FactSalesQuota;  
```  
  
 然后通过选择 sysdiag 架构查看诊断会话结果。  
  
```sql  
SELECT * FROM master.sysdiag.MYDIAGSESSION;  
```  
  
 请注意，sysdiag 架构包含一个依据诊断会话名称进行命名的视图。  
  
 若要仅查看连接的活动，请添加 `Session.SPID` 属性并向查询添加 `WHERE [Session.SPID] = @@spid;`。  
  
 不再需要诊断会话后，请使用 DROP DIAGNOSTICS 命令将其删除。  
  
```sql  
DROP DIAGNOSTICS SESSION MYDIAGSESSION;  
```  
  
### <a name="b-alternative-diagnostic-session"></a>B. 替代诊断会话  
 使用略有不同的属性的另一个示例。  
  
```sql  
-- Determine the session_id of your current session  
SELECT TOP 1 session_id();  
-- Replace \<*session_number*> in the code below with the numbers in your session_id  
CREATE DIAGNOSTICS SESSION PdwOptimizationDiagnostics AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:MemoGenerationBeginEvent" />  
      <Event Name="EngineInstrumentation:MemoGenerationEndEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationBeginEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationEndEvent" />  
      <Event Name="DSQLInstrumentation:BuildRelOpContextTreeBeginEvent" />  
      <Event Name="DSQLInstrumentation:PostPlanGenModifiersEndEvent" />  
      <Where>  
         <SessionId Value="\<*session_number*>" ComparisonType="Equals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Session.SessionId" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Query.CommandText" />  
      <Property Name="Name" />  
      <Property Name="DateTimePublished" />  
      <Property Name="DateTimePublished.Ticks" />  
  </Capture>  
</Session>';  
```  
  
 运行查询，如：  
  
```sql  
USE ssawPDW;  
GO  
SELECT * FROM dbo.FactFinance;  
```  
  
 以下查询将返回授权计时：  
  
```sql  
SELECT *   
FROM master.sysdiag.PdwOptimizationDiagnostics   
ORDER BY DateTimePublished;  
```  
  
 不再需要诊断会话后，请使用 DROP DIAGNOSTICS 命令将其删除。  
  
```sql  
DROP DIAGNOSTICS SESSION PdwOptimizationDiagnostics;  
```  
  
  
