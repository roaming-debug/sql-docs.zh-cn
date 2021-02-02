---
title: 实现自定义冲突解决程序（合并）
description: 了解如何在 SQL Server 中为合并发布实现自定义冲突解决程序。
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
- merge replication conflict resolution [SQL Server replication], stored procedure-based resolvers
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 76bd8524-ebc1-4d80-b5a2-4169944d6ac0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5b07107b7ee615e317a9375b652a1b3ef43ad3d8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077135"
---
# <a name="implement-a-custom-conflict-resolver-for-a-merge-article"></a>为合并项目实现自定义冲突解决程序
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或[基于 COM 的自定义冲突解决程序](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md)，在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中为合并项目实现自定义冲突解决程序。  
  
 **本主题内容**  
  
-   **为合并项目实现自定义冲突解决程序时使用：**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [基于 COM 的冲突解决程序](#COM)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 您可以编写自己的自定义冲突解决程序以作为每个发布服务器上的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程。 在同步过程中，当冲突解决程序注册到的项目遇到冲突时，就是调用此存储过程。 合并代理将冲突行的相关信息传递到此过程的必需参数。 基于存储过程的自定义冲突解决程序将始终在发布服务器上创建。  
  
> [!NOTE]  
>  调用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程冲突解决程序只是为了处理基于行更改的冲突。 它们不能用于处理其他类型的冲突，如由 PRIMARY KEY 冲突或唯一索引约束冲突触发的插入失败。
  
#### <a name="to-create-a-stored-procedure-based-custom-conflict-resolver"></a>创建基于存储过程的自定义冲突解决程序  
  
1.  在发布服务器的发布数据库或 **msdb** 数据库中，创建用于实现以下所需参数的新系统存储过程：  
  
    |参数|数据类型|说明|  
    |---------------|---------------|-----------------|  
    |**\@tableowner**|**sysname**|冲突被解决的表的所有者名称。 这是发布数据库中的表的所有者。|  
    |**\@tablename**|**sysname**|冲突被解决的表的名称。|  
    |**\@rowguid**|**uniqueidentifier**|有冲突的行的唯一标识符。|  
    |**\@subscriber**|**sysname**|正在从中传播冲突更改的服务器的名称。|  
    |**\@subscriber_db**|**sysname**|正在从中传播冲突更改的数据库的名称。|  
    |**\@log_conflict OUTPUT**|**int**|设置合并进程是否应记录冲突以供以后解决：<br /><br /> **0** = 不记录冲突。<br /><br /> **1** = 订阅服务器是冲突解决落选方。<br /><br /> **2** = 发布服务器是冲突解决落选方。|  
    |**\@conflict_message OUTPUT**|**nvarchar(512)**|记录冲突时要提供的有关解决方法的消息。|  
    |**\@destowner**|**sysname**|订阅服务器上的已发布表的所有者。|  
  
     此存储过程使用由合并代理传递给这些参数的值，实现自定义冲突解决逻辑。 它必须返回单行结果集，此结果集的结构与基表完全相同，且包含行获胜版本的数据值。  
  
2.  向订阅服务器用来连接到发布服务器的任何登录名授予对存储过程的 EXECUTE 权限。  

#### <a name="use-a-custom-conflict-resolver-with-a-new-table-article"></a>将自定义冲突解决程序用于新的表项目  
  
1. 执行 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 来定义项目。 
1. 为 **article_resolver 参数指定 Microsoft SQL Server 存储过程冲突解决程序值**  **\@** 。 
1. 为 \@resolver_info 参数指定用于实现冲突解决程序逻辑的存储过程的名称。 

   有关详细信息，请参阅[定义项目](../../relational-databases/replication/publish/define-an-article.md)。
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>将自定义冲突解决程序用于现有表项目  
  
1.  执行 [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)，指定 **publication 和 \@article，将** property 的值指定为 article_resolver，将 **value 的值指定为 MicrosoftSQL Server Stored ProcedureResolver\@**  **\@**   **\@** 。  
  
2.  执行 [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)，指定 \@publication、\@article 并指定 \@property 的值为 resolver_info，同时为 \@value 指定用于实现冲突解决程序逻辑的存储过程的名称。  
  
##  <a name="using-a-com-based-custom-resolver"></a><a name="COM"></a> 使用基于 COM 的自定义冲突解决程序  
 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> 命名空间实现了一个接口，可便于编写复杂的业务逻辑，用于处理事件，并解决在合并复制同步过程中发生的冲突。 有关详细信息，请参阅[为合并项目实现业务逻辑处理程序](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)。 您也可以编写自己的基于本机代码的自定义业务逻辑以解决冲突。 此逻辑作为 COM 组件生成，并编译到动态链接库 (DLL) 中（使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++ 等产品）。 这类基于 COM 的自定义冲突解决程序必须实现 ICustomResolver  接口，这是专为解决冲突而设计的接口。  
  
#### <a name="to-create-and-register-a-com-based-custom-conflict-resolver"></a>创建和注册基于 COM 的自定义冲突解决程序  
  
1.  在与 COM 兼容的创作环境中，添加对自定义冲突解决程序库的引用。  
  
2.  对于 Visual C++ 项目，请使用 #import 命令将该库导入项目中。  
  
3.  创建一个用于实现 **ICustomResolver** 接口的类。  
  
4.  实现某些特定的方法和属性。  
  
5.  生成项目以创建自定义冲突解决程序库文件。  
  
6.  将库部署到包含合并代理可执行文件的目录（通常为 \Microsoft SQL Server\100\COM）。  
  
    > [!NOTE]  
    >  对于请求订阅，必须在订阅服务器上部署自定义冲突解决程序；对于推送订阅，必须在分发服务器上部署；或者部署在使用 Web 同步的 Web 服务器上。  
  
7.  运行部署目录中的 regsvr32.exe，注册自定义冲突解决程序库，如下所示：  
  
    ```  
    regsvr32.exe mycustomresolver.dll  
    ```  
  
8.  在发布服务器上，执行 [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md)，以验证库是否尚未注册为自定义冲突解决程序。  
  
9. 若要将库注册为自定义冲突解决程序，请在分发服务器上执行 [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)。 将 COM 对象的友好名称指定给 \@article_resolver，将库的 ID (CLSID) 指定给 \@resolver_clsid，将 false 值指定给 \@is_dotnet_assembly。  
  
    > [!NOTE]  
    >  如果不再需要自定义冲突解决程序，可使用 [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md) 取消注册它。  
  
10. （可选）在群集上，对群集的所有节点重复步骤 6-9，以注册自定义冲突解决程序。 为了确保自定义冲突解决程序在故障转移之后能够正常加载协调器，必须执行这些步骤。
  
#### <a name="to-use-a-custom-conflict-resolver-with-a-new-table-article"></a>将自定义冲突解决程序用于新的表项目  
  
1.  在发布服务器上，执行 [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md)，并记下所需解决程序的易记名称。  
  
2.  在发布服务器上，对发布数据库执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 来定义项目。 为 \@article_resolver 指定步骤 1 中项目解决程序的友好名称。 有关详细信息，请参阅[定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>将自定义冲突解决程序用于现有表项目  
  
1.  在发布服务器上，执行 [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md)，并记下所需解决程序的易记名称。  
  
2.  执行 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)，指定 \@publication、\@article，为 \@property 指定值 article_resolver，为 \@value 指定步骤 1 中的项目解决程序的友好名称。  
  

## <a name="see-also"></a>另请参阅  
 [高级合并复制冲突检测和解决](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [基于 COM 的自定义冲突解决程序](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
