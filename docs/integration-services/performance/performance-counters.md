---
description: 性能计数器
title: 性能计数器 | Microsoft Docs
ms.custom: supportability
ms.date: 08/27/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- logs [Integration Services], performance counters
- performance counters [Integration Services]
- data flow [Integration Services], performance
- counters [Integration Services]
- data flow engine [Integration Services]
ms.assetid: 11e17f4e-72ed-44d7-a71d-a68937a78e4c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7ae8429bb51ec2a58a167784b701b7ae0532bb92
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076925"
---
# <a name="performance-counters"></a>性能计数器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 安装一组性能计数器，可用于监视数据流引擎的性能。 例如，可以监视 "Buffers spooled" 计数器，以确定在运行包时数据缓冲区是否正在临时写入磁盘。 此交换会降低性能并指示计算机内存不足。  
  
> **注意：** 如果在运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的计算机上安装 [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)]，然后将该计算机升级到 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)]，则升级过程会从该计算机删除 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 性能计数器。 若要还原计算机上的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 性能计数器，请在修复模式下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序。  
  
 下表介绍了这些性能计数器：  
  
|性能计数器|说明|  
|-------------------------|-----------------|  
|BLOB bytes read|数据流引擎从所有源中读取的二进制大型对象 (BLOB) 数据的字节数。|  
|BLOB bytes written|数据流引擎已写入所有目标的 BLOB 数据的字节数。|  
|BLOB files in use|数据流引擎当前用于假脱机的 BLOB 文件数。|  
|Buffer memory|正在使用的内存数量。 这可能包括物理内存和虚拟内存。 如果该数字大于物理内存量，则 **Buffers Spooled** 计数将随内存交换的增加而增加。 内存交换的增加会降低数据流引擎的性能。|  
|Buffers in use|数据流组件和数据流引擎当前使用的各种类型的缓冲区对象的数量。|  
|Buffers Spooled|当前写入磁盘的缓冲区的数量。 如果数据流引擎运行时物理内存较低，则当前没有使用的缓冲区会被写入磁盘，然后在需要时重新加载。|  
|Flat buffer memory|所有平面缓冲区使用的内存总量（以字节为单位）。 平面缓冲区是组件用于存储数据的内存块。 平面缓冲区是可逐字节访问的大型字节块。|  
|Flat buffers in use|数据流引擎使用的平面缓冲区的数量。 所有平面缓冲区都是专用缓冲区。|  
|Private buffer memory|所有专用缓冲区所使用的内存总量。 如果数据流引擎创建的是一个支持数据流的缓冲区，则该缓冲区就不是专用缓冲区。 专用缓冲区是指转换仅用于临时工作的缓冲区。 例如，聚合转换使用专用缓冲区来完成其工作。|  
|Private buffers in use|转换使用的缓冲区的数量。|  
|Rows read|源产生的行数。 该数值不包括查找转换从引用表中读取的行数。|  
|Rows written|提供给目标的行数。 该数值不反映写入目标数据存储区中的行数。|  
  
 您可以使用 Microsoft 管理控制台 (MMC) 管理单元“性能”来创建一个捕获性能计数器的日志。  
  
 有关如何提高性能的信息，请参阅 [数据流性能特点](../../integration-services/data-flow/data-flow-performance-features.md)。  
  
## <a name="obtain-performance-counter-statistics"></a>获取性能计数器统计信息  
 对于部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，你可以通过使用 [dm_execution_performance_counters （SSISDB 数据库）](../../integration-services/functions-dm-execution-performance-counters.md)函数获取性能计数器统计信息。  
  
 在下面的示例中，该函数将返回 ID 为 34 的正在运行的执行的统计信息。  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (34)  
```  
  
 在下面的示例中，该函数返回正在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上运行的所有执行的统计信息。  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (NULL)  
  
```  
  
> **重要说明!!** 如果你是 **ssis_admin** 数据库角色的成员，将返回所有正在运行的执行的性能统计信息。  如果你不是 **ssis_admin** 数据库角色的成员，则返回你对其具有读权限的正在运行的执行的性能统计信息。  
  
## <a name="related-content"></a>相关内容  
  
-   codeplex.com 上的工具 [Business Intelligence Development Studio 的 SSIS 性能可视化（CodePlex 项目）](https://go.microsoft.com/fwlink/?LinkId=146626)。  
  
-   msdn.microsoft.com 上的视频 [测量和了解 SSIS 包在企业中的性能（SQL Server 视频）](/previous-versions/sql/sql-server-2008/dd795223(v=sql.100))。  
  
-   support.microsoft.com 上的支持文章 [升级到 Windows Server 2008 后性能监视器中不再提供 SSIS 性能计数器](https://web.archive.org/web/20111112202346/http://support.microsoft.com:80/kb/955632)。  

## <a name="add-a-log-for-data-flow-performance-counters"></a>添加数据流性能计数器的日志
  本过程介绍如何为数据流引擎提供的性能计数器添加日志。  
  
> [!NOTE]  
>  如果在运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的计算机上安装 [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)]，然后将该计算机升级到 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)]，则升级过程会从该计算机删除 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 性能计数器。 若要还原计算机上的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 性能计数器，请在修复模式下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序。  
  
### <a name="to-add-logging-of-performance-counters"></a>添加性能计数器的日志记录  
  
1.  在 **“控制面板”** 中，如果您使用的是经典视图，请单击 **“管理工具”** 。 如果使用的是分类视图，请单击 **“性能和维护”** ，再单击 **“管理工具”** 。  
  
2.  单击 **“性能”** 。  
  
3.  在“性能”对话框中，展开“性能日志和警报”，右键单击“计数器日志”，再单击“新建日志设置”。 键入日志的名称。 例如，键入 **MyLog**。  
  
4.  单击“确定”。  
  
5.  在 **MyLog** 对话框中，单击 **“添加计数器”** 。  
  
6.  单击 **“使用本地计算机计数器”** 记录本地计算机上性能计数器的日志，或者单击 **“从计算机选择计数器”** ，然后从列表中选择计算机，以记录该指定的计算机的性能计数器的日志。  
  
7.  在 **“添加计数器”** 对话框中，选择 **“性能对象”** 列表中的 **“SQL Server:SSIS 管道”** 。  
  
8.  若要选择性能计数器，请执行下列操作之一：  
  
    -   选择 **“所有计数器”** 以记录所有性能计数器的日志。  
  
    -   选择 **“选择列表中的计数器”** ，然后选择要使用的性能计数器。  
  
9. 单击“添加”。  
  
10. 单击“关闭”。  
  
11. 在 **MyLog** 对话框中，检查 **“计数器”** 列表中记录日志的性能计数器的列表。  
  
12. 若要添加其他计数器，请重复步骤 5 到步骤 10。  
  
13. 单击“确定”。  
  
    > [!NOTE]  
    >  必须使用属于 Administrators 组成员的本地帐户或域帐户启动性能日志和警报服务。  

## <a name="see-also"></a>另请参阅  
 [项目和包的执行](../packages/run-integration-services-ssis-packages.md) [Integration Services 包记录的事件](../../integration-services/performance/events-logged-by-an-integration-services-package.md)
