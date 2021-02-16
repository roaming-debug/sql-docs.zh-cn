---
description: catalog.create_customized_logging_level
title: catalog.create_customized_logging_level | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 20b3ba0a-126f-49bf-b70f-61b2a0fcb750
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5fbedb41369300353d7b0f8ce64da355592b6eaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346840"
---
# <a name="catalogcreate_customized_logging_level"></a>catalog.create_customized_logging_level 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  创建新的自定义日志记录级别。 有关自定义日志记录级别的详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_customized_logging_level [ @level_name = ] level_name  
    , [ @level_description = ] level_description  
    , [ @profile_value = ] profile_value  
    , [ @events_value = ] events_value  
    , [ @level_id = ] level_id OUT   
```  
  
## <a name="arguments"></a>参数  
 [ @level_name = ] level_name  
 新的现有自定义日志记录级别的名称。  
  
 level_name 为 nvarchar(128)。  
  
 [ @level_description = ] level_description  
 有关新的现有自定义日志记录级别的说明。  
  
 level_description 为 nvarchar(max)。  
  
 [ @profile_value = ] profile_value  
 需要新的自定义日志记录级别记录的统计信息。  
  
 统计信息的有效值包括以下内容。 这些值与“自定义日志记录级别管理”对话框的“统计信息”选项卡上的值相对应。  
  
-   执行 = 0  
  
-   卷 = 1  
  
-   性能 = 2    
  
 profile_value 为 bigint。  
  
 [ @events_value = ] events_value  
 需要新的自定义日志记录级别记录的事件。  
  
 事件的有效值包括以下内容。 这些值与“自定义日志记录级别管理”对话框的“事件”选项卡上的值相对应。  
  
|没有事件上下文的事件|具有事件上下文的事件|  
|----------------------------------|-------------------------------|  
|OnVariableValueChanged = 0<br /><br /> OnExecutionStatusChanged = 1<br /><br /> OnPreExecute = 2<br /><br /> OnPostExecute = 3<br /><br /> OnPreValidate = 4<br /><br /> OnPostValidate = 5<br /><br /> OnWarning = 6<br /><br /> OnInformation = 7<br /><br /> OnError = 8<br /><br /> OnTaskFailed = 9<br /><br /> OnProgress = 10<br /><br /> OnQueryCancel = 11<br /><br /> OnBreakpointHit = 12<br /><br /> OnCustomEvent = 13<br /><br /> Diagnostic = 14<br /><br /> DiagnosticEx = 15<br /><br /> NonDiagnostic = 16|OnVariableValueChanged_IncludeContext = 32<br /><br /> OnExecutionStatusChanged_IncludeContext = 33<br /><br /> OnPreExecute_IncludeContext = 34<br /><br /> OnPostExecute_IncludeContext = 35<br /><br /> OnPreValidate_IncludeContext = 36<br /><br /> OnPostValidate_IncludeContext = 37<br /><br /> OnWarning_IncludeContext = 38<br /><br /> OnInformation_IncludeContext = 39<br /><br /> OnError_IncludeContext = 40<br /><br /> OnTaskFailed_IncludeContext = 41<br /><br /> OnProgress_IncludeContext = 42<br /><br /> OnQueryCancel_IncludeContext= 43<br /><br /> OnBreakpointHit_IncludeContext = 44<br /><br /> OnCustomEvent_IncludeContext = 45<br /><br /> Diagnostic_IncludeContext = 46<br /><br /> DiagnosticEx_IncludeContext = 47<br /><br /> NonDiagnostic_IncludeContext = 48|  
  
 events_value 为 bigint。  
  
 [ @level_id = ] level_id OUT  
 新的自定义日志记录级别的 ID。  
  
 level_id 为 bigint。  
  
## <a name="remarks"></a>备注  
 若要合并 Transact-SQL 中用于 profile_value 或 events_value 参数的的多个值，请按此示例中的操作执行。 若要捕获 OnError (8) 和 DiagnosticEx (15) 事件，计算 events_value 的公式为 `2^8 + 2^15 = 33024`。  
  
## <a name="return-codes"></a>返回代码  
 0（成功）  
  
 存储过程失败时引发错误。  
  
## <a name="result-set"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色中的成员资格  
  
-   **sysadmin** 服务器角色中的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下表说明了导致存储过程失败的情况。  
  
-   用户不具有所需的权限。  
  
  
