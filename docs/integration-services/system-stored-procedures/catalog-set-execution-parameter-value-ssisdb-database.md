---
description: catalog.set_execution_parameter_value（SSISDB 数据库）
title: catalog.set_execution_parameter_value（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 055d86c9-befd-4e63-acb1-6dfe833549d2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5775b87b13fc126907dfc0f121e9838c2d490fd0
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243640"
---
# <a name="catalogset_execution_parameter_value-ssisdb-database"></a>catalog.set_execution_parameter_value（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的执行实例设置参数的值。  
  
 当已开始执行实例后，就不能再更改参数值。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_execution_parameter_value [ @execution_id = execution_id  
    , [ @object_type = ] object_type  
    , [ @parameter_name = ] parameter_name  
    , [ @parameter_value = ] parameter_value  
```  
  
## <a name="arguments"></a>参数  
 [ @execution_id = ] execution_id  
 执行实例的唯一标识符。 execution_id 为 bigint。  
  
 [ @object_type = ] *object_type*  
 参数的类型。  
  
 对于以下参数，请将 *object_type* 设置为 50  
  
-   LOGGING_LEVEL  
  
-   CUSTOMIZED_LOGGING_LEVEL  
  
-   DUMP_ON_ERROR  
  
-   DUMP_ON_EVENT  
  
-   DUMP_EVENT_CODE  
  
-   CALLER_INFO  
  
-   SYNCHRONIZED  
  
 使用值 `20` 表示项目参数，使用值 `30` 表示包参数。  
  
 *object_type* 为 **smallint** 。  
  
 [ @parameter_name = ] *parameter_name*  
 参数的名称。 parameter_name 为 nvarchar(128)。  
  
 [ @parameter_value = ] *parameter_value*  
 参数值。 parameter_value 为 sql_variant。  
  
## <a name="remarks"></a>备注  
 要查找用于给定执行的参数值，请查询 catalog.execution_parameter_values 视图。  
  
 若要指定在包执行期间记录的信息范围，请将 *parameter_name* 设置为 LOGGING_LEVEL 并将 *parameter_value* 设置为以下值之一。  
  
 将 *object_type* 参数设置为 50。  
  
|值|说明|  
|-----------|-----------------|  
|0|无<br /><br /> 关闭日志记录。 仅记录包执行状态。|  
|1|基本<br /><br /> 除了自定义事件和诊断事件之外，记录其余所有事件。 这是默认值。|  
|2|性能<br /><br /> 仅记录性能统计信息、OnError 和 OnWarning 事件。|  
|3|“详细”<br /><br /> 记录所有事件，包括自定义事件和诊断事件。 <br />自定义事件包括 Integration Services 任务记录的那些事件。 有关详细信息，请参阅[日志记录的自定义消息](../../integration-services/performance/integration-services-ssis-logging.md#custom_messages)|  
|4|运行时沿袭<br /><br /> 收集跟踪数据流中的沿袭所需的数据。|  
|100|自定义日志记录级别<br /><br /> 指定 CUSTOMIZED_LOGGING_LEVEL 参数中的设置。 有关可指定的值的详细信息，请参阅 [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md)。<br /><br /> 有关自定义日志记录级别的详细信息，请参阅[在 SSIS 服务器上启用包执行的日志记录](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)。|  
  
 若要指定 Integration Services 服务器在包执行期间出现任意错误时生成转储文件，请为未运行的执行实例设置以下参数值：  
  
|参数|值|  
|---------------|-----------|  
|*execution_id*|执行实例的唯一标识符|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_ERROR|  
|*parameter_value*|1|  
  
 若要指定 Integration Services 服务器在包执行期间出现事件时生成转储文件，请为未运行的执行实例设置以下参数值：  
  
|参数|值|  
|---------------|-----------|  
|*execution_id*|执行实例的唯一标识符|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_EVENT|  
|*parameter_value*|1|  
  
 若要指定在包执行期间导致 Integration Services 服务器生成转储文件的事件，请为未运行的执行实例设置以下参数值： 使用分号分隔多个事件代码。  
  
|参数|值|  
|---------------|-----------|  
|*execution_id*|执行实例的唯一标识符|  
|*object_type*|50|  
|*parameter_name*|DUMP_EVENT_CODE|  
|*parameter_value*|一个或多个事件代码|  
  
## <a name="examples"></a>示例  

### <a name="a-generate-dump-files-for-errors"></a>A. 为错误生成转储文件

 以下示例指定 Integration Services 服务器在包执行期间出现任意错误时生成转储文件：  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_ERROR',1  
```  
  
### <a name="b-generate-dump-files-for-events"></a>B. 为事件生成转储文件

 以下示例指定 Integration Services 服务器在包执行期间出现事件时生成转储文件，并指定导致该服务器生成这些文件的事件：  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_EVENT',1  
  
declare @event_code nvarchar(50)  
set @event_code = '0xC020801C'  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_EVENT_CODE', @event_code  
```  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对执行实例的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   用户没有相应的权限  
  
-   执行标识符无效  
  
-   参数名称无效  
  
-   参数值的数据类型与参数的数据类型不匹配  
  
## <a name="see-also"></a>另请参阅  
 [catalog.execution_parameter_values（SSISDB 数据库）](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)   
 [生成包执行的转储文件](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
