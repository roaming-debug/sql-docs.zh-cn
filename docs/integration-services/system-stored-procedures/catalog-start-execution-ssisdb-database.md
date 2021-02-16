---
description: catalog.start_execution（SSISDB 数据库）
title: catalog.start_execution（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f8663ff3-aa98-4dd8-b850-b21efada0b87
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e98650614583eab2b40e6433721a7cf2d73b1a6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342177"
---
# <a name="catalogstart_execution-ssisdb-database"></a>catalog.start_execution（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中启动执行实例。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.start_execution [ @execution_id = ] execution_id [, [ @retry_count = ] retry_count]  
```  
  
## <a name="arguments"></a>参数  
 [@execution_id =] execution_id  
 执行实例的唯一标识符。 execution_id 为 bigint。
 
 [@retry_count =] retry_count  
 执行失败时的重试次数。 仅当在 Scale Out 中执行时才生效。此参数是可选的。 如果未指定，其值设置为 0。 retry_count 为 int。
  
## <a name="remarks"></a>注解  
 使用执行来指定参数值，包在单个包执行实例中将使用这些参数值。 在创建执行实例后，但在启动此实例之前，可能重新部署对应的项目。 在本例中，执行实例引用过时的项目。 这个无效引用导致存储过程失败。  
  
> [!NOTE]  
>  只能启动执行一次。 要开始执行实例，它必须处于已创建状态（[catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) 视图中 status 列的值为 `1`）。  
  
## <a name="example"></a>示例  
 以下示例调用 catalog.create_execution 来创建 Child1.dtsx 包的执行实例。 Integration Services Project1 包含该包。 该示例调用 catalog.set_execution_parameter_value 来设置 Parameter1、Parameter2 和 LOGGING_LEVEL 参数的值。 该示例调用 catalog.start_execution 启动一个执行实例。  
  
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
  
-   针对执行实例的 READ 和 MODIFY 权限，针对项目的 READ 和 EXECUTE 权限，针对引用环境的 READ 权限（如果适用）  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   用户没有相应的权限  
  
-   执行标识符无效  
  
-   执行已被启动或已完成；只能启动执行一次  
  
-   与项目关联的环境引用无效  
  
-   尚未设置所需的参数值  
  
-   与实例执行关联的项目版本已过时；只可执行项目的最新版本  
  
  
