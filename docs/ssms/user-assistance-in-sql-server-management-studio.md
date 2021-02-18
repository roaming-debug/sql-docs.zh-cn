---
description: SQL Server Management Studio 中的用户帮助
title: SQL Server Management Studio 中的用户帮助
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Help [SQL Server Management Studio]
- SQL Server Management Studio [SQL Server], user assistance
ms.assetid: 3c33a474-e507-4712-86fe-ae40e8370319
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c1d8ad8f855a2888dc1a37fd985f2b1a3bfc0357
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338797"
---
# <a name="user-assistance-in-sql-server-management-studio"></a>SQL Server Management Studio 中的用户帮助
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 通过“帮助”菜单和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书提供用户帮助。 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 中的“帮助”菜单以几种不同的途径提供有关 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的信息。 它还提供了对以前无法在“帮助”环境中使用的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 社区和 MSDN 在线资源的访问。 此外，现在可以将“帮助”环境配置为在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 环境中启动或在其自身的关联外部窗口中启动。  
  
## <a name="the-help-interface"></a>“帮助”界面  
“目录”和“索引”提供了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用户熟悉的功能和界面。 其他选项有：  
  
-   **如何**  
  
    提供链接页的层次结构集，其中包含与常用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 任务相关的有用主题。 通过组件和任务来组织内容，例如“复制”主题等。  
  
-   **搜索**  
  
    使用或不使用预定义的筛选器来搜索主题。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的“搜索”是一个单独的选项卡式页。 用户可以使用一个或多个预定义的主题类型、语言或技术筛选器来限定搜索。 默认情况下，“搜索”不使用任何预定义的筛选器，并且只搜索已安装集合中的主题。  
  
    用户可以启用联机帮助，以在搜索中包括在线资源。 有关详细信息，请参阅本主题后面的“MSDN Online 和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 社区”。  
  
-   **动态帮助**  
  
    当用户在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 环境中工作时，自动显示相关信息的链接。  
  
-   **帮助收藏夹**  
  
    存储用户主题书签，以便于以后访问。  
  
对“帮助”的帮助（[!INCLUDE[msCoName](../includes/msconame_md.md)] Document Explorer 帮助）可将用户链接到有关帮助查看器的文档，但这些主题位于一个独立于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书的集合中。 有关帮助查看器的信息，请从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书的“帮助”菜单上选择“帮助之帮助”。  
  
## <a name="msdn-online-and-sql-server-communities"></a>MSDN Online 和 SQL Server 社区  
[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 中的“帮助”还允许用户通过访问网站上的 MSDN Online 和专门针对于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的社区两种途径来获取信息。 可以执行以下操作：  
  
-   从“如何实现”页访问 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 社区。  
  
-   搜索 MSDN Online 和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 社区站点。  
  
#### <a name="to-access-sql-server-focused-communities-from-the-how-do-i-page"></a>从“如何实现”页访问专门针对于 SQL Server 的社区  
  
1.  在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中的“帮助”菜单上，单击“如何实现”。  
  
2.  此时会打开“操作方法[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ”页。 在“社区链接”侧栏中，单击要访问的社区站点的名称。  
  
    > [!NOTE]  
    > 运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的计算机必须直接连接到 Web。  
  
    在搜索 MSDN Online 或 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 社区之前，必须先启用联机搜索。  
  
#### <a name="to-enable-online-search"></a>启用联机搜索  
  
1.  在 **“工具”** 菜单上，单击 **“选项”** 。 在“选项”对话框中，展开“环境”和“帮助”节点（如有必要），然后单击“联机”。  
  
2.  在“当载入帮助内容时”区域中，选择一个联机选项。  
  
3.  在“搜索这些提供程序”列表中，选择要搜索的搜索提供程序，并清除不需要的搜索提供程序。  
  
4.  如果 **Codezone 社区** 是你选定的搜索提供程序之一，则可根据需要在“Codezone 社区”列表中选择和清除项。  
  
5.  单击“确定”。  
  
#### <a name="to-search-msdn-online-and-sql-server-focused-communities-from-the-search-page"></a>从“搜索”页搜索 MSDN Online 和专门针对于 SQL Server 的社区  
  
1.  在“帮助”菜单上，单击“搜索”。  
  
2.  在“搜索”文本框中输入搜索字词，然后单击“搜索”。  
  
无论是否使用可用的筛选器（技术、语言和主题类型）执行搜索，搜索都将立即对选择的所有搜索提供程序执行。  
  
## <a name="launching-help"></a>启动帮助  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]中有两种可以显示帮助的方法。 默认情况下，从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中打开 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]联机丛书时，该丛书将从 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 环境以外的文档窗口中打开。 此窗口仍与 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]相关联；它可以对一些 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 事件做出响应；当您关闭 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]时，联机丛书也会关闭。 当您使用两台监视器时，以这种方式“打开联机丛书”特别有用；您可以将联机丛书窗口拖到第二台监视器中，不干扰您在第一台监视器中所进行的工作，并且便于引用。  
  
还可以在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]中将联机丛书作为文档窗口打开。 当屏幕空间有限，并且要使用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 及其隐藏窗口的功能时，最好使用这种方法。  
  
> [!NOTE]  
> 若要使联机丛书完全独立于 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]，请从“开始”菜单打开 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书，这样它便不再响应你在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 环境中执行的操作，也不会在你退出 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 时关闭。  
  
#### <a name="to-configure-help-and-sql-server-books-online-to-launch-inside-the-management-studio-window"></a>将帮助和 SQL Server 联机丛书配置为在 Management Studio 窗口中启动  
  
1.  在“工具”菜单上，单击“选项”，依次展开“环境”和“帮助”，然后单击“常规”。  
  
2.  在“使用下列选项显示帮助”框中，单击“集成帮助查看器”。  
  
