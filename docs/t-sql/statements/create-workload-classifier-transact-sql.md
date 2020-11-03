---
description: CREATE WORKLOAD Classifier (Transact-SQL)
title: CREATE WORKLOAD Classifier (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest||=sqlallproducts-allversions
ms.openlocfilehash: 0a9c2fc146de0ea8b6e5ab769b0eccfc1bca6d12
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067289"
---
# <a name="create-workload-classifier-transact-sql"></a>CREATE WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

创建用于工作负荷管理的分类器对象。  分类器根据分类器语句定义中指定的参数将传入请求分配给工作负荷组。  对提交的每个请求评估分类器。  如果请求与分类器不匹配，则将其分配给默认工作负荷组。  默认工作负荷组是 smallrc 资源类。

> [!NOTE]
> 工作负荷分类器取代 sp_addrolemember 资源类分配。  创建工作负荷分类器后，执行 sp_droprolemember 以删除任何冗余的资源类映射。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数

 classifier_name  
 指定用于标识工作负荷分类器的名称。  classifier_name 为 sysname。  最长可为 128 个字符，并且在实例中必须是唯一的。

 *WORKLOAD_GROUP* =  *'name'*    
 当分类器规则满足条件时，名称将请求映射到工作负荷组。  名称为 sysname。  它最多可以包含 128 个字符，并且在创建分类器时必须是有效的工作负荷组名称。

 可用工作负荷组可在 [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md) 目录视图中找到。

 *MEMBERNAME* =  *'security_account'*     
 用作分类依据的安全帐户。  Security_account 为 sysname，没有默认值。 Security_account 可以是数据库用户、数据库角色、Azure Active Directory 登录名或 Azure Active Directory 组。
 
 *WLM_LABEL*   
 指定可作为请求分类依据的标签值。  标签是类型为 nvarchar(255) 的可选参数。  使用请求中的 [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) 来匹配分类器配置。

示例：

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
指定可作为请求分类依据的会话上下文值。  上下文是类型为 nvarchar(255) 的可选参数。  在提交设置会话上下文的请求之前，请使用变量名称为 [ 的 ](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest)sys.sp_set_session_context`wlm_context`。

示例：

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* 和 *END_TIME*  
指定可作为请求分类依据的 start_time 和 end_time。  start_time 和 end_time 为采用 HH:MM 格式的 UTC 时区值。  start_time 和 end_time 必须一起指定。

示例：

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

IMPORTANCE = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
指定请求的相对重要性。  重要性为以下值之一：

- LOW
- BELOW_NORMAL
- NORMAL（默认值）
- ABOVE_NORMAL
- HIGH  

如果未指定重要性，则使用工作负荷组的重要性设置。  默认工作负荷组重要性为“普通”。  重要性会影响请求的安排顺序，导致首先访问资源和锁定。

## <a name="classification-parameter-weighting"></a>分类参数权重

请求可以与多个分类器匹配。  分类器参数有一个权重。  权重较高的匹配分类器会用于分配工作负载组和重要性。  权重如下所示：

|分类器参数 |重量   |
|---------------------|---------|
|USER                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

请考虑以下分类器配置。

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

两个分类器均配置了用户 `userloginA`。  如果 userloginA 在 UTC 时间下午 6 点到上午 7 点之间运行标签为 `salesreport` 的查询，则该请求会分类为具有“高”重要性的 wgDashboards 工作负荷组。  预期结果可能会是将请求分类为具有“低”重要性的 wgUserQueries 以用于非工作时间报告，但 WLM_LABEL 的权重高于 START_TIME/END_TIME。  classifierA 的权重为 80（用户 64，加上 WLM_LABEL 的 16）。  ClassifierB 的权重为 68（用户 64，加上 START_TIME/END_TIME 的 4）。  在这种情况下，可以将 WLM_LABEL 添加到 classifierB。

 有关详细信息，请参阅[工作负载权重](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting)。

## <a name="permissions"></a>权限

 需要 CONTROL DATABASE 权限。  
  
## <a name="examples"></a>示例

 下面的示例演示了如何创建名为 `wgcELTRole` 的工作负荷分类器。 它使用 staticrc20 工作负荷组和用户 `ELTRole`，并将重要性设置为 `above_normal`。

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>另请参阅

[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 分类](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

