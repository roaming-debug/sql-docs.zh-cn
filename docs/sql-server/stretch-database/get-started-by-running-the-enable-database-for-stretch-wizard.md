---
description: 通过运行“启用数据库延伸向导”开始
title: 通过运行“启用数据库延伸向导”开始
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: quickstart
f1_keywords:
- sql13.swb.stretchwizard.f1
- sql13.swb.stretchwizard.createdatabasecredentials.f1
- sql13.swb.stretchwizard.selectdatabasetables.f1
- sql13.swb.stretchwizard.validatesqlserver.f1
- sql13.swb.stretchwizard.selectazuredeployment.f1
- sql13.swb.stretchwizard.configureazuredeployment.f1
- sql13.swb.stretchwizard.Summary.f1
- sql13.swb.stretchwizard.Results.f1
- sql13.swb.stretchwizard.introduction.f1
helpviewer_keywords:
- Stretch Database, wizard
- Enable Database for Stretch Wizard
ms.assetid: 855dd9fc-f80c-4dbc-bf46-55a9736bfe15
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 74435746bc2ef72741cf25a610cb37d4ac1893b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080988"
---
# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>通过运行“启用数据库延伸向导”开始
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 若要为 Stretch Database 配置数据库，请运行“启用数据库延伸向导”。  本文介绍了需要在该向导中输入的信息和做出的选择。  
  
 若要了解 Stretch Database，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。 
 
 > [!NOTE] 
 > 之后如果要禁用 Stretch Database，请记住，禁用表或数据库的 Stretch Database 不会删除远程对象。 如果希望删除远程表或远程数据库，则需要使用 Azure 管理门户进行删除。 远程对象会继续产生 Azure 成本，直到手动删除它们。 
  
## <a name="launch-the-wizard"></a>启动向导  
  
1.  在 SQL Server Management Studio 的对象资源管理器中，选择你想在其上启用 Stretch 的数据库。  
  
2.  右键单击并选择“任务”，再选择“Stretch”，然后选择“启用”以启动向导。  
  
##  <a name="introduction"></a><a name="Intro"></a> 简介  
 查看向导和必备组件的用途。  
 
 重要的先决条件包括以下内容。
 -   必须是管理员才能更改数据库设置。
 -   必须有 Microsoft Azure 订阅。
 -   你的 SQL Server 必须能够使用远程 Azure 服务器进行通信。
  
 ![Stretch Database 向导的“简介”页](../../sql-server/stretch-database/media/stretch-wizard-1.png "Stretch Database 向导的“简介”页")  
  
##  <a name="select-tables"></a><a name="Tables"></a> 选择表  
 选择要为其启用延伸的表。  
 
具有大量行的表显示于已排序列表的顶端。 向导显示表的列表之前，它会对当前不受 Stretch Database 支持的数据类型对其进行分析。 
  
 ![Stretch Database 向导的“选择表”页](../../sql-server/stretch-database/media/stretch-wizard-2.png "Stretch Database 向导的“选择表”页")  
  
