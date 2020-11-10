---
title: 使用分页报表（Web 门户）| Microsoft Docs
description: 了解如何在 Web 门户中查看和管理分页报表的属性。 另请了解如何使用报表生成器创建或编辑分页报表。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: fb0bc38f-dc56-4350-8457-cd135c0346e1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1ebe16bd9b9474b892346ec2a8dd6f25254c6d0b
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243757"
---
# <a name="working-with-paginated-reports-web-portal"></a>使用分页报表（Web 门户）

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

你可以在 Web 门户中查看和管理分页报表的属性。 Web 门户可以将你启动到报表生成器中，以创建或编辑分页报表。  
   
## <a name="create-a-paginated-report"></a>创建分页报表  
  
若要创建新的共享数据集，你可以执行以下操作。  
  
1.  从菜单栏中选择“新建”。  
  
2.  选择“分页报表”。  
  
    ![屏幕截图：已调出“分页报表”选项的“新建”下拉列表。](../reporting-services/media/ssrswebportal-new-report.png)  
  
3.  这将启动报表生成器，或者提示你下载它。  
  
4.  生成报表，然后选择左上方的“保存”图标，将分页报表保存回报表服务器。  
  
## <a name="manage-an-existing-paginated-report"></a>管理现有分页报表  
  
若要管理现有分页报表，可以执行以下操作。  
  
> [!NOTE]
> 如果在文件夹中看不到分页报表，请确保你看的是分页报表。 你可以从 Web 门户右上方的菜单栏中选择“查看”  。 请确保已选中“分页报表”。  
  
1.  选择要管理的数据集旁边的省略号 (…)。  
      
    ![屏幕截图：用户选择数据集的省略号选项。](../reporting-services/media/ssrswebportal-manage-report1.png)  
  
2.  选择“管理”会将你转到编辑屏幕。  
    
    ![屏幕截图：已选择省略号选项，且已显示“管理”选项。](../reporting-services/media/ssrswebportal-manage-report2.png)  
  
## <a name="properties"></a>属性  
  
在属性屏幕中，可以更改分页报表的“名称”和“描述” 。 也可以“删除”、“移动”、“创建链接报表”、“在报表生成器中编辑”、“下载”或“替换”     。  
    
![屏幕截图：“编辑公司销售额”对话框的“属性”屏幕。](../reporting-services/media/ssrswebportal-report-properties.png)  
   
## <a name="parameters"></a>参数  
  
可以修改分页报表的现有参数。 若要添加新参数，必须在报表生成器或 SQL Server Data Tools 中编辑报表。  
  
![屏幕截图：“编辑公司销售额”对话框的“参数”屏幕。](../reporting-services/media/ssrswebportal-report-parameters.png)  
   
## <a name="data-source"></a>数据源  
可以指向共享数据源，或输入自定义数据源的连接信息。  
  
![屏幕截图：“编辑公司销售额”对话框的“数据源”屏幕。](../reporting-services/media/ssrswebportal-report-datasource.png)  
  
下列选项用于指定自定义数据源。  
  
类型  
  
指定用于处理数据源中数据的数据处理扩展插件。 有关内置数据扩展插件的列表，请参阅 [Reporting Services 支持的数据源 (SSRS)]。 其他数据处理扩展插件可能由第三方供应商提供。  
  
**连接字符串**  
  
指定报表服务器用于连接到数据源的连接字符串。 连接类型确定应使用的语法。 例如，XML 数据处理扩展插件的连接字符串是 XML 文档的 URL。 在大多数情况下，常见的连接字符串指定数据库服务器和数据文件。 以下示例演示用于连接到名为 MyData 的 SQL Server 数据库的连接字符串：  
  
`data source=(a SQL Server instance);initial catalog=MyData`
  
可以将连接字符串配置为表达式，以便可以在运行时指定数据源。 数据源表达式是在报表设计器中的报表中定义的。 不能在 Web 门户中定义、查看或修改数据源表达式。 但可以通过单击 **“覆盖默认值”** ，键入静态连接字符串来替换数据源表达式。 如果要切换回原来的表达式，请单击“恢复到默认值”。 报表服务器将存储原始连接字符串，以便在需要时还原。 若要使用数据源表达式，必须使用最初在报表中发布的数据源连接信息。 共享数据源不支持在连接字符串中使用表达式。  
  
**凭据**  
  
可以指定用于确定凭据获取方式的选项。  
  
