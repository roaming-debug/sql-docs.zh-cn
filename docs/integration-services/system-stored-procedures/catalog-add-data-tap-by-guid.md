---
description: catalog.add_data_tap_by_guid
title: catalog.add_data_tap_by_guid | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ed9d7fa3-61a1-4e21-ba43-1ead7dfc74eb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 969f6975d2f73cfea80582694a3b0370a00f7264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354916"
---
# <a name="catalogadd_data_tap_by_guid"></a>catalog.add_data_tap_by_guid 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于执行实例，在包数据流中将数据分流点添加到指定的数据流路径。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.add_data_tap_by_guid [ @execution_id = ] execution_id  
, [ @dataflow_task_guid = ] dataflow_task_guid   
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id  
```  
  
## <a name="arguments"></a>参数  
 [ @execution_id = ] execution_id  
 包含包的执行 ID。 execution_id 为 bigint。  
  
 [ @dataflow_task_guid = ] *dataflow_task_guid*  
 包含要分流的数据流路径的包中的数据任务流 ID。 *dataflow_task_guid* 为 **uniqueidentifier**。  
  
 [ @dataflow_path_id_string = ] dataflow_path_id_string  
 数据流路径的标识字符串。 一个路径连接两个数据流组件。 路径的 IdentificationString 属性指定该字符串。  
  
 若要找到此标识字符串，请在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 中右键单击两个数据流组件之间的路径，然后单击“属性”。 将在“属性”窗口中显示 IdentificationString 属性 。  
  
 dataflow_path_id_string 为 nvarchar(4000)。  
  
 [ @data_filename = ] data_filename  
 存储分流的数据的文件名称。 如果数据流任务在 Foreach 循环或 For 循环容器内执行，则用单独的文件存储该循环每次迭代的分流数据。 用与每次迭代对应的编号为每个文件加前缀。 数据分流点文件将写入文件夹“\<SQL Server installation folder>\130\DTS\\”。 data_filename 为 nvarchar(4000)。  
  
 [ @max_rows = ] max_rows  
 在数据分流期间捕获的行数。 如果未指定此值，则捕获所有行。 max_rows 为 **int**。  
  
 [ @data_tap_id = ] data_tap_id  
 数据分流点的 ID。 data_tap_id 为 bigint。  
  
## <a name="example"></a>示例  
 在以下示例中，对数据流任务 `Paths[SRC DimDCVentor.OLE DB Source Output]` 中的数据流路径 `{D978A2E4-E05D-4374-9B05-50178A8817E8}` 创建一个数据分流点。 分流的数据存储在 DCVendorOutput.csv 文件中。  
  
```sql
exec catalog.add_data_tap_by_guid   @execution_id,   
'{D978A2E4-E05D-4374-9B05-50178A8817E8}',   
'Paths[SRC DimDCVentor.OLE DB Source Output]',   
'D:\demos\datafiles\DCVendorOutput.csv'  
```  
  
## <a name="remarks"></a>备注  
 若要添加数据分流点，执行实例必须处于已创建状态（在 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)视图的 status 列中值为 1）。 只要运行执行，状态值就会发生更改。 可以通过调用 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)创建执行。  
  
 以下是 add_data_tap_by_guid 存储过程的注意事项。  
  
-   添加数据分流点时，在运行包之前不进行验证。  
  
-   建议限制在数据分流期间捕获的行数，以避免生成大型数据文件。 如果执行存储过程的计算机无法为数据文件提供足够的存储空间，存储过程将停止运行。  
  
-   运行 add_data_tap_by_guid 存储过程会影响包的性能。 建议您仅运行该存储过程来解决数据问题。  
  
-   要访问存储分流数据的文件，您必须对运行该存储过程的计算机具有管理员权限，或者必须是开始执行包含带数据分流点的包的用户。  
  
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
  
## <a name="see-also"></a>另请参阅  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)  
  
  
