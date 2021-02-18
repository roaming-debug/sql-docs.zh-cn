---
title: SQL Server 属性（“高级”选项卡）
description: 了解“SQL Server 属性”对话框的“高级”选项卡上的选项，例如数据路径、实例 ID 和自定义属性。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2ffd10fd-bac1-478f-9cff-96ed6c8b787f
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 3b62decb0bc8f350fbb1916e7a5fb8779c9878e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345118"
---
# <a name="sql-server-properties-advanced-tab"></a>SQL Server 属性（“高级”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  默认情况下， **“高级”** 选项卡中会显示下列属性。 如果定义了自定义属性，则这些属性及其值将显示在此选项卡上。  
  
## <a name="options"></a>选项  
 **群集**  
 指示此服务是否作为群集服务器的资源进行安装。  
  
 **客户反馈报告**  
 指示是否已对此服务启用服务质量监视。 有关客户反馈报告的详细信息，请在联机丛书中搜索主题“错误和使用报告设置”。  
  
 **数据路径**  
 显示所安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]二进制文件路径。  
  
 **转储目录**  
 显示发生错误时内存转储的存放位置。  
  
 **错误报告**  
 当设置为“是”时，如果发生严重错误，Dr. Watson 程序将把相关信息转发给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]，或转发给错误服务器。 有关错误报告的详细信息，请在联机丛书中搜索主题“错误和使用报告设置”。 若要更改此值，请在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中右键单击服务器，再单击“属性”，然后单击“**杂项”。服务器设置”** 页。 这些选项将显示在 **“信息报告”** 区域中。  
  
 **文件版本**  
 显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可执行文件的版本。  
  
 **安装路径**  
 显示所安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]二进制文件路径。  
  
 **实例 ID**  
 指示使用此服务的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 **语言**  
 显示服务器消息的默认语言。  
  
 **注册表根目录**  
 显示此应用程序所使用的注册表项的位置。  
  
 **Service Pack 级别**  
 显示此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的 Service Pack 级别。  
  
 **SKU 名称**  
 显示产品的单品 (SKU)，有时称为产品版本。  
  
 **引导参数**  
 列出此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例使用的所有引导参数。 这些参数以分号分隔。 默认参数包括 master 数据库的数据文件 (`master.mdf`)、master 数据库的日志文件 (`mastlog.ldf`) 和错误日志文件的路径。 有关引导参数的语法，请在联机丛书中搜索主题“使用 SQL Server 服务启动选项”主题   
  
 **单品**  
 显示产品的单品 (SKU) 编号。  
  
 **版本**  
 显示此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的版本号。  
  
 **虚拟服务器名称**  
 在群集服务器上安装 **后的** 虚拟服务器名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
  