> [!IMPORTANT]
> 如果连接字符串中提供了凭据，则忽略此部分中提供的选项和值。 请注意，如果在连接字符串中指定了凭据，则会向浏览此页的所有用户以明文显示这些值。  
  
**以用户身份查看报表**  
  
使用当前用户的 Windows 凭据来访问数据源。 如果用于访问数据源的凭据与用于登录到网络域的凭据相同，请选择此选项。 如果您的域启用了 Kerberos 身份验证或者数据源与报表服务器位于同一台计算机上，则此选项最为有效。 如果未启用 Kerberos，则不能将 Windows 凭据传递到另一个服务或远程计算机。 如果需要其他计算机连接，您将得到错误提示而不是所需的数据。  
  
报表服务器管理员可以禁止使用 Windows 集成安全性来访问报表数据源。 如果此值显示为灰色，则说明该功能不可用。  
  
如果您打算计划或订阅此报表，请不要使用此选项。 计划的或无人参与的报表处理需要可以在没有用户输入或当前用户的安全上下文的情况下获取的凭据。 只有存储的凭据才能提供此功能。 因此，如果报表配置为 Windows 集成安全性凭据类型，则报表服务器会禁止您安排对报表或订阅的处理。 如果为已订阅的报表或者具有计划操作的报表选择此选项，则订阅和计划操作将停止。  
  
**使用这些凭据**  
  
在报表服务器数据库中存储加密的用户名和密码。 选择此选项可在无人参与的模式下运行报表（例如，通过计划或事件而不是用户操作来启动报表）。   
  
还可以选择凭据的类型： Windows 身份验证（Windows 用户名和密码），或者特定数据库凭据（数据库用户名和密码），例如 SQL 身份验证。  
  
如果帐户为 Windows 凭据，指定的帐户对用来托管报表所用数据源的计算机必须具有本地登录权限。  
  
选择“使用这些凭据登录，然后尝试模拟用户查看报表”可允许委托凭据，但前提是数据源支持模拟。 对于 SQL Server 数据库，此选项设置“SETUSER”功能 对于 Analysis Services，此选项使用 EffectiveUserName。  
  
**通过提示用户查看报表以获取凭据**  
  
每一名用户都必须键入用户名和密码以访问数据源。 可以定义请求用户凭据的提示文本。 例如，“输入用户名和密码以访问数据源。”  
  
还可以选择凭据的类型： Windows 身份验证（Windows 用户名和密码），或者特定数据库凭据（数据库用户名和密码），例如 SQL 身份验证。  
  
**没有任何凭据**  
  
此选项允许你不为数据源提供任何凭据。 如果数据源要求用户登录，则选择此选项将不起任何作用。 只有在数据源连接不需要用户凭据的情况下，才应选择此选项。  
  
若要使用此选项，则以前必须为报表服务器配置过无人参与的执行帐户。 当其他凭据源不可用时，可以使用无人参与的执行帐户连接到外部数据源。 如果您指定此选项，但是未配置无人参与的执行帐户，则到报表数据源的连接将失败，而且将不会进行报表处理。 有关此帐户的详细信息，请参阅[配置无人参与的执行帐户（报表服务器配置管理器）](../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)。  
  
## <a name="subscriptions"></a>订阅  
Reporting Services 订阅是一种配置，它在特定时间或为响应某个事件，以指定的文件格式传递报表。 例如，每周三将 MonthlySales.rdl 报表作为 Microsoft Word 文档保存至文件共享。 订阅可以用于对报表的传递（以特定报表参数值集）进行计划并使其自动完成。 有关详细信息，请参阅[使用订阅](working-with-subscriptions-web-portal.md)。
  
![屏幕截图：“编辑公司销售额”对话框的“订阅”屏幕。](../reporting-services/media/ssrswebportal-report-subscription1.png)
   
## <a name="dependent-items"></a>依赖项  
使用“依赖项”页可以查看引用此报表的项的列表。 每个项类型的图标都指示了它是什么。 然后，可以选择每个项对应的省略号 (…) 进一步管理这些项。  
  
## <a name="caching"></a>Caching  
缓存分页报表的数据时，你有多个选项。 只需进行简单选择即可开始操作。  
  
1.  **始终用最新数据运行此报表** 在每次运行报表时都会向数据源发出查询。 这将生成一个包含最新数据的按需报表。 每次打开报表时，都会创建报表的新实例，其中包含新查询的结果。 使用此方法时，如果有十个用户同时打开报表，则会向数据源发送十个要处理的查询。  
  
