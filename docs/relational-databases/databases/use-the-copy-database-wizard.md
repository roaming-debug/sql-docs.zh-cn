---
description: 使用复制数据库向导
title: 使用复制数据库向导 | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.cdw.packageconfiguration.f1
- sql13.swb.cdw.schedule.f1
- sql13.swb.cdw.transfermethod.f1
- sql13.swb.cdw.databases.f1
- sql13.swb.cdw.destinationconfiguration.f1
- sql13.swb.cdw.destination.f1
- sql13.swb.cdw.locationofsourcedbfiles.f1
- sql13.swb.cdw.source.f1
- sql13.swb.cdw.selectdatabaseobjects.f1
- sql13.swb.cdw.complete.f1
- sql13.swb.cdw.welcome.f1
helpviewer_keywords:
- Copy Database Wizard
- starting Copy Database Wizard
ms.assetid: 7a999fc7-0a26-4a0d-9eeb-db6fc794f3cb
author: stevestein
ms.author: sstein
ms.openlocfilehash: 6fdb28c8ef89b9c8b79b482b0887f9a0bf40cbb3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88487060"
---
# <a name="use-the-copy-database-wizard"></a>使用复制数据库向导
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
复制数据库向导可将数据库和某些服务器对象从一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  实例轻松移动或复制到另一个实例，而且无需服务器停机。 使用此向导可执行以下操作： 
  
-   选取源服务器和目标服务器。  
  
-   选择要移动或复制的数据库。  
  
-   为数据库指定文件位置。  
  
-   将登录名复制到目标服务器。  
  
-   复制其他支持的对象、作业、用户定义的存储过程和错误消息。  
  
-   计划何时移动或复制数据库。  
  

##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   在 Express 版本中未提供复制数据库向导。  
  
-   复制数据库向导不能用于复制或移动以下数据库：
  
    -   系统。
  
    -   标记为进行复制。
  
    -   标记为“无法访问”、“正在加载”、“脱机”、“正在恢复”、“可疑”或处于“紧急模式”。
    
    -   将数据或日志文件存储在 Microsoft Azure 存储空间。
  
-   使用 [FileTables](../../relational-databases/blob/filetables-sql-server.md) 时，不能在同一服务器上使用复制数据库向导，因为该向导使用同一目录名。

-   无法将数据库移动或复制到较早版本的 SQL Server。
  
-   如果选择 **“移动”** 选项，则在移动数据库之后，该向导将自动删除源数据库。 如果选择 **“复制”** 选项，则复制数据库向导不会删除源数据库。  此外，将复制所选服务器对象，而不是将其移至目标；数据库是唯一实际移动的对象。
  
-   如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象方法移动全文目录，则必须在移动后重新填充索引。  
  
-   **分离和附加** 方法可分离数据库，移动或复制数据库 .mdf、.ndf、.ldf 文件，并在新的位置重新附加该数据库。 对于 **分离和附加** 方法，若要避免数据丢失或不一致，不能将活动会话附加到正在移动或复制的数据库。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象方法，由于数据库从不会脱机，因此允许活动会话。  

-    如果传输的 SQL Server 代理作业引用目标服务器上尚不存在的数据库，将导致整个操作失败。  向导在创建数据库之前，会尝试创建 SQL Server 代理作业。  解决方法如下：
     1. 在目标服务器上创建与要复制或移动的数据库同名的 Shell 数据库。  请参阅 [创建数据库](../../relational-databases/databases/create-a-database.md)。
     
     2. 从“配置目标数据库”页中，选择“删除目标服务器上同名的任何数据库，然后继续传输数据库，覆盖现有数据库文件”。

> **重要说明!!** **分离和附加** 方法将导致源数据库和目标数据库所有权设置为执行 **复制数据库向导** 的登录名。  若要更改数据库的所有权，请参阅 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md) 。
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
-   确保在目标服务器上启动了 SQL Server 代理。  

-   确保可以从目标服务器访问源服务器上的数据和日志文件目录。