|列|说明|  
|------------|-----------------|  
|（无标题）|选中此列中的复选框可为选定的表启用延伸。|  
|**Name**|指定数据库中表的名称。|  
|（无标题）|此列中的符号可能代表一条警告，不会阻止你为 Stretch 启用所选表。 还有可能代表一个阻止问题，将阻止你为 Stretch 启用所选表 - 例如因为该表使用了不支持的数据类型。 将鼠标悬停于符号上可在工具提示中显示更多信息。 有关详细信息，请参阅 [Stretch Database 限制](../../sql-server/stretch-database/limitations-for-stretch-database.md)。|  
|**已拉伸**|指示该表是否已为 Stretch 启用。|  
|**迁移**|你可以迁移整个表（**整个表**），或在表中现有的列上指定一个筛选器。 若想要使用不同的筛选器函数来选择要迁移的行，请运行 ALTER TABLE 语句以在退出向导后指定筛选器函数。 有关筛选器函数的详细信息，请参阅[通过使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。 有关如何应用函数的详细信息，请参阅[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) 或 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。|  
|**行**|指定表中的行数。|  
|**大小(KB)**|指定表的大小（以 KB 为单位）。|  
  
## <a name="optionally-provide-a-row-filter"></a>选择性地提供行筛选器  
 若想要提供筛选器函数来选择要迁移的行，请在“选择表”页执行以下操作。  
  
1.  在“选择你想要拉伸的表”列表中，在表的行中单击“整个表”。 此时会打开“选择要延伸的行”对话框。  
  
     ![定义基于日期的筛选器谓词](../../sql-server/stretch-database/media/stretch-wizard-2a.png "定义基于日期的筛选器谓词")  
  
2.  在“选择要拉伸的行”对话框中，选择“选择行”。  
  
3.  在“名称字段”中，为筛选器函数提供一个名称。  
  
4.  在 **Where** 子句中，选择表中的某列，然后选择一个运算符并提供一个值。  
  
5.  单击“检查”以测试函数。 如果函数从表中返回结果 - 即如果存在满足条件的待迁移的行 - 该测试会报告“成功”。  

> [!NOTE] 
> 显示筛选器查询的文本框为只读。 无法在文本框中编辑查询。
  
6.  单击“完成”，返回到“选择表”页。  

仅在完成该向导时，才会在 SQL Server 中创建筛选器函数。 届时你可以返回到“选择表”页更改或重命名该筛选器函数。

![定义筛选器谓词后的“选择表”页](../../sql-server/stretch-database/media/stretch-wizard-2b.png "定义筛选器谓词后的“选择表”页")

如果想要使用不同类型的筛选器函数来选择要迁移的行，请执行以下操作之一。  
  
-   退出向导并运行 ALTER TABLE 语句以对表启用 Stretch 并指定筛选器函数。 有关详细信息，请参阅 [为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)。  
  
-   运行 ALTER TABLE 语句以在退出向导后指定筛选器函数。 有关所需步骤，请参阅 [运行向导后添加筛选器函数](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz)。  
  
##  <a name="configure-azure"></a><a name="Configure"></a> 配置 Azure  
  
1.  使用 Microsoft 帐户登录到 Microsoft Azure。  
  
     ![登录到 Azure - Stretch Database 向导](../../sql-server/stretch-database/media/stretch-wizard-3.png "登录到 Azure - Stretch Database 向导")  
  
2.  选择用于 Stretch Database 的现有 Azure 订阅。 

> [!NOTE] 
> 若要在数据库上启用 Stretch，则必须具有正在使用的订阅的管理员权限。 Stretch Database 向导将只显示用户具有管理员权限的订阅。
  
3.  为 Stretch Database 选择要使用的 Azure 区域。
    -   如果创建一个新服务器，则该服务器将在此区域进行创建。  
    -   如果你在所选区域中有现有的服务器，则当你选择“现有服务器”时，向导会将其列出。
  
     为了尽量减少延迟，请选择 SQL Server 所在的 Azure 区域。 有关区域的详细信息，请参阅 [Azure 区域](https://azure.microsoft.com/regions/)。  
  
4.  指定是希望使用现有服务器还是新建 Azure 服务器。  
  
     如果 SQL Server 上的 Active Directory 与 Azure Active Directory 联合，则可以选择使用 SQL Server 的联合服务帐户与远程 Azure 服务器进行通信。 有关此选项要求的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
    -   **新建服务器**  
  
        1.  创建服务器管理员的登录名和密码。  
  
        2.  或者，使用 SQL Server 的联合服务帐户与远程 Azure 服务器进行通信。  
  
         ![创建新的 Azure 服务器 - Stretch Database 向导](../../relational-databases/tables/media/stretch-wizard-4.png "创建新的 Azure 服务器 - Stretch Database 向导")  
  
    -   **现有服务器**  
  
        1.  选择现有的 Azure 服务器。  
  
        2.  选择身份验证方法。  
  
            -   如果选择“SQL Server 身份验证”，请提供管理员登录名和密码。  
  
            -   选择“Active Directory 集成身份验证”  ，以使用 SQL Server 的联合服务帐户与远程 Azure 服务器进行通信。 如果所选服务器未与 Azure Active Directory 集成，则此选项不会出现。
  
         ![选择现有的 Azure 服务器 - Stretch Database 向导](../../sql-server/stretch-database/media/stretch-wizard-5.png "选择现有的 Azure 服务器 - Stretch Database 向导")  
  
##  <a name="secure-credentials"></a><a name="Credentials"></a> 安全凭据  
 必须有一个数据库主密钥，以保护 Stretch Database 用于连接到远程数据库的凭据。  
  
 如果数据库主密钥已存在，请对其输入密码。  
  
 ![屏幕截图显示了 Stretch Database 向导的“安全凭据”页面，其中“密码”文本框为空。](../../sql-server/stretch-database/media/stretch-wizard-6b.PNG "Stretch Database 向导的“安全凭据”页")  
  
 如果数据库没有现有的主密钥，请输入一个强密码以创建数据库主密钥。  
  
 ![屏幕截图显示了 Stretch Database 向导的“安全凭据”页面，其中已填写“新建密码”和“确认密码”文本框。](../../relational-databases/tables/media/stretch-wizard-6.png "Stretch Database 向导的“安全凭据”页")  
  
 有关数据库主密钥的详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md) 和[创建数据库主密钥](../../relational-databases/security/encryption/create-a-database-master-key.md)。 有关向导创建的凭据的详细信息，请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。  
  
##  <a name="select-ip-address"></a><a name="Network"></a> 选择 IP 地址  
 使用子网 IP 地址范围（推荐）或 SQL Server 的公共 IP 地址，在允许 SQL Server 与远程 Azure 服务器通信的 Azure 上创建防火墙规则。  
  
 你在此页上提供的 IP 地址（一个或多个）将告知 Azure 服务器允许通过 Azure 防火墙传递由 SQL Server 启动的传入数据、查询和管理操作。 该向导不会更改 SQL Server 上的防火墙设置中的任何内容。  
  
 ![选择 Stretch Database 向导的“IP 地址”页](../../relational-databases/tables/media/stretch-wizard-7.png "选择 Stretch Database 向导的“IP 地址”页")  
  
##  <a name="summary"></a><a name="Summary"></a>总结  
 查看你输入的值和你在该向导中选择的选项以及 Azure 上的预估成本。 然后选择“完成”  以启用 Stretch。  
  
 ![Stretch Database 向导的“摘要”页](../../sql-server/stretch-database/media/stretch-wizard-8.png "Stretch Database 向导的“摘要”页")  
  
##  <a name="results"></a><a name="Results"></a> 结果  
 查看结果。  
  
 若要监视数据迁移的状态，请参阅[数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)。  
  
 ![Stretch Database 向导的“结果”页](../../sql-server/stretch-database/media/stretch-wizard-9.PNG "Stretch Database 向导的“结果”页")  
  
##  <a name="troubleshooting-the-wizard"></a><a name="KnownIssues"></a> 对向导进行故障排除  
 **Stretch Database 向导失败。**  
  如果延伸数据库尚未在服务器级别启用，而你在不使用系统管理员权限的情况下运行向导来启用延伸数据库，则向导会失败。 让系统管理员在本地服务器实例上启用 Stretch Database，然后再次运行该向导。 有关详细信息，请参阅 [先决条件：在服务器上启用 Stretch Database 的权限](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer)。  
  
## <a name="next-steps"></a>后续步骤  
 为延伸数据库启用其他表。 监视数据迁移并管理已启用 Stretch 的数据库和表。  
  
-   [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) 以启用其他表。  
  
-   [数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) 以查看数据迁移的状态。  
  
-   [暂停和恢复数据迁移 (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [延伸数据库的管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [备份启用了延伸的数据库](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [还原已启用延伸的数据库](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>另请参阅  
 [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
