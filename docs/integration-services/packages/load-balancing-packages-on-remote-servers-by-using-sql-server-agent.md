---
description: 使用 SQL Server 代理在远程服务器上平衡包的负载
title: 使用 SQL Server 代理在远程服务器上平衡包的负载 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- load-balancing [Integration Services]
- parent packages [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 9281c5f8-8da3-4ae8-8142-53c5919a4cfe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 19c64b4d9d20bbaa6552f11fa5f5228a62e7dd6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346905"
---
# <a name="load-balancing-packages-on-remote-servers-by-using-sql-server-agent"></a>使用 SQL Server 代理在远程服务器上平衡包的负载

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在必须运行很多包时，方便的做法是使用其他可用的服务器。 这种当所有包都处于一个父包控制下时使用其他服务器来运行这些包的方法称为负载平衡。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中，负载均衡是必须由包的所有者构建的手动过程。 服务器不自动执行负载平衡。 而且，在远程服务器上运行的包必须是整个包，而不能是其他包中的单个任务。  
  
 负载平衡在以下情形下是有用的：  
  
-   包可以同时运行。  
  
-   包很大，并且如果按顺序运行，其运行时间可能超过为处理所留出的时间。  
  
 管理员和体系结构设计师可以确定使用其他服务器来执行处理是否有益于他们的处理过程。  
  
## <a name="illustration-of-load-balancing"></a>负载平衡的图例  
 以下关系图显示了服务器上的父包。 父包包含多个“执行 SQL 作业代理”任务。 父包中的每项任务都会调用远程服务器上的 SQL Server 代理。 这些远程服务器包含 SQL Server 代理作业，而作业中包括调用该服务器上的包的步骤。  
  
 ![SSIS 负载均衡体系结构概览](../../integration-services/packages/media/loadbalancingoverview.gif "SSIS 负载均衡体系结构概览")  
  
 在此体系结构中的负载平衡所需的步骤不是新的概念。 实际上，负载平衡是通过一种新的方式使用现有概念和通用 SSIS 对象实现的。  
  
## <a name="execution-of-packages-on-a-remote-instance-by-using-sql-server-agent"></a>使用 SQL Server 代理在远程实例上执行包  
 在用于执行远程包的基本体系结构中，中心包驻留在控制其他远程包的 SQL Server 实例上。 关系图显示了此中心包，它的名称是“SSIS Parent”。 驻留此父包的实例控制运行子包的 SQL Server 代理作业的执行。 子包不按受远程服务器上的 SQL Server 代理所控制的固定计划来运行。 实际上，子包在被父包调用时由 SQL Server 代理启动，并运行于 SQL Server 代理所驻留的同一个 SQL Server 实例上。  
  
 必须先配置父包和子包并设置控制子包的 SQL Server 代理作业，然后才能通过使用 SQL Server 代理来运行远程包。 以下部分提供了有关如何创建、配置、运行和维护在远程服务器上运行的包的详细信息。 该过程有下面几个步骤：  
  
-   创建子包并将它们安装在远程服务器上。  
  
-   在将运行包的远程实例上创建 SQL Server 代理作业。  
  
-   创建父包。  
  
-   确定子包的日志记录方案。  
  
## <a name="implementation-of-child-packages"></a>子包的实现
  使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]来实现负载平衡时，子包将安装在其他服务器上，以利用可用的 CPU 或服务器时间。 若要创建和运行子包，需要执行以下步骤：  
  
-   设计子包。  
  
-   将包移动到远程服务器。  
  
-   在包含运行子包步骤的远程服务器上创建 SQL Server 代理作业。  
  
-   测试并调试 SQL Server 代理作业和子包。  
  
 设计子包时，包的设计不受限制，并且可以添加任何希望的功能。 但是，如果包要访问数据，则必须确保运行包的服务器能够访问该数据。  
  
 若要标识执行子包的父包，请在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的解决方案资源管理器中右键单击该包，然后单击 **“入口点包”**。  
  
 设计完子包之后，下一个步骤是将它们部署在远程服务器上。  
  
### <a name="moving-the-child-package-to-the-remote-instance"></a>将子包移动到远程实例  
 将包移动到其他服务器的方式有多个。 两个推荐的方法是：  
  
-   通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将包导出。  
  
-   先为包含要部署的包的项目生成部署实用工具，然后运行包安装向导将包安装到文件系统或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例，从而达到部署包的目的。 有关详细信息，请参阅[早期包部署 (SSIS)](../../integration-services/packages/legacy-package-deployment-ssis.md)。  
  
 必须重复部署到希望使用的每个远程服务器。  
  
