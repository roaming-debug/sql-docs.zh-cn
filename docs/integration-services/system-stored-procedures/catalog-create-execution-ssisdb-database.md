---
description: catalog.create_execution（SSISDB 数据库）
title: catalog.create_execution（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 45d0c2f6-1f38-445f-ac06-e2a01f6ac600
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a6b6a3733b54c24be1f06e91a6a60135faf
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129873"
---
# <a name="catalogcreate_execution-ssisdb-database"></a>catalog.create_execution（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建执行实例。  
  
 此存储过程使用默认服务器日志记录级别。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_execution [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id ]  
  [  , [ @use32bitruntime = ] use32bitruntime ] 
  [  , [ @runinscaleout = ] runinscaleout ]
  [  , [ @useanyworker = ] useanyworker ] 
     , [ @execution_id = ] execution_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [@folder_name =] folder_name  
 包含要执行的包的文件夹名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@project_name =] project_name  
 包含要执行的包的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [@package_name =] package_name  
 包含要执行的包的名称。 package_name 为 nvarchar(260)。  
  
 [@reference_id =] reference_id  
 环境引用的唯一标识符。 此参数是可选的。 reference_id 为 bigint   。  
  
 [@use32bitruntime =] use32bitruntime  
 指示是否应使用 32 位运行时在 64 位操作系统上运行包。 使用值 1 表示在 64 位操作系统上运行时，使用 32 位运行时执行此包。 使用值 0 表示在 64 位操作系统上运行时，使用 64 位运行时执行此包。 此参数是可选的。 Use32bitruntime 为 bit。  
 
 [@runinscaleout =] runinscaleout  
 指示执行是否在 Scale Out 中进行。使用值 1 在 Scale Out 中执行包。使用值 0 执行包，而无需 Scale Out。此参数是可选的。 如果未指定，其值在 [SSISDB].[catalog].[catalog_properties] 中设置为 DEFAULT_EXECUTION_MODE。 runinscaleout 为 bit。 
 
[@useanyworker =] useanyworker  
指示是否允许任何 Scale Out Worker 执行相应操作。

-   借助任何 Scale Out Worker 使用值 1 执行包。 将 `@useanyworker` 设为 true 时，尚未达到最大任务数（如辅助角色配置文件中所示）的任何辅助角色都可用于运行包。 有关辅助角色配置文件的信息，请参阅 [Integration Services (SSIS) Scale Out 辅助角色](../scale-out/integration-services-ssis-scale-out-worker.md)。

-   使用值 0 指示并非允许所有 Scale Out Workers 执行包。 将 `@useanyworker` 设为 false 时，必须指定通过使用 Scale Out Manager 或调用存储过程 `[catalog].[add_execution_worker]` 允许运行包的辅助角色。 如果指定的辅助角色已在运行另一个包，则该辅助角色会先完成运行当前的包，然后再请求执行另一个。

此参数是可选的。 如果未指定，其值设置为 1。 useanyworker 为 bit。 
  
 [@execution_id =] execution_id  
 返回执行实例的唯一标识符。 execution_id 为 bigint。  

  
## <a name="remarks"></a>注解  
 使用执行来指定参数值，包在单个包执行实例中将使用这些参数值。  
  
 如果使用 reference_id 参数指定某个环境引用，则该存储过程将使用对应环境变量中的文本值或引用值填充项目和包参数。 如果指定了环境引用，则在包执行过程中将使用默认参数值。 若要精确确定将哪些值用于特定执行实例的值，应使用此存储过程中的 execution_id 输出参数值，并查询 [execution_parameter_values](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md) 视图。  
  
 在执行中只能指定标记为入口点包的包。 如果指定的包不是入口点，则执行失败。  
  
## <a name="example"></a>示例  
 以下示例调用 catalog.create_execution 来创建 Child1.dtsx 包（不在 Scale Out 中）的执行实例。Integration Services Project1 包含该包。 该示例调用 catalog.set_execution_parameter_value 来设置 Parameter1、Parameter2 和 LOGGING_LEVEL 参数的值。 该示例调用 catalog.start_execution 启动一个执行实例。  
  
```sql  
Declare @execution_id bigint  
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Child1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'TestDeply4', @project_name=N'Integration Services Project1', @use32bitruntime=False, @reference_id=Null  
Select @execution_id  
DECLARE @var0 sql_variant = N'Child1.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter1', @parameter_value=@var0  
DECLARE @var1 sql_variant = N'Child2.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter2', @parameter_value=@var1  
DECLARE @var2 smallint = 1  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var2  
EXEC [SSISDB].[catalog].[start_execution] @execution_id  
GO  
```  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 和 EXECUTE 权限，如果适用，则需要针对引用环境的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  

 如果 @runinscaleout 为 1，则此存储过程需要下列权限之一：
 
-   **ssis_admin** 数据库角色的成员资格

-   **ssis_cluster_executor** 数据库角色的成员资格

-   **sysadmin** 服务器角色的成员资格
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可引发错误或警告的情况：  
  
-   该包不存在。  
  
-   用户不具备适当的权限。  
  
-   环境引用 reference_id 无效。  
  
-   指定的包不是入口点包。  
  
-   引用环境变量的数据类型不同于项目或包参数的数据类型。  
  
-   项目或包包含需要值的参数，但尚未分配任何值。  
  
-   无法在环境引用 reference_id 所指定的环境中找到引用的环境变量。  
  
## <a name="see-also"></a>另请参阅  
 [catalog.start_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)   
 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
 [catalog.add_execution_worker（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)  
  
