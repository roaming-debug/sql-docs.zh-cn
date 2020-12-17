---
title: SQL Server 属性（“启动参数”选项卡）
description: 使用“SQL Server 属性”对话框的“启动参数”选项卡来添加或删除启动参数，这可能影响数据库引擎的性能。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 16942624-5374-446c-8de4-ee6ed34d6e94
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f8e0b03d5ea3be6473c8e00a8d62ef5900f33973
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465728"
---
# <a name="sql-server-properties-startup-parameters-tab"></a>SQL Server 属性（“启动参数”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用此对话框可以添加或删除 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的启动参数。 启动参数可能会对 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 性能产生很大影响。 在添加或更改启动参数前，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的主题“使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动选项”。  
  
## <a name="options"></a>选项  
 **指定启动参数**  
 若要添加某一参数，请键入该参数，然后单击 **“添加”** 。  
  
 若要修改所需的参数之一，请在 **“现有参数”** 框中键入该参数，更改 **“指定启动参数”** 框中的值，然后单击 **“更新”**。  
  
 **“现有参数”**  
 若要删除某一参数，请选择该参数，然后单击 **“删除”** 。  
  
## <a name="parameter-format"></a>参数格式  
 不要在参数之间输入分隔符。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器会自动添加分隔符。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器将强制以下参数要求。  
  
-   前导空格和尾随空格将会从任何启动参数中删除。  
  
-   所有启动参数都以短破折号 (-) 开头，且第二个值是字母。  
  
## <a name="required-parameters"></a>必需的参数  
 下列参数是必需的。 可以更改这些参数，但不能删除它们。  
  
-   -d 是 **master.mdf** 文件（master 数据库数据文件）的路径。  
  
-   -l 是 **master.ldf** 文件（master 数据库日志文件）的路径。  
  
-   -e 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志文件的路径。  
  
> [!CAUTION]  
>  如果该文件路径参数不正确， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能不会启动。  
  
 有关如何移动 master 数据库的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“移动系统数据库”主题。  
  
## <a name="optional-parameters"></a>可选参数  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书的“使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动选项”主题中介绍了所有支持的启动参数。 -T *trace#* 的启动参数指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例应该以有效的指定跟踪标志 (*trace#* ) 启动。 跟踪标记用于以非标准行为启动服务器。 有关跟踪标志的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“跟踪标志（[!INCLUDE[tsql](../../includes/tsql-md.md)]）”主题。  
  
> [!CAUTION]  
>  您可能会看到在 Internet 上描述的其他未记录的启动参数和跟踪标志。 创建未记录的启动参数和跟踪标志是为了满足某些不常见问题或者强制测试所需的某些条件。 使用未记录的启动参数可能会导致意外结果。 除非 Microsoft 客户支持服务部门指示，否则不要使用未记录的参数。  
  
 下表描述某些常见的可选参数。  
  
|参数|简短说明|  
|---------------|-----------------------|  
|-M|在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。|  
|-T1204|返回参与死锁的锁的资源和类型，以及受影响的当前命令。|  
|-T1224|基于锁数禁用锁升级。|  
|-T3608|禁止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动启动和恢复除 master 数据库之外的任何数据库。|  
|-T7806|在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]上启用专用管理员连接 (DAC)。|  
  
> [!CAUTION]  
>  某些可选参数可能会更改服务器行为并且可能会影响性能。  
  
## <a name="permissions"></a>权限  
 此页的使用被限制为可以在注册表中更改相关条目的用户。 其中包括以下用户。  
  
-   本地管理员组的成员。  
  
-   如果将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]配置为在域帐户下运行，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用该域帐户。  
  
## <a name="books-online-references"></a>联机丛书参考  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动参数的其他信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“如何配置服务器启动选项（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器）”。  
  
  
