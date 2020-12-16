---
title: 对扩展事件使用 PowerShell 提供程序
description: 使用 SQL Server PowerShell 提供程序管理 SQL Server 扩展事件。 本文介绍创建、更改和管理会话的示例。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- PowerShell [SQL Server], xevent
- extended events [SQL Server], PowerShell
- PowerShell [SQL Server], extended events
ms.assetid: 0b10016f-a479-4444-a484-46cb4677cf64
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 034142392069443993c5d987b8aed80231c229fb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481328"
---
# <a name="use-the-powershell-provider-for-extended-events"></a>对扩展事件使用 PowerShell 提供程序

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供程序管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 扩展事件。 XEvent 子文件夹位于 SQLSERVER 驱动器下。 您可以使用以下任意一种方法访问该文件夹：  
  
-   在命令提示符处，键入 **sqlps**，然后按 Enter。 键入 **cd xevent**，然后按 Enter。 然后，可以使用 **cd** 和 **dir** 命令（或 **Set-Location** 和 **Get-Childitem** cmdlet）导航到服务器名称和实例名称。  
  
-   在对象资源管理器中，展开实例名称，展开“管理”  ，右键单击“扩展事件”  ，然后单击“启动 PowerShell”  。 这将在以下路径中启动 PowerShell：  
  
     PS SQLSERVER:\XEvent\\*ServerName*\\*InstanceName*>  
  
    > [!NOTE]  
    >  您可以从 **“扩展事件”** 下的任意节点启动 PowerShell。 例如，你可以右键单击“会话”  ，然后单击“启动 PowerShell”  。 这将在下一级别（即“会话”文件夹）启动 PowerShell。  
  
 您可以浏览 XEvent 文件夹树以查看现有的扩展事件会话及其关联的事件、目标和谓词。 例如，在 PS SQLSERVER:\XEvent\\*ServerName*\\*InstanceName*> 路径下，如果你键入 **cd sessions**，按“Enter”，键入 **dir**，然后按“Enter”，则会看到该实例上存储的会话列表。 您还可以查看会话是否正在运行（如果正在运行，那么可以查看运行了多长时间），以及会话是否配置为在实例启动时启动。  
  
 若要查看与会话关联的事件、它们的谓词以及目标，您可以将目录更改为该会话的名称，然后查看事件或目标文件夹。 例如，若要查看与默认系统运行状况会话关联的事件以及它们的谓词，请在 PS SQLSERVER:\XEvent\\*ServerName*\\*InstanceName*\Sessions> 路径下，键入 **cd system_health\events,** ，按“Enter”，键入 **dir**，然后按“Enter”。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供程序是一种非常强大的工具，您可以用其创建、更改和管理扩展事件会话。 下面一节提供将 PowerShell 脚本与扩展事件结合使用的一些基本示例。  
  
## <a name="examples"></a>示例  
 在下面的示例中，请注意以下事项：  
  
-   脚本必须从 PS SQLSERVER:\\> 提示符（可在命令提示符下键入 **sqlps** 来打开该提示符）下运行。  
  
-   脚本使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的默认实例。  
  
-   脚本必须使用 .ps1 扩展名保存。  
  
-   PowerShell 执行策略必须允许脚本运行。 若要设置执行策略，请使用 **Set-Executionpolicy** cmdlet。 （有关详细信息，请键入 **get-help set-executionpolicy -detailed**，然后按“Enter”。）  
  
 下面的脚本将创建一个名为“TestSession”的新会话。  
  
```  
#Script for creating a session.  
cd XEvent  
$h = hostname  
cd $h  
  
#Use the default instance.  
$store = dir | where {$_.DisplayName -ieq 'default'}  
$session = new-object Microsoft.SqlServer.Management.XEvent.Session -argumentlist $store, "TestSession"  
$event = $session.AddEvent("sqlserver.file_written")  
$event.AddAction("package0.callstack")  
$session.Create()  
```  
  
 下面的脚本将环形缓冲区目标添加到上一个示例所创建的会话中。 （本示例演示了 **Alter** 方法的用法。 请注意，在首次创建会话后即可添加目标。）  
  
```  
#Script to alter a session.  
cd XEvent  
$h = hostname  
cd $h  
cd DEFAULT\Sessions  
  
#Used to find the specified session.  
$session = dir|where {$_.Name -eq 'TestSession'}  
  
#Add the ring buffer target and call the Alter method.  
$session.AddTarget("package0.ring_buffer")  
$session.Alter()  
```  
  
 下面的脚本将创建一个使用谓词表达式的新会话。 在本示例中，该会话将收集有关 c:\temp.log 文件何时发生写入操作的信息（通过 sqlserver.file_written 事件）。  
  
```  
#Script for creating a session.  
cd XEvent  
$h = hostname  
cd $h  
  
#Use the default instance.  
$store = dir | where {$_.DisplayName -ieq 'default'}  
$session = new-object Microsoft.SqlServer.Management.XEvent.Session -argumentlist $store, "TestSession2"  
$event = $session.AddEvent("sqlserver.file_written")  
  
#Construct a predicate "equal_i_unicode_string(path, N'c:\temp.log')".  
$column = $store.SqlServerPackage.EventInfoSet["file_written"].DataEventColumnInfoSet["path"]  
$operand = new-object Microsoft.SqlServer.Management.XEvent.PredOperand -argumentlist $column  
$value = new-object Microsoft.SqlServer.Management.XEvent.PredValue -argumentlist "c:\temp.log"  
$compare = $store.Package0Package.PredCompareInfoSet["equal_i_unicode_string"]  
$predicate = new-object Microsoft.SqlServer.Management.XEvent.PredFunctionExpr -argumentlist $compare, $operand, $value  
$event.SetPredicate($predicate)  
$session.Create()  
```  
  
## <a name="security"></a>安全性  
 若要创建、更改或删除扩展事件会话，您必须拥有 ALTER ANY EVENT SESSION 权限。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server PowerShell](../../powershell/sql-server-powershell.md)   
 [使用 system_health 会话](../../relational-databases/extended-events/use-the-system-health-session.md)   
 [扩展事件工具](../../relational-databases/extended-events/extended-events-tools.md)  
  
