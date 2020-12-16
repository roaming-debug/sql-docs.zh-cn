---
description: Replication Management Objects Concepts
title: 复制管理对象概念 | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- replication [SQL Server], RMO
- programming interfaces [SQL Server replication]
- replication [SQL Server], how-to topics
- RMO [SQL Server]
- Replication Management Objects
- programming [SQL Server replication], RMO
ms.assetid: 37476d50-fb47-49e3-9504-3b163ac381d8
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ca46285eacce3f77a07681fef9e75d16eddfd436
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469138"
---
# <a name="replication-management-objects-concepts"></a>Replication Management Objects Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  复制管理对象 (RMO) 是一个封装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制功能的托管代码程序集。 RMO 是由 <xref:Microsoft.SqlServer.Replication> 命名空间实现的。  
  
 以下几节中的主题介绍如何使用 RMO 以编程方式控制复制任务：  
  
 [配置分发](../../../relational-databases/replication/configure-distribution.md)  
 本节中的主题介绍如何使用 RMO 配置发布和分发。  
  
 [创建发布](../../../relational-databases/replication/publish/create-a-publication.md)  
 本节中的主题介绍如何使用 RMO 创建、删除和修改发布和项目。  
  
 [订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)  
 本节中的主题介绍如何使用 RMO 创建、删除和修改订阅。  
  
 [保护复制拓扑](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
 本节中的主题介绍如何使用 RMO 查看和修改安全设置。  
  
 [同步订阅（复制）](../../../relational-databases/replication/synchronize-data.md)  
 本节中的主题介绍如何同步订阅。  
  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
 本节中的主题介绍如何以编程方式监视复制拓扑。  
  
## <a name="introduction-to-rmo-programming"></a>RMO 编程简介  
 RMO 专用于对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制的各个方面进行编程。 RMO 的命名空间为 <xref:Microsoft.SqlServer.Replication>，它是由 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework 程序集 Microsoft.SqlServer.Rmo.dll 实现的。 Microsoft.SqlServer.Replication.dll 程序集也属于 <xref:Microsoft.SqlServer.Replication> 命名空间，该程序集实现了用于编程多种复制代理（快照代理、分发代理和合并代理）的托管代码接口。 该程序集的类可以从 RMO 访问以同步订阅。 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> 命名空间中的类由 Microsoft.SqlServer.Replication.BusinessLogicSupport.dll 程序集实现，这些类用于创建合并复制的自定义业务逻辑。 此程序集独立于 RMO。  
  
## <a name="deploying-applications-based-on-rmo"></a>部署基于 RMO 的应用程序  
 RMO 依赖于复制组件和客户端连接组件，这些组件包含在除 SQL Server Compact 之外的所有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本中。 若要部署基于 RMO 的应用程序，必须在要运行该应用程序的计算机上安装包含复制组件和客户端连接组件的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本。  
  
## <a name="getting-started-with-rmo"></a>RMO 入门  
 本节介绍如何使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio 开始一个简单的 RMO 项目。  
  
#### <a name="to-create-a-new-microsoft-visual-c-project"></a>创建新的 Microsoft Visual C# 项目  
  
1.  启动 Visual Studio。  
  
2.  在“文件”菜单中，单击“新建项目”。 将显示“新建项目”对话框。  
  
3.  在“项目类型”对话框中，选择“Visual C# 项目”。 在“模板”窗格中，选择“Windows 应用程序”。   
  
4.  （可选）在“名称”中，键入新应用程序的名称。  
  
5.  单击“确定”加载 Visual C# Windows 模板。  
  
6.  在“项目”菜单中，选择“添加引用”项。 此时将显示“添加引用”对话框。  
  
7.  从“.NET”选项卡上的列表中选择以下程序集，然后单击“确定”。  
  
    -   Microsoft.SqlServer.Replication .NET Programming Interface  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Replication Agent Library  
  
    > [!NOTE]  
    >  使用 CTRL 键可选择多个文件。  
  
8.  （可选）重复步骤 6。 单击“浏览”选项卡，导航到 [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM，选择“Microsoft.SqlServer.Replication.BusinessLogicSupport.dll”，然后单击“确定”。  
  
9. 在 **“视图”** 菜单上，单击 **“代码”**。  
  
10. 在代码的命名空间语句前，键入以下 **using** 语句，限定 RMO 命名空间中的类型：  

    ```  
    // These namespaces are required.  
    using Microsoft.SqlServer.Replication;  
    using Microsoft.SqlServer.Management.Common;  
    // This namespace is only used when creating custom business  
    // logic for merge replication.  
    using Microsoft.SqlServer.Replication.BusinessLogicSupport;   
    ```  
  
#### <a name="to-create-a-new-microsoft-visual-basic-net-project"></a>创建新的 Microsoft Visual Basic .NET 项目  
  
1.  启动 Visual Studio。  
  
2.  在“文件”菜单中，选择“新建项目”。 将显示“新建项目”对话框。  
  
3.  在“项目类型”窗格中，选择“Visual Basic”。 在“模板”窗格中，选择“Windows 应用程序”。  
  
4.  （可选）在“名称”框中，键入新应用程序的名称。  
  
5.  单击“确定”加载 Visual Basic Windows 模板。  
  
6.  在“项目”菜单中，选择“添加引用”。 此时将显示“添加引用”对话框。  
  
7.  从“.NET”选项卡上的列表中选择以下程序集，然后单击“确定”。  
  
    -   Microsoft.SqlServer.Replication .NET Programming Interface  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Replication Agent Library  
  
    > [!NOTE]  
    >  使用 CTRL 键可选择多个文件。  
  
8.  （可选）重复步骤 6。 单击“浏览”选项卡，导航到 [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM，选择“Microsoft.SqlServer.Replication.BusinessLogicSupport.dll”，然后单击“确定”。  
  
9. 在 **“视图”** 菜单上，单击 **“代码”**。  
  
10. 在代码的任何声明前，键入以下 **Imports** 语句限定 RMO 命名空间中的类型。  
  
    ```  
    ' These namespaces are required.  
    Imports Microsoft.SqlServer.Replication  
    Imports Microsoft.SqlServer.Management.Common  
    ' This namespace is only used when creating custom business  
    ' logic for merge replication.  
    Imports Microsoft.SqlServer.Replication.BusinessLogicSupport   
    ```  
  
## <a name="connecting-to-a-replication-server"></a>连接复制服务器  
 RMO 编程对象要求使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类的实例连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 与该服务器的连接是独立于任何 RMO 编程对象的。 然后，在创建实例的过程中或在给 RMO 对象的 `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContex`t 属性赋值的过程中，该编程对象会传递给 RMO 对象。 采用这种方式，RMO 编程对象实例和连接对象实例可以分别创建和管理，而多个 RMO 编程对象可以重用一个连接对象。 连接复制服务器时适用下列规则：  
  
-   该连接的所有属性都对给定的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象定义。  
  
-   与每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的连接必须有自己的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
-   将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象赋给正在创建的或正在服务器上访问的 RMO 编程对象的 `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext` 属性。  
  
-   <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> 方法打开与服务器的连接。 必须在调用此方法后，才能对任何使用该连接的 RMO 编程对象调用任一访问服务器的方法。  
  
-   由于 RMO 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO) 都使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类来连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，因此 RMO 和 SMO 对象可使用相同的连接。 有关详细信息，请参阅[连接到 SQL Server 的实例](../../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)。  
  
-   执行连接并成功登录服务器所需的所有验证信息由 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象提供。  
  
-   默认情况下使用 Windows 身份验证。 若要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，<xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.LoginSecure%2A> 必须设置为“False”，并且 <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Login%2A> 和 <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Password%2A> 必须设置为有效的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名和密码。 安全凭据必须始终以安全的方式存储和处理，并且尽可能在运行时才提供。  
  
-   对于多线程应用程序，每个线程应使用单独的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
 对 <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Disconnect%2A> 对象调用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 方法将关闭 RMO 对象所使用的活动的服务器连接。  
  
## <a name="setting-rmo-properties"></a>设置 RMO 属性  
 RMO 编程对象的属性表示服务器上这些复制对象的属性。 在服务器上创建新的复制对象时，RMO 属性用于定义这些对象。 对于现有对象，RMO 属性表示现有对象的属性，其中可修改的只有可写和可设置的属性。 对新对象或现有对象都可以设置属性。  
  
### <a name="setting-properties-for-new-replication-objects"></a>为新复制对象设置属性  
 在服务器上创建新的复制对象时，必须指定所有必需的属性，然后才能调用该对象的 **Create** 方法。 有关为新的复制对象设置属性的详细信息，请参阅[配置发布和分发](../../../relational-databases/replication/configure-publishing-and-distribution.md)。  
  
### <a name="setting-properties-for-existing-replication-objects"></a>为现有复制对象设置属性  
 对于服务器上存在的复制对象，RMO 可能只支持更改其属性的部分或全部，具体取决于对象。 只有可写或可设置的属性才能更改。 更改属性前，必须调用 **Load** 或 `M:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties` 方法从服务器获取当前属性。 调用这些方法表示要修改现有对象。  
  
 默认情况下，更改对象属性时，RMO 会根据所使用的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 的执行模式向服务器提交这些更改。 尝试检索或更改对象的属性之前，可以使用 `P:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject` 方法来验证服务器上是否存在该对象。 有关更改复制对象属性的详细信息，请参阅[查看和修改分发服务器和发布服务器属性](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
> [!NOTE]  
>  多个 RMO 客户端或一个 RMO 编程对象的多个实例同时访问服务器上的同一个复制对象时，可根据服务器上该对象的当前状态，调用 RMO 对象的 **Refresh** 方法来更新属性。  
  
### <a name="caching-property-changes"></a>缓存属性更改  
 <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes> 属性设置为 <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes.CaptureSql> 时，RMO 生成的所有 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句都会被捕获，因此可使用执行方法之一在一个批次内手动执行这些语句。 RMO 可以缓存属性更改并使用对象的 `M:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges` 方法在一个批次内将这些更改一起提交。 若要缓存属性更改，对象的 `P:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges` 属性必须设置为 **true**。 缓存 RMO 中的属性更改时，<xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象仍控制何时向服务器发送更改。 有关缓存复制对象的属性更改的详细信息，请参阅[查看和修改分发服务器和发布服务器属性](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
> [!IMPORTANT]  
>  虽然 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类支持在设置属性时声明显式事务，但这样的事务会影响内部复制事务，可能产生难以预料的结果，因此不应在 RMO 中使用。  

### <a name="enabling-tls-12-support-for-rmo-components"></a>启用 RMO 组件的 TLS 1.2 支持 
 通过安装更新 [KB 3140245](https://support.microsoft.com/help/3140245)，并创建文章中提到的注册表项，可以在 Windows 2012 及更低版本上启用 RMO 组件的 TLS1.2 支持。 在 Windows 2012 R2 和更高版本上，只需创建上述文章中提到的注册表项。
 
## <a name="example"></a>示例  
 本示例演示如何缓存属性更改。 对事务发布属性的更改被缓存，直到将其显式发送给服务器。  
  
 [!code-cs[HowTo#rmo_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changetranpub_cached)]  
  
## <a name="see-also"></a>另请参阅  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [复制编程概念](../../../relational-databases/replication/concepts/replication-programming-concepts.md)  
  
  
