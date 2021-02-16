---
description: 由 Integration Services 服务记录的事件
title: 由 Integration Services 服务记录的事件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- service [Integration Services], events
- events [Integration Services], service
- Integration Services service, events
ms.assetid: d4122dcf-f16f-47a0-93a2-ffa3d0d4f9cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 45612ea0bfdbda2b27e6cac8b4deb1e302eb9298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346551"
---
# <a name="events-logged-by-the-integration-services-service"></a>由 Integration Services 服务记录的事件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务将各种消息记入 Windows 应用程序事件日志。 该服务会在服务启动时、服务停止时和特定问题出现时记录这些消息。  
  
 本主题提供有关该服务记入应用程序事件日志的常见事件消息的信息。 本主题中说明的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务记录的所有消息均以 SQLISService 为事件源。  
  
 有关 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的常规信息，请参阅 [Integration Services 服务（SSIS 服务）](../../integration-services/service/integration-services-service-ssis-service.md)。  
  
## <a name="service-status-messages"></a>服务状态消息
 当您选择安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 时，系统将安装并启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务，且其启动类型将设置为自动。  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|256|DTS_MSG_SERVER_STARTING|正在启动 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务。|服务即将启动。|  
|257|DTS_MSG_SERVER_STARTED|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务已启动。|服务已启动。|  
|260|DTS_MSG_SERVER_START_FAILED|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务启动失败。%n错误：%1|服务未能启动。 服务未能启动可能是由于安装已损坏或服务帐户不适当。|  
|258|DTS_MSG_SERVER_STOPPING|正在停止 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务。%n%n退出时停止所有正在运行的包：%1|服务正在停止，如果将服务配置为执行此操作，则所有正在运行的包都会停止。 可以在配置文件中设置 True 或 False 值来决定服务是否在自身停止时停止正在运行的包。 此事件的消息包括这项设置的值。|  
|259|DTS_MSG_SERVER_STOPPED|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务已停止。%n服务器版本 %1|服务已停止。|  
  
## <a name="settings-file-messages"></a>设置文件消息  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的设置存储在一个 XML 文件中，您可以修改该文件。 有关详细信息，请参阅 [Integration Services 服务（SSIS 服务）](../../integration-services/service/integration-services-service-ssis-service.md)。  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务：%n指定配置文件的注册表设置不存在。 %n正尝试加载默认的配置文件。|包含配置文件路径的注册表项不存在或为空。|  
|272|DTS_MSG_SERVER_MISSING_CONFIG|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务配置文件不存在。%n正在使用默认设置加载。|在指定位置不存在配置文件自身。|  
|273|DTS_MSG_SERVER_BAD_CONFIG|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务配置文件不正确。%n读取配置文件 %1 时出错%n%n正在使用默认设置加载服务器。|无法读取配置文件或配置文件无效。 此错误可能是由文件中的 XML 语法错误引起的。|  
  
## <a name="other-messages"></a>其他消息  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|336|DTS_MSG_SERVER_STOPPING_PACKAGE|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务：正在停止运行中的包。%n包实例 ID：%1%n包 ID：%2%n包名称：%3%n包说明：%4%n包|服务正在尝试停止运行中的包。 可以在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中监视和停止正在运行的包。 有关如何在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中管理包的信息，请参阅[包管理（SSIS 服务）](../../integration-services/service/package-management-ssis-service.md)。|  

## <a name="view-events"></a>查看事件
  可以使用以下两个工具来查看 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的事件：  
  
-   **中的** “日志文件查看器” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]对话框。 **“日志文件查看器”** 对话框包括导出、筛选和搜索日志的选项。 有关“日志文件查看器”中选项的详细信息，请参阅[日志文件查看器 F1 帮助](../../relational-databases/logs/log-file-viewer-f1-help.md)。  
  
-   Windows 事件查看器。  
  
 有关由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务记录的事件的说明，请参阅 [由 Integration Services 服务记录的事件](../../integration-services/service/events-logged-by-the-integration-services-service.md)。  
  
### <a name="to-view-service-events-for-integration-services-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中查看 Integration Services 的服务事件  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
2.  在 **“文件”** 菜单上，单击 **“连接对象资源管理器”**。  
  
3.  在 **“连接到服务器”** 对话框中，选择 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器类型，选择或找到要连接到的服务器，然后单击 **“连接”**。  
  
4.  在对象资源管理器中，右键单击 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，再单击“查看日志”。  
  
5.  若要查看 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 事件，请选择 **SQL Server Integration Services**。 如果选择或清除 **SQL Server Integration Services** 选项，则会同时选择或清除 **“NT 事件”** 选项。  
  
### <a name="to-view-service-events-for-integration-services-in-windows-event-viewer"></a>在 Windows 事件查看器中查看 Integration Services 的服务事件  
  
1.  在 **“控制面板”** 中，如果使用的是经典视图，请单击 **“管理工具”**；如果使用的是分类视图，请单击 **“性能和维护”** ，再单击 **“管理工具”**。  
  
2.  单击 **“事件查看器”**。  
  
3.  在 **“事件查看器”** 对话框中，单击 **“应用程序”**。  
  
4.  在“应用程序”管理单元中，找到“源”列中具有值 **SQLISService** 的项，右键单击该项，再单击“属性”。  
  
5.  可以选择单击向上键或向下键来显示上一个事件或下一个事件。  
  
6.  可以选择单击“复制到剪贴板”图标来复制事件信息。  
  
7.  选择使用字节或字来显示事件数据。  
  
8.  单击“确定”。   
  
9. 在 **“文件”** 菜单上，单击 **“退出”** 关闭 **“事件查看器”** 对话框。  
 
## <a name="related-tasks"></a>Related Tasks  
 有关如何查看日志项的信息，请参阅 [Integration Services 包记录的事件](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
