---
title: 更改目标平台并发布数据库项目
description: 了解如何将 SQL Server Data Tools 数据库项目的平台更改为受支持 SQL Server 实例。 了解如何发布数据库项目。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.publish.dialog
- sql.data.tools.publishdacproject
ms.assetid: 6012e120-5f72-4f4f-ae6e-f9a57ae1dea7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8a5a9cec35620f408182af3df75702eb85ce4e27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018457"
---
# <a name="how-to-change-target-platform-and-publish-a-database-project"></a>如何：更改目标平台并发布数据库项目

可以将 SQL Server Data Tools (SSDT) 数据库项目的目标 SQL Server 版本更改为任何受支持的 SQL Server 实例（SQL Server 2005、2008、2008 R2、Microsoft SQL Server 2012 或 SQL Azure）。 通过这样做，可以针对一个项目进行数据库开发，但在需要时将其发布到多种不同风格的 SQL Server 实例中。  
  
SSDT 还通过识别您的目标平台并且自动检测出您的代码中的任何错误（例如，在您为将要发布到 SQL Azure 的项目使用不支持的功能时），对此任务进行简化。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
### <a name="to-change-a-projects-target-platform"></a>更改项目的目标平台  
  
1.  在“解决方案资源管理器”中右键单击你的项目并选择“属性”。 单击左侧的 **“项目设置”** 选项卡以便访问 **“项目设置”** 属性页。  
  
2.  此页中的 **“目标平台”** 下拉列表包含某一数据库项目可发布到的所有支持的 SQL Server 平台。 为此过程，请选择 **SQL Azure**。  
  
### <a name="to-use-platform-validation-when-editing-scripts"></a>在编辑脚本时使用平台验证  
  
1.  在“解决方案资源管理器”中右键单击 Products 表，然后选择“查看代码”以便在 Transact\-SQL 编辑器中打开它。  
  
2.  将 `ON [PRIMARY]` 追加到 `CREATE TABLE` 语句的末尾。  
  
3.  请注意，以下错误将出现在“错误列表”窗格中：SQL70015: 在 SQL Azure 中不支持“文件组引用和分区方案”。  
  
    SSDT 将基于目标平台自动验证您的脚本。 在这种情况下，因为 SQL Azure 不支持文件组，所以 SSDT 将返回错误。 有关 SQL Azure 中不支持的 Transact\-SQL 语句，请参阅[部分支持的 Transact-SQL 语句（Microsoft Azure SQL 数据库）](/previous-versions/azure/ee336267(v=azure.100))。  
  
4.  删除 `ON` 子句。 请注意，该错误将立即从 **“错误列表”** 中消失。  
  
### <a name="to-publish-a-database-project"></a>发布数据库项目  
  
1.  如果您有权访问 SQL Azure 实例，则可以跳到下一步。 否则，在“解决方案资源管理器”中右键单击 TradeDev 项目，然后选择“属性”以便访问“项目设置”属性页。 使用“目标平台”下拉列表选择想要将项目发布到的 SQL Server 平台。  
  
2.  在“解决方案资源管理器”中右键单击 TradeDev 项目，然后选择“发布”。 SSDT 将开始生成您的项目。 如果没有生成错误， **“发布数据库”** 对话框将出现。  
  
3.  在 **“发布数据库”** 对话框中，单击 **“编辑”** 以编辑目标数据库连接。  
  
4.  在“连接属性”对话框中，输入你的 SQL Server 实例名称和用于身份验证的凭据。 在 **“连接到数据库”** 中，输入 **NewTrade**。 这将尝试将您的数据库项目发布到新数据库。 您也可以选择要发布到的现有数据库。 例如，如果选择现有的 TradeDev 数据库，然后在脱机 TradeDev 项目中对这些对象已在进行的所有更改（作为脚本）将传播到实时 TradeDev 数据库中。  
  
    如果您有权对想要发布到的数据库进行任何更改，则按下 **“发布”** 按钮。 但是，如果对生产数据库没有写访问权限，则可以单击“生成脚本”按钮以便生成一个 Transact\-SQL 发布脚本，然后可以将该脚本交给某一 DBA。 然后，该 DBA 可以运行该脚本以更新生产服务器，以便其架构与数据库项目同步。  
  
5.  “数据工具操作”   窗口将显示发布操作的进度，并在出现任何错误时通知你。 在此新窗口中，您还可以根据需要选择查看部署预览、生成的脚本或完整发布结果。  
  
6.  您还可以将发布设置保存到某一配置文件中，以便为将来的发布操作重复使用相同的设置。 为此，在 **“发布数据库”** 对话框中单击 **“将配置文件另存为”** 按钮。 在将来，您可以在想要重新加载现有设置时单击 **“加载配置文件”** 按钮。  
  
7.  请注意 **“数据工具操作”** 窗口中的消息。 单击“创建发布预览…”右侧的“查看预览”链接右侧的“视图预览”链接。这将打开部署预览报表。 如果项目的目标平台并不是项目所发布到的数据库服务器，则 SSDT 将在报表中发出警告。  例如，如果项目的目标平台是 Microsoft SQL Server 2012 并且正在尝试将该项目发布到某一 SQL Server 2008 R2 服务器实例，则你将会在“输出”窗口中看到以下警告：  
  
将 Microsoft SQL Server 2012 指定为目标平台的项目可能会遇到与 SQL Server 2008 的兼容性问题    如果此类项目包含 Microsoft SQL Server 2012 中引入的实体（例如，一个 Sequence 对象），则发布操作将失败。  
  
如果对象谓词对新创建的全文索引使用 CONTAINS 或 FREETEXT，并且使用了事务脚本，则部署将失败 。 如果在部署期间启用包含事务脚本的选项，则当在部署脚本结尾在事务外定义全文索引时在事务内定义过程和视图。 由于脚本中的这一顺序，使用 CONTAINS 或 FREETEXT 的过程或视图将不会针对全文索引进行解析，从而导致部署错误。  
