---
description: catalog.add_data_tap
title: catalog.add_data_tap | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a25ebcc7-535e-4619-adf6-4e2b5a62ba37
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8d188b361b32627ffac604a9b69e14f54fa26355
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339200"
---
# <a name="catalogadd_data_tap"></a>catalog.add_data_tap 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于执行实例，在包数据流中添加组件输出的数据分流点。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.add_data_tap [ @execution_id = ] execution_id  
, [ @task_package_path = ] task_package_path  
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [ @execution_id = ] execution_id  
 包含包的执行 ID。 execution_id 为 bigint。  
  
 [ @task_package_path = ] task_package_path  
 数据流任务的包路径。 数据流任务的 PackagePath 属性指定该路径。 此路径区分大小写。 若要查找包路径，请在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 中右键单击数据流任务，然后单击“属性”。 将在“属性”窗口中显示 PackagePath 属性 。  
  
 task_package_path 为 nvarchar(max)。  
  
 [ @dataflow_path_id_string = ] dataflow_path_id_string  
 数据流路径的标识字符串。 一个路径连接两个数据流组件。 路径的 IdentificationString 属性指定该字符串。  
  
 若要找到此标识字符串，请在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 中右键单击两个数据流组件之间的路径，然后单击“属性”。 将在“属性”窗口中显示 IdentificationString 属性 。  
  
 dataflow_path_id_string 为 nvarchar(4000)。  
  
 [ @data_filename = ] data_filename  
 存储分流的数据的文件名称。 如果数据流任务在 Foreach 循环或 For 循环容器内执行，则用单独的文件存储该循环每次迭代的分流数据。 用与每次迭代对应的编号为每个文件加前缀。  
  
 默认情况下，该文件存储在 \<*drive*>:\Program Files\Microsoft SQL Server\130\DTS\DataDumps 文件夹中。  
  
 data_filename 为 nvarchar(4000)。  
  
 [ @max_rows = ] max_rows  
 在数据分流期间捕获的行数。 如果未指定此值，则捕获所有行。 max_rows 为 int。  
  
 [ @data_tap_id = ] data_tap_id  
 返回数据分流点的 ID。 data_tap_id 为 bigint。  
  
## <a name="example"></a>示例  
 在以下示例中，对数据流任务 `'Paths[OLE DB Source.OLE DB Source Output]` 中的数据流路径 `\Package\Data Flow Task` 创建一个数据分流点。 分流的数据存储在 DataDumps 文件夹的 `output0.txt` 文件中（即 \<*drive*>:\Program Files\Microsoft SQL Server\130\DTS\DataDumps）。  
  
```sql
Declare @execution_id bigint  
Exec SSISDB.Catalog.create_execution @folder_name='Packages',@project_name='SSISPackages', @package_name='Package.dtsx',@reference_id=Null, @use32bitruntime=False, @execution_id=@execution_id OUTPUT  
  
Exec SSISDB.Catalog.set_execution_parameter_value @execution_id,50, 'LOGGING_LEVEL', 0  
  
Exec SSISDB.Catalog.add_data_tap @execution_id, @task_package_path='\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[OLE DB Source.OLE DB Source Output]', @data_filename = 'output0.txt'  
  
Exec SSISDB.Catalog.start_execution @execution_id  
```  
  
## <a name="remarks"></a>备注  
 若要添加数据分流点，执行实例必须处于已创建状态（在 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)视图的 status 列中值为 1）。 只要运行执行，状态值就会发生更改。 可以通过调用 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)创建执行。  
  
 以下是 add_data_tap 存储过程的注意事项。  
  
-   如果执行包含一个父包以及一个或多个子包，您需要为要分流数据的每个包添加数据分流点。  
  
-   如果包中包含名称相同的多个数据流任务，task_package_path 唯一标识包含分流的组件输出的数据流任务。  
  
-   添加数据分流点时，在运行包之前不进行验证。  
  
-   建议限制在数据分流期间捕获的行数，以避免生成大型数据文件。 如果执行存储过程的计算机无法为数据文件提供足够的存储空间，包将停止运行并将错误消息写入日志。  
  
-   运行 add_data_tap 存储过程会影响包的性能。 建议您仅运行该存储过程来解决数据问题。  
  
-   要访问存储分流的数据的文件，您必须是运行该存储过程的计算机上的管理员。 您还必须是开始执行包含带数据分流点的包的用户。  
  
## <a name="return-codes"></a>返回代码  
 0（成功）  
  
 存储过程失败时引发错误。  
  
## <a name="result-set"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对执行实例的 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下表说明了导致存储过程失败的情况。  
  
-   用户没有 MODIFY 权限。  
  
-   已添加指定的包中指定组件的数据分流点。  
  
-   为要捕获的行数指定的值无效。  
  
## <a name="requirements"></a>要求  
  
## <a name="external-resources"></a>外部资源  
 rafael-salas.com 上的博客文章：[SSIS 2012：数据分流点概览](https://go.microsoft.com/fwlink/?LinkId=239983)。  
  
## <a name="see-also"></a>另请参阅  
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
  
  