-   在 **分离和附加** 方法下，目标服务器上必须存在用于 SSIS 子系统的 SQL Server 代理的代理，并且该代理具有可以访问源服务器和目标服务器的文件系统的凭据。 有关代理的详细信息，请参阅 [创建 SQL Server 代理的代理帐户](~/ssms/agent/create-a-sql-server-agent-proxy.md)。

> **重要说明!!** 在 **分离和附加** 方法下，如果不使用 Integration Services 代理帐户，复制或移动过程将失败。  在某些情况下，源数据库不会重新附加到源服务器，并将从数据和日志文件中去除所有 NTFS 安全权限。  如果发生这种情况，请导航到你的文件，重新应用相关权限，然后再将数据库重新附加到 SQL Server 实例。
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   为了确保升级后的数据库具有最佳性能，请对升级后的数据库运行 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) （更新统计信息）。  
  
-   将数据库移动或复制到另一个服务器实例时，为了给用户和应用程序提供一致的体验，你可能需要在另一个服务器实例上重新创建该数据库的部分或全部元数据（例如登录名和作业）。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  

  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 您必须是源服务器和目标服务器上 **sysadmin** 固定服务器角色的成员。  
  
##  <a name="the-copy-database-wizard-pages"></a><a name="Overview"></a> 复制数据库向导页 
从 SQL Server Management Studio 的 **对象资源管理器** 中启动 **复制数据库向导**，并展开“数据库”。  然后右键单击某个数据库，指向“任务”，再单击“复制数据库”。  如果显示“欢迎使用复制数据库向导”初始页，则单击“下一步”。


### <a name="select-a-source-server"></a>选择源服务器
用于指定要移动或复制的数据库所在的服务器并输入登录信息。  在选择身份验证方法并输入登录信息之后，单击 **“下一步”** 即可与源服务器建立连接。  此连接会在整个会话过程中保持打开状态。

-    **源服务器**  
用于标识想要移动或复制的数据库所在的服务器的名称。  手动输入，或单击省略号以导航到所需的服务器。  该服务器必须至少为 SQL Server 2005。

-    **Use Windows Authentication**  
允许用户通过 Microsoft Windows 用户帐户进行连接。

-    **Use SQL Server Authentication**  
允许用户通过提供 SQL Server 身份验证用户名和密码进行连接。

     -    **用户名**  
用于输入连接所使用的用户名。 只有在已选择使用 **SQL Server 身份验证** 进行连接的情况下，此选项才可用。

     -    **密码**  
用于输入登录名的密码。 只有在已选择使用 **SQL Server 身份验证** 进行连接的情况下，此选项才可用。

###  <a name="select-a-destination-server"></a>选择目标服务器
用于指定将数据库移动或复制到的服务器。  如果将源服务器和目标服务器设置为同一个服务器实例，则会创建一个数据库副本。  在此情况下，必须稍后在向导中重命名数据库。  仅当目标服务器上不存在名称冲突时，源数据库名称才能用于复制或移动的数据库。  如果存在名称冲突，则必须在目标服务器上手动解决冲突问题，然后才能在此处使用源数据库名称。
  
-    **目标服务器**  
用于标识想要将数据库移动或复制到的服务器的名称。  手动输入，或单击省略号以导航到所需的服务器。  该服务器必须至少为 SQL Server 2005。

     >**注意** 可以使用群集服务器作为目标；复制数据库向导将确保你只选择群集目标服务器上的共享驱动器。

-    **Use Windows Authentication**  
允许用户通过 Microsoft Windows 用户帐户进行连接。

-    **Use SQL Server Authentication**  
允许用户通过提供 SQL Server 身份验证用户名和密码进行连接。

     -    **用户名**  
用于输入连接所使用的用户名。 只有在已选择使用 **SQL Server 身份验证** 进行连接的情况下，此选项才可用。

     -    **密码**  
用于输入登录名的密码。 只有在已选择使用 **SQL Server 身份验证** 进行连接的情况下，此选项才可用。

###  <a name="select-the-transfer-method"></a>选择传输方法
 