### <a name="creating-the-sql-server-agent-jobs"></a>创建 SQL Server 代理作业  
 子包已经部署到多个服务器之后，请在包含子包的每个服务器上创建 SQL Server 代理作业。 SQL Server 代理作业包含调用作业代理时运行子包的步骤。 SQL Server 代理作业不是已调度作业；它们只在父包调用它们时才会运行子包。 返回到父包的作业成功或失败的通知反映 SQL Server 代理作业成功或失败，以及是否成功调用了代理作业，而不是反映子包的成功或失败或它是否已执行。  
  
### <a name="debugging-the-sql-server-agent-jobs-and-child-packages"></a>调试 SQL Server 代理作业和子包  
 通过使用下列方法之一，可以测试 SQL Server 代理作业及其子包：  
  
-   通过单击“调试” / “开始执行(不调试)”在 SSIS 设计器中运行每个子包 。  
  
-   通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]在远程计算机上运行单个 SQL Server 代理作业，以确保包运行。  
  
 有关如何对从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业运行的包进行故障排除的信息，请参阅 [支持知识库中的](https://support.microsoft.com/kb/918760) An SSIS package does not run when you call the SSIS package from a SQL Server Agent job step [!INCLUDE[msCoName](../../includes/msconame-md.md)] （从 SQL Server 代理作业步骤调用 SSIS 包时 SSIS 包不运行）。  
  
 SQL Server 代理检查代理的子系统访问权，并在每次运行作业步骤时将访问权授予代理。  
  
 可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建代理。  

## <a name="implementation-of-the-parent-package"></a>父包的实现
  负载平衡跨越多个服务器的 SSIS 包时，如果子包已经创建并部署且用来运行子包的远程 SQL Server 代理作业也创建之后，其下一个步骤是创建父包。 父包将包含很多执行 SQL Server 代理作业任务，每个任务负责调用用于运行其中一个子包的不同的 SQL Server 代理作业。 父包中的执行 SQL Server 代理作业任务又会运行各个 SQL Server 代理作业。 父包中的每个任务都包含诸如信息，例如，如何连接到远程服务器以及服务器上运行什么作业等。 有关详细信息，请参阅 [Execute SQL Server Agent Job Task](../../integration-services/control-flow/execute-sql-server-agent-job-task.md)。  
  
 若要标识执行子包的父包，请在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的解决方案资源管理器中右键单击该包，然后单击 **“入口点包”**。  
  
### <a name="listing-child-packages"></a>列出子包  
 如果将包含父包和子包的项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器，则可以查看由父包执行的子包的列表。 运行父包时， **中将自动生成父包** “概述” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]报表。 该报表列出了由父包中包含的执行包任务执行的子包，如下图所示。  
  
 ![包含子包列表的概述报告](../../integration-services/packages/media/overviewreport-childpackagelisting.png "包含子包列表的概述报告")  
  
 有关访问 **“概述”** 报表的信息，请参阅 [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)。  
  
### <a name="precedence-constraints-in-the-parent-package"></a>父包中的优先约束  
 在父包中的执行 SQL Server 代理作业任务之间创建优先约束时，这些优先约束只控制 SQL Server 代理作业在远程服务器上的启动时间。 优先约束无法接收关于从 SQL Server 代理作业的步骤中运行的子包成功或失败的信息。  
  
 这意味着子包的成功或失败不会传播到父包，因为父包中执行 SQL Server 代理作业的单独功能是请求 SQL Server 代理作业运行该子包。 成功调用 SQL Server 代理作业之后，父包将收到<xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success>的结果。  
  
 此方案中的失败只表示调用远程 SQL Server 代理作业任务已经失败。 可以发生该情况的一种情形是远程服务器已关闭且代理未响应。 但是，只要代理激发，父包就会成功完成其任务。  
  
> [!NOTE]  
>  可以使用包含 **sp_start_job N'package_name'** 的 Transact-SQL 语句的执行 SQL 任务。 有关详细信息，请参阅 [sp_start_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)。  
  
### <a name="debugging-environment"></a>调试环境  
 测试父包时，请在设计器的调试环境中通过使用“调试”/“启动调试”(F5) 来运行父包。 另外，还可以使用命令提示符实用工具 **dtexec**。 有关详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。  

## <a name="logging-for-load-balanced-packages-on-remote-servers"></a>远程服务器上的负载平衡包的日志记录
  如果所有子包都使用相同的日志提供程序并且它们全部写入相同的目标，则管理员更容易管理在各个服务器上运行的所有子包的日志。 创建所有子包公用日志文件的一种方式是：将子包配置为“将子包事件记录到 SQL Server 日志提供程序”。 可以将所有包配置为使用相同的数据库、相同的服务器和相同的服务器实例。  
  
 若要查看日志文件，管理员只须登录到单个服务器上，即可查看所有子包的日志文件。  
  
 有关如何在包中启用日志记录的信息，请参阅[Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  

## <a name="related-tasks"></a>Related Tasks  
 [包的 SQL Server 代理作业](../../integration-services/packages/sql-server-agent-jobs-for-packages.md)  
  
  
