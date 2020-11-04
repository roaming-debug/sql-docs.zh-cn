---
description: dm_execution_performance_counters（SSISDB 数据库）
title: dm_execution_performance_counters（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 1b38e8e3-c560-4b6e-b60e-bfd7cfcd4fdf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 19111422e69b2ce77f53e13bb6d1a450b4ef7692
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243662"
---
# <a name="functions---dm_execution_performance_counters"></a>函数 - dm_execution_performance_counters

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]

  返回正在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务器上运行的执行的性能统计信息。  
  
## <a name="syntax"></a>语法  
  
```sql  
dm_execution_performance_counters [ @execution_id = ] execution_id  
  
```  
  
## <a name="arguments"></a>参数  
 [ @execution_id = ] execution_id   
 包含一个或多个包的执行的唯一标识符。 通过“执行包”任务执行的包在同一个执行中作为父包运行。  
  
 如果未指定执行 ID，则返回多个执行的性能统计信息。 如果你是 **ssis_admin** 数据库角色的成员，将返回所有正在运行的执行的性能统计信息。  如果你不是 **ssis_admin** 数据库角色的成员，则返回你对其具有读权限的正在运行的执行的性能统计信息。 *execution_id* 为 **BigInt** 。  
  
## <a name="remarks"></a>备注  
 下表列出了由 dm_execution_performance_counter 函数返回的计数器名称值。  
  
|计数器名称|说明|  
|------------------|-----------------|  
|BLOB bytes read|数据流引擎从所有源中读取的二进制大型对象 (BLOB) 数据的字节数。|  
|BLOB bytes written|数据流引擎写入所有目标的 BLOB 数据的字节数。|  
|BLOB files in use|数据流引擎正用于假脱机的 BLOB 文件数。|  
|Buffer memory|Integration Services 缓冲区使用的内存量，包括物理内存和虚拟内存。|  
|Buffers in use|数据流组件和数据流引擎正在使用的各种类型的缓冲区对象的数量。|  
|Buffers Spooled|写入磁盘的缓冲区的数量。|  
|Flat buffer memory|所有平面缓冲区使用的内存量（以字节为单位）。 平面缓冲区是组件用于存储数据的内存块。|  
|Flat buffers in use|数据流引擎使用的平面缓冲区的数量。 所有平面缓冲区都是专用缓冲区。|  
|Private buffer memory|所有专用缓冲区所使用的内存量。 专用缓冲区是指转换用于临时工作的缓冲区。<br /><br /> 如果数据流引擎创建的是一个支持数据流的缓冲区，则该缓冲区不是专用缓冲区。|  
|Private buffers in use|转换用于临时工作的缓冲区的数量。|  
|Rows read|执行读取的总行数。|  
|Rows written|执行写入的总行数。|  
  
## <a name="return"></a>返回  
 dm_execution_performance_counters 函数为正在运行的执行返回包含以下列的表。 返回的信息用于执行中包含的所有包。 如果没有正在运行的执行，则返回空表。  
  
|列名|列类型|说明|备注|  
|-----------------|-----------------|-----------------|-------------|  
|execution_id|**BigInt**<br /><br /> **NULL** 不是有效值。|包含包的执行的唯一标识符。||  
|counter_name|**nvarchar(128)**|计数器的名称。|请参阅值的 **备注** 部分。|  
|counter_value|**BigInt**|计数器所返回的值。||  
  
## <a name="examples"></a>示例  

### <a name="a-return-statistics-for-a-running-execution"></a>A. 返回正在运行的执行的统计信息

 在下面的示例中，该函数将返回 ID 为 34 的正在运行的执行的统计信息。  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (34)  
```  
  
### <a name="b-return-statistics-for-all-running-executions"></a>B. 返回所有正在运行的执行的统计信息

 在下面的示例中，该函数返回正在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务器上运行的所有执行的统计信息，具体取决于您的权限。  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (NULL)  
  
```  
  
## <a name="permissions"></a>权限  
 此函数需要下列权限之一：  
  
-   针对执行实例的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下表说明了导致函数失败的情况。  
  
-   用户对指定的执行没有 MODIFY 权限。  
  
-   指定的执行 ID 无效。  
  
  