-    **使用分离和附加方法**  
从源服务器上分离数据库，将数据库文件（.mdf、.ndf 和 .ldf）复制到目标服务器，然后在目标服务器上附加数据库。 此方法通常较快，因为其主要任务只是读取源磁盘和写入目标磁盘。 无需使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 逻辑在数据库中创建对象或创建数据存储结构。 但是，如果数据库包含大量已分配但未使用的空间，此方法会比较慢。 例如，对于一个在创建时分配了 100 MB 空间的几乎为空的新数据库，即使只使用了 5 MB 空间，也会复制全部 100 MB 空间。

     > **注意** 如果使用此方法，用户将无法在传输过程中使用数据库。

     -    **如果失败，则重新附加源数据库**  
     数据库复制之后，原始数据库文件将始终重新附加到源服务器。 如果无法完成数据库移动，请使用此框将原始文件重新附加到源数据库。 
  
-    **使用 SQL 管理对象方法**  
     此方法读取源数据库上每个数据库对象的定义，在目标数据库上创建每个对象。 然后，它从源表向目标表传输数据，重新创建索引和元数据。
  
     > [!NOTE]
     >  数据库用户可以在传输过程中继续访问数据库。 
  
###  <a name="select-database"></a>选择数据库
选择要从源服务器移动或复制到目标服务器的数据库。  请参阅主题顶部的 [限制和局限](#Restrictions) 。  
  
-    **移动**  
将数据库移动到目标服务器。

-    **复制**  
将数据库复制到目标服务器。

-    **Source**  
显示源服务器上的数据库。

-    **Status**  
显示源数据库的各种信息。

-    **“刷新”**  
刷新数据库列表。
  
### <a name="configure-destination-database"></a>配置目标数据库
更改数据库名称（如果适用）并指定数据库文件的位置和名称。  在移动或复制每个数据库时都会出现此页。

-    **源数据库**  
源数据库的名称。  此文本框不可编辑。

-    **目标数据库**  
要创建的目标数据库的名称，根据需要进行修改。

-    **目标数据库文件:**  
     
     -    **Filename**  
要创建的目标数据库文件的名称，根据需要进行修改。

     -    **大小(MB)**  
目标数据库文件的大小（以兆字节为单位）。

     -    **目标文件夹**  
目标服务器上用于托管目标数据库文件的文件夹，根据需要进行修改。

     -    **Status**  
状态

-    **如果目标数据库已存在:**  
     确定目标数据库已存在时要执行的操作。

     -    **如果目标上已存在同名的数据库或文件则停止传输。**

     -    **删除目标服务器上同名的任何数据库，然后继续传输数据库，覆盖现有数据库文件。**

###  <a name="select-server-objects"></a>选择服务器对象 
只有当源和目标为不同服务器的时候，此页才可用。  
  
-    **可用相关对象**  
列出可供传输到目标服务器的对象。  若要包含某个对象，请在“可用相关对象”框中单击对象名称，然后单击 **>>** 按钮，将该对象移动到“所选相关对象”框中。 

-    **所选相关对象**  
列出将传输到目标服务器的对象。  若要排除某个对象，请在“所选相关对象”框中单击对象名称，然后单击 **<\<** 按钮，将该对象移动到“可用相关对象”框中。  默认情况下，将传输每个所选类型中的所有对象。 若要选择某个类型中的单个对象，请在 **“所选相关对象”** 框中单击该对象类型旁边的省略号按钮。  这将打开一个对话框，您可以从中选择各个对象。

-    **服务器对象列表** 

      -    **登录名（默认为选中状态。）** 
  
     -    **SQL Server 代理作业**  
  
     -    **用户定义的错误消息**  
  
     -    **Endpoints**  
  
     -    **全文目录** 
  
     -    **SSIS 包**  
     
     -    **master 数据库中的存储过程** 
          >**注意** 不能对扩展存储过程及其相关的 DLL 进行自动复制。  
    
  
###   <a name="location-of-source-database-files"></a>源数据库文件的位置
只有当源和目标为不同服务器的时候，此页才可用。  指定源服务器上包含数据库文件的文件系统共享。
  
-    **Database**  
     显示正在移动的每个数据库的名称。  
  
-    **文件夹位置**  
源服务器上数据库文件所在的文件夹位置。
例如：`C:\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\DATA`。

  
-    **源服务器上的文件共享**  
包含源服务器上的数据库文件的文件共享。  手动输入共享，或单击省略号以导航到共享。
例如：`\\server_name\C$\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\Data`。

### <a name="configure-the-package"></a>配置包
复制数据库向导将创建 SSIS 包以传输数据库。

-    **包位置**  
显示 SSIS 包的写入位置。

-    **包名称**  
将创建的 SSIS 包的默认名称，根据需要进行修改。

-    **日志记录选项**  
选择是将日志记录信息存储在 Windows 事件日志中还是文本文件中。

-    **错误日志文件路径**  
只有在选择了文本文件日志记录选项后，才能使用此选项。  提供日志文件位置的路径。 

### <a name="schedule-the-package"></a>安排运行包
指定希望何时启动移动或复制操作。  如果你不是系统管理员，则必须指定有权访问 Integration Services (SSIS) 包执行子系统的 SQL Server 代理的代理帐户。

> **重要说明!!** 在 **分离和附加** 方法下，必须使用 Integration Services 代理帐户。  

-    **Run immediately**  
     SSIS 包将在完成向导后执行。
  
-    **计划**  
     SSIS 包将按照计划执行。 
  
     -    **更改计划**   
打开“新建作业计划”对话框。  根据需要进行配置。  完成后，单击“确定”。


-    **Integration Services 代理帐户** 从下拉列表中选择可用的代理帐户。  若要计划传输，则必须至少有一个代理帐户可供用户使用，而且必须将该帐户配置为拥有对 **SSIS 包执行子系统** 的权限。

        若要为 SSIS 包执行创建代理帐户，请在 **对象资源管理器** 中，依次展开“SQL Server 代理”、“代理”，再右键单击“执行 SSIS 包”，然后单击“新建代理”。

### <a name="complete-the-wizard"></a>完成向导
显示所选选项的摘要。  单击 **“上一步”** 可以更改选项。  单击“完成”可创建 SSIS 包。 “正在执行操作”页会监视与 **复制数据库向导** 的执行有关的状态信息。

-    **操作**  
 列出要执行的每项操作。

-    **Status**  
 指示操作总体来说是成功还是失败。

-    **消息**  
提供每个步骤返回的任何消息。

##  <a name="examples"></a><a name="Examples"></a> 示例
### <a name="common-steps"></a>**一般步骤** 
无论你选择“移动”还是“复制”，“分离和附加”还是“SMO”，下列五个步骤均相同。  为简洁起见，这些步骤仅在此处列出一次，之后所有示例将从 **步骤 6** 开始。

1.  在“对象资源管理器”  中，连接到一个 SQL Server 数据库引擎实例，然后展开该实例。

2.  展开“数据库”，右键单击所需的数据库，指向“任务”，然后单击“复制数据库...”。

3.  如果显示“欢迎使用复制数据库向导”初始页，则单击“下一步”。

4.  “选择源服务器”页：指定要移动或复制的数据库所在的服务器。  选择身份验证方法。  如果选择“使用 SQL Server 身份验证”，则需要输入登录凭据。  单击 **“下一步”** 即可与源服务器建立连接。  此连接会在整个会话过程中保持打开状态。

5.  “选择目标服务器”页：指定将数据库移动或复制到的服务器。  选择身份验证方法。  如果选择“使用 SQL Server 身份验证”，则需要输入登录凭据。  单击 **“下一步”** 即可与源服务器建立连接。  此连接会在整个会话过程中保持打开状态。

     > **注意** 可以从任何数据库启动复制数据库向导。  可以从源服务器或目标服务器使用复制数据库向导。
  
### <a name="a--move-database-using-detach-and-attach-method-to-an-instance-on-a-different-physical-server--a-login-and-sql-server-agent-job-will-be-moved-as-well"></a>**A.使用分离和附加方法将数据库移动到其他物理服务器上的实例。同时将移动登录名和 SQL Server 代理作业。**  
以下示例会将 `Sales` 数据库、名为 `contoso\Jennie` 的 Windows 登录名和名为 `Jennie's Report` 的 SQL Server 代理作业从 `Server1` 上的 SQL Server 2008 实例移动到 `Server2`上的 SQL Server 2016 实例。  `Jennie's Report` 使用 `Sales` 数据库。  `Sales` 在目标服务器 `Server2`上尚不存在。  `Server1` 将在数据库移动后重新分配到其他团队。
  
6.  如前面的 [限制和局限](#Restrictions)中所述，在传输一个引用目标服务器上尚不存在的数据库的 SQL Server 代理作业时，需在目标服务器上创建一个 Shell 数据库。  在目标服务器上创建一个名为 `Sales` 的 Shell 数据库。 

7.  返回到 **向导** 的“选择传输方法”页：查看并维护默认值。  单击“下一步”。
  
8.  “选择数据库”页：为所需数据库 `Sales` 选中“移动”复选框。  单击“下一步”。
  
9.  “配置目标数据库”页：**向导** 已确定目标服务器上已存在 `Sales`（在前面的 **步骤 6** 中创建），并且已将 `_new` 追加到该“目标数据库”名称。  从“目标数据库”文本框中删除 `_new`。  根据需要更改“文件名”和“目标文件夹”。  选择“删除目标服务器上同名的任何数据库，然后继续传输数据库，覆盖现有数据库文件”。  单击“下一步”。
  
10. “选择服务器对象”页：在“所选相关对象:”面板中，单击“对象名称登录名”的省略号按钮。  在“复制选项”下面，选择“只复制所选登录名:”。  选中“显示所有服务器登录名”的对应框。  选中 `contoso\Jennie` 的对应“登录”框。  单击“确定”。   在“可用相关对象:”面板中，选择“SQL Server 代理作业”，然后单击 **>** 按钮。  在“所选相关对象:”面板中，单击“SQL Server 代理作业”的省略号按钮。  在“复制选项”下面，选择“只复制所选作业”。  选中 `Jennie's Report`的对应框。  单击“确定”。  单击“下一步”。  
  
11. “源数据库文件的位置”页：单击“源服务器上的文件共享”的省略号按钮，并导航到给定文件夹位置的相应位置。  例如，对于文件夹位置 `D:\MSSQL13.MSSQLSERVER\MSSQL\DATA`，请对“源服务器上的文件共享”使用 `\\Server1\D$\MSSQL13.MSSQLSERVER\MSSQL\DATA`。  单击“下一步”。
  
12. “配置包”页：在“包名称:”文本框中输入 `SalesFromServer1toServer2_Move`。  选中“是否保存传输日志?”框。  在“日志记录选项”下拉列表中，选择“文本文件”。  记下“错误日志文件路径”；根据需要进行修改。  单击“下一步”。  
  
     > **注意**“错误日志文件路径”是目标服务器上的路径。
  
13. “安排运行包”页：从“Integration Services 代理帐户”下拉列表中选择相关的代理。  单击“下一步”。

14. “完成该向导”页：查看所选选项的摘要。  单击 **“上一步”** 可以更改选项。  单击“完成”可执行任务。  在传输过程中，“正在执行操作”页会监视与 **向导** 的执行有关的状态信息。

15. “正在执行操作”页：如果操作成功，请单击“关闭”。  如果操作不成功，请查看错误日志，并且可能需要单击“返回”以便进一步查看。  否则，请单击“关闭”。
  
16. **移动后步骤** 请考虑在新主机 `Server2`上执行以下 T-SQL 语句：
  
     ~~~ tsql 
     ALTER AUTHORIZATION ON DATABASE::Sales TO sa;

     ALTER DATABASE Sales 
     SET COMPATIBILITY_LEVEL = 130;

     USE Sales
     GO

     EXEC sp_updatestats;
     ~~~
 
17. **移动后步骤清理**  
由于 `Server1` 将移动到其他团队，并且“移动”操作不会重复，因此，请考虑执行以下步骤：
     -    删除 `SalesFromServer1toServer2_Move` 上的 SSIS 包 `Server2`。
     -    删除 `SalesFromServer1toServer2_Move` 上的 SQL Server 代理作业 `Server2`。
     -    删除 `Jennie's Report` 上的 SQL Server 代理作业 `Server1`。
     -    删除 `contoso\Jennie` 上的登录名 `Server1`。


### <a name="b-----copy-database-using-detach-and-attach-method-to-the-same-instance-and-set-recurring-schedule"></a>**B.   使用分离和附加方法将数据库复制到同一实例并设置定期计划。**  
在本示例中，将复制 `Sales` 数据库，并在同一实例上将其创建为 `SalesCopy` 。  此后，将每周重新创建一次 `SalesCopy`。

6.  “选择传输方法”页：查看并维护默认值。  单击“下一步”。

7.  “选择数据库”页：为 `Sales` 数据库选中“复制”复选框。  单击“下一步”。

8.  “配置目标数据库”页：将“目标数据库”名称更改为 `SalesCopy`。  根据需要更改“文件名”和“目标文件夹”。  选择“删除目标服务器上同名的任何数据库，然后继续传输数据库，覆盖现有数据库文件”。  单击“下一步”。

9.  “配置包”页：在“包名称:”文本框中输入 `SalesCopy Weekly Refresh`。  选中“是否保存传输日志?”框。  单击“下一步”。

10. “安排运行包”页：单击“计划:”单选按钮，然后单击“更改计划”按钮。 
 
    1. “新建作业计划”页：在“名称”文本框中输入 `Weekly on Sunday`。 
          
    2. 单击“确定”。 

11. 从“Integration Services 代理帐户”下拉列表中选择相关的代理。  单击“下一步”。

12. “完成该向导”页：查看所选选项的摘要。  单击 **“上一步”** 可以更改选项。  单击“完成”可执行任务。  在包创建期间，“正在执行操作”页会监视与 **向导** 的执行有关的状态信息。

13. “正在执行操作”页：如果操作成功，请单击“关闭”。  如果操作不成功，请查看错误日志，并且可能需要单击“返回”以便进一步查看。  否则，请单击“关闭”。

14. 手动启动新创建的 SQL Server 代理作业 `SalesCopy weekly refresh`。  查看作业历史记录，并确保实例上现在存在 `SalesCopy` 。

  
##  <a name="follow-up-after-upgrading-a-database"></a><a name="FollowUp"></a> 跟进：升级数据库之后  
 使用复制数据库向导将数据库从早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]后，该数据库将立即变为可用，然后自动升级。 如果数据库具有全文检索，升级过程将导入、重置或重新生成它们，具体取决于 **全文升级选项** 服务器属性的设置。 如果将升级选项设置为“导入”或“重新生成”，在升级过程中将无法使用全文检索。 导入可能需要数小时，而重新生成所需的时间最多时可能十倍于此，具体取决于要编制索引的数据量。 另请注意，当升级选项设置为“导入”时，如果全文目录不可用，将重新生成关联的全文检索。 有关查看或更改“全文升级选项”属性设置的信息，请参阅[管理和监视服务器实例的全文搜索](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)。  
  
 如果升级前用户数据库的兼容级别为 100 或更高，升级后将保持相应级别。 如果兼容级别为 90，则在升级后的数据库中，兼容级别将设置为 100，该级别为 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]支持的最低兼容级别。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
 
 ## <a name="post-copy-or-move-considerations"></a><a name="Post"></a> 复制或移动后的注意事项
 请考虑在“复制”或“移动”之后是否要执行以下步骤：
-    使用分离和附加方法时更改数据库的所有权。
-    在“移动”后删除源服务器上的服务器对象。
-    删除由向导在目标服务器上创建的 SSIS 包。
-    删除由向导在目标服务器上创建的 SQL Server 代理作业。

  
## <a name="more-information"></a>详细信息！ 
 [使用分离和附加来升级数据库 (Transact-SQL)](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)   
 [创建 SQL Server 代理的代理帐户](../../ssms/agent/create-a-sql-server-agent-proxy.md)  
  
  

