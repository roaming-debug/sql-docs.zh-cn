---
title: 为合并项目设置业务逻辑处理程序
description: 使用复制编程或复制管理对象来配置业务逻辑处理程序，以便进行合并复制同步。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], business logic handlers
- merge replication business logic handlers [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
- business logic handlers [SQL Server replication]
- BusinessLogicModule class
ms.assetid: ed477595-6d46-4fa2-b0d3-a5358903ec05
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 45ed99072fb5abad47b9f63a22f879c0f4e438eb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076825"
---
# <a name="implement-a-business-logic-handler-for-a-merge-article"></a>实现合并项目的业务逻辑处理程序
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用复制编程方式或复制管理对象 (RMO) 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中实现合并项目的业务逻辑处理程序。  
  
 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> 命名空间实现了一个可编写复杂业务逻辑以处理合并复制同步期间发生的事件的接口。 复制进程可以针对同步期间复制的每个已更改行调用业务逻辑处理程序中的方法。  
  
 实现业务逻辑处理程序的一般过程是：  
  
1.  创建业务逻辑处理程序程序集。  
  
2.  在分发服务器上注册程序集。  
  
3.  在运行合并代理的服务器上部署程序集。 对于请求订阅，代理在订阅服务器上运行，而对于推送订阅，代理在分发服务器上运行。 使用 Web 同步时，代理在 Web 服务器上运行。  
  
4.  创建使用业务逻辑处理程序的项目，或修改现有项目以使用业务逻辑处理程序。  
  
 指定的业务逻辑处理程序是针对每个同步的行来执行的， 因此复杂的逻辑以及对其他应用程序或网络服务的调用都可能会影响性能。 有关业务逻辑处理程序的详细信息，请参阅[在合并同步期间执行业务逻辑](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md)。  
  
 **本主题内容**  
  
-   **实现合并项目的业务逻辑处理程序，使用：**  
  
     [复制编程方式](#ReplProg)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="using-replication-programming"></a><a name="ReplProg"></a> 使用复制编程  
  
#### <a name="to-create-and-deploy-a-business-logic-handler"></a>创建和部署业务逻辑处理程序  
  
1.  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio 中，为包含可实现业务逻辑处理程序的代码的 .NET 程序集创建一个新项目。  
  
2.  将对以下列命名空间的引用添加到该项目。  
  
    |程序集引用|位置|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM（默认安装）|  
    |<xref:System.Data>|GAC（.NET Framework 的组件）|  
    |<xref:System.Data.Common>|GAC（.NET Framework 的组件）|  
  
3.  添加用于覆盖 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 类的类。  
  
4.  实现 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> 属性以指示处理的更改类型。  
  
5.  覆盖 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 类的以下一个或多个方法：  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> - 在同步期间提交数据更改时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> - 在上载或下载 DELETE 语句过程中出现错误时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> - 在上载或下载 DELETE 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> - 在上载或下载 INSERT 语句过程中出现错误时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> - 在上载或下载 INSERT 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> - 当发布服务器和订阅服务器上出现冲突的 UPDATE 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> - 当发布服务器和订阅服务器上的 UPDATE 语句与 DELETE 语句冲突时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> - 在上载或下载 UPDATE 语句过程中出现错误时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> - 在上载或下载 UPDATE 语句时调用。  
  
6.  生成该项目以创建业务逻辑处理程序程序集。  
  
7.  在包含合并代理可执行文件 (replmerg.exe) 的目录中部署该程序集，对默认安装来说，该目录为 [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM，或将该程序集安装在 .NET 全局程序集缓存 (GAC) 中。 如果合并代理之外的应用程序需要访问该程序集，您只能将该程序集安装在 GAC 中。 可以使用 .NET Framework SDK 中提供的全局程序集缓存工具 (**Gacutil.exe)** 将该程序集安装到 GAC 中。  
  
    > [!NOTE]  
    >  必须在每个运行合并代理的服务器上部署业务逻辑处理程序，包括使用 Web 同步时 replisapi.dll 所驻留的 IIS 服务器。  
  
#### <a name="to-register-a-business-logic-handler"></a>注册业务逻辑处理程序  
  
1.  在发布服务器中，执行 [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) 以验证该程序集是否尚未注册为业务逻辑处理程序。  
  
2.  在分发服务器上，执行 [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)，为 \@article_resolver 指定业务逻辑处理程序的友好名称，为 \@is_dotnet_assembly 指定值 true，为 \@dotnet_assembly_name 指定程序集的名称，并为 \@dotnet_class_name 指定替换 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 的类的完全限定名称。  
  
    > [!NOTE]  
    >  如果未将该程序集部署在与合并代理可执行文件相同的目录下、与同步启动合并代理的应用程序相同的目录下，或者全局程序集缓存 (GAC) 中，则需要为 \@dotnet_assembly_name 指定带有该程序集名称的完整路径。 使用 Web 同步时，必须指定程序集在 Web 服务器中的位置。  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>将业务逻辑处理程序与新的表项目一起使用  
  
1.  执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 来定义项目，为 \@article_resolver 指定业务逻辑处理程序的友好名称。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>将业务逻辑处理程序用于现有的表项目  
  
1.  执行 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)，指定 \@publication、\@article，为 \@property 指定值 article_resolver，为 \@value 指定业务逻辑处理程序的友好名称。  
  
###  <a name="examples-replication-programming"></a><a name="TsqlExample"></a> 示例（复制编程方式）  
 该示例演示了可创建审核日志的业务逻辑处理程序。  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 以下示例在分发服务器中注册了业务逻辑处理程序程序集，并更改了一个现有合并项目以使用此自定义业务逻辑。  
  
 [!code-sql[HowTo#sp_RegisterBLH_10](../../relational-databases/replication/codesnippet/tsql/implement-a-business-log_3.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
  
#### <a name="to-create-a-business-logic-handler"></a>创建业务逻辑处理程序  
  
1.  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio 中，为包含可实现业务逻辑处理程序的代码的 .NET 程序集创建一个新项目。  
  
2.  将对以下列命名空间的引用添加到该项目。  
  
    |程序集引用|位置|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM（默认安装）|  
    |<xref:System.Data>|GAC（.NET Framework 的组件）|  
    |<xref:System.Data.Common>|GAC（.NET Framework 的组件）|  
  
3.  添加用于覆盖 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 类的类。  
  
4.  实现 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> 属性以指示处理的更改类型。  
  
5.  覆盖 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 类的以下一个或多个方法：  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> - 在同步期间提交数据更改时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> - 在上载或下载 DELETE 语句出错时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> - 在上载或下载 DELETE 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> - 在上载或下载 INSERT 语句出错时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> - 在上载或下载 INSERT 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> - 当发布服务器和订阅服务器上出现冲突的 UPDATE 语句时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> - 当发布服务器和订阅服务器上的 UPDATE 语句与 DELETE 语句冲突时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> - 在上载或下载 UPDATE 语句出错时调用。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> - 在上载或下载 UPDATE 语句时调用。  
  
    > [!NOTE]  
    >  自定义业务逻辑未显式处理的任何项目冲突都由项目的默认冲突解决程序进行处理。  
  
6.  生成该项目以创建业务逻辑处理程序程序集。  
  
#### <a name="to-register-a-business-logic-handler"></a>注册业务逻辑处理程序  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumBusinessLogicHandlers%2A> 并检查返回的 <xref:System.Collections.ArrayList> 对象，以确保还未将该程序集注册为业务逻辑处理程序。  
  
4.  创建的 <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler> 类的实例。 指定以下属性：  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetAssemblyName%2A> - .NET 程序集的名称。 如果该程序集未部署到与合并代理可执行文件相同的目录中、与同步启动合并代理的应用程序相同的目录中或 GAC 中，则必须随程序集名称提供完整的路径。 将业务逻辑处理程序与 Web 同步一起使用时，必须随程序集名称提供完整的路径。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetClassName%2A> - 用于覆盖 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 并实现业务逻辑处理程序的类的完全限定名称。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> - 访问业务逻辑处理程序时使用的友好名称。  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.IsDotNetAssembly%2A> - 值为 **\@is_dotnet_assembly**。  
  
#### <a name="to-deploy-a-business-logic-handler"></a>部署业务逻辑处理程序  
  
1.  将程序集部署到运行合并代理的服务器上，具体部署位置为在分发服务器上注册业务逻辑处理程序时所指定的文件位置。 对于请求订阅，代理在订阅服务器上运行，而对于推送订阅，代理在分发服务器上运行。 使用 Web 同步时，代理在 Web 服务器上运行。 如果注册业务逻辑处理程序时未对程序集名称提供完整的路径，则将程序集部署到与合并代理可执行文件相同的目录中、与同步启动合并代理的应用程序相同的目录中。 如果有多个应用程序使用相同的程序集，则可以将程序集安装在 GAC 中。  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>将业务逻辑处理程序与新的表项目一起使用  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergeArticle> 类的实例。 设置以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>设置为项目的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>设置为发布的名称。  
  
    -   为 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A>设置发布数据库的名称。  
  
    -   为<xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A>设置业务逻辑处理程序的友好名称 ( <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>)。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.Article.Create%2A> 方法。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>将业务逻辑处理程序用于现有的表项目  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergeArticle> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>和 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> 属性。  
  
4.  为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置步骤 1 中的连接。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 3 中的项目属性定义不正确或此项目不存在。 有关详细信息，请参阅 [View and Modify Article Properties](../../relational-databases/replication/publish/view-and-modify-article-properties.md)。  
  
6.  为 <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>设置业务逻辑处理程序的友好名称。 这是注册业务逻辑处理程序时指定的 <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> 属性的值。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例是一个业务逻辑处理程序，它记录有关订阅服务器上的插入、更新和删除操作的信息。  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 此示例在分发服务器上注册业务逻辑处理程序。  
  
 [!code-cs[HowTo#rmo_RegisterBLH_10](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_registerblh_10)]  
  
 [!code-vb[HowTo#rmo_vb_RegisterBLH_10](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_registerblh_10)]  
  
 此示例更改现有项目以使用业务逻辑处理程序。  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>另请参阅  
 [为合并项目实现自定义冲突解决程序](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [调试业务逻辑处理程序（复制编程）](../../relational-databases/replication/debug-a-business-logic-handler-replication-programming.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [复制管理对象概念](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
  