2.  **缓存此报表的副本并在可用时使用副本** 会将数据的临时副本放在缓存中供以后使用。 由于将从缓存中返回数据（而不是重新运行数据集查询），因此进行缓存通常会提高性能。 使用此方法时，如果有十个用户打开报表，则只有第一个请求会向数据源发出查询。 随后即会缓存报表，这样，其余九个用户将查看缓存的报表。  
  
3.  **始终对预先生成的快照运行此报表** 将缓存给定时间段内的报表布局和数据。 您可以按报表快照形式运行报表，以防止报表在任意时间运行（例如，在执行计划备份期间）。 可以按计划刷新快照。 [了解详细信息]  
  
![屏幕截图：“编辑公司销售额”对话框中的“缓存”屏幕，其中已选中“始终用最新数据运行此报表”选项。](../reporting-services/media/ssrswebportal-report-caching1.png)  
   
选择“缓存此报表的副本并在可用时使用副本”时将显示更多选项。  
  
![屏幕截图：“编辑公司销售额”对话框中的“缓存”屏幕，其中已选中“缓存此报表的副本并在可用时使用副本”选项。](../reporting-services/media/ssrswebportal-report-caching2.png)  

有关详细信息，请参阅[使用快照](working-with-snapshots-web-portal.md)。
  
### <a name="cache-expiration"></a>缓存过期  
  
你可以控制是要在某段时间后针对分页报表使缓存过期，还是希望按计划使缓存过期。 你可以使用共享计划。  
  
> [!NOTE]
> 这不会刷新缓存。  
  
### <a name="cache-refresh-plans"></a>缓存刷新计划  
  
可以使用“缓存刷新计划”创建使用分页报表数据的临时副本预加载缓存的计划。 刷新计划包括计划和指定或覆盖参数值的选项。 不能覆盖标记为只读的参数值。 可以创建和使用多个刷新计划。  
   
通过“内容管理员”、“我的报表”和“发布者”这些默认角色分配，你可以添加、删除和更改缓存刷新计划的分页报表。  
  
应用上面的缓存选项后，可以定义缓存刷新计划。 为此，请选择应用缓存设置后出现的“管理刷新计划”链接。 这会将你转到缓存刷新计划页。   
  
若要创建新的缓存刷新计划，请选择“新建缓存刷新计划”。 然后，可以为该计划输入名称并指定一个计划。 如果数据集定义了参数，这些参数将列出，你可以提供值，除非它们标记为只读。  
  
完成后，你便可以选择“创建缓存刷新计划”。  
  
![屏幕截图：显示了“创建缓存刷新计划”选项的“编辑公司销售额”对话框。](../reporting-services/media/ssrswebportal-report-caching3.png)  
  
> [!NOTE]
> SQL Server 代理必须处于运行状态才能创建缓存刷新计划。  
  
之后，你可以“编辑”  或“删除”  列出的计划。 仅当选择一个缓存刷新计划时才启用“从现有项新建”选项。 此选项将创建一个新的刷新计划，该计划是从原始计划复制而来。 将打开缓存刷新计划页，其中预先填充了所选计划的详细信息。 然后，您可以修改刷新计划选项并用新说明保存该计划。  
  
## <a name="history-snapshots"></a>历史记录快照  
  
使用“历史记录快照”页可以查看一段时间中生成并存储的报表快照。 根据所设置的选项，报表历史记录可能只包含较新的快照。  
  
报表历史记录总是显示在所源于的报表的上下文中。 您不能一起查看报表服务器中所有报表的历史记录。  
  
若要生成快照，报表必须能够以无人参与的方式运行（也就是说，报表必须使用存储的凭据；参数化报表必须包含所有参数的默认参数值）。 可以手动或以计划操作方式生成快照。   
  
您可以通过单击报表历史记录快照来查看它。 报表历史记录中显示的快照只能通过创建的日期和时间来区分。 无法通过直观方式判断出某个快照是为响应计划而生成的还是为响应某个手动操作而生成的。  
  
## <a name="security"></a>安全性  
使用“安全属性”页可以查看或修改安全设置，以确定对报表的访问权限。 此页可用于您有权保护的项。  
  
对项的访问是通过指定某个组或用户可以执行的任务的角色分配来定义的。 角色分配包含一个用户名或组名，以及指定一组任务的一个或多个角色定义。  
  
**编辑项安全设置**  
  
选择此选项可更改为当前项定义安全性的方式。

## <a name="next-steps"></a>后续步骤

[Web 门户](../reporting-services/web-portal-ssrs-native-mode.md)  
[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
