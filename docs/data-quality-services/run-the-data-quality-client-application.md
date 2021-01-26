---
description: 运行数据质量客户端应用程序
title: 运行数据质量客户端应用程序
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.browseforservers.f1
- sql13.dqs.connecttoserver.f1
ms.assetid: 0b2aa202-7ab2-4c9d-b0f1-802588053a1e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: f83334dbbcc50642c463681614db5f23e10f6807
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765645"
---
# <a name="run-the-data-quality-client-application"></a>运行数据质量客户端应用程序

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  运行 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]，并登录到 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 您必须已通过运行 DQSInstaller.exe 文件完成了 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] 安装。 有关详细信息，请参阅 [运行 DQSInstaller.exe 以便完成数据质量服务器安装](../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 您必须具有对 DQS_MAIN 数据库授予的三种 DQS 角色（dqs_adminstrator、dqs_kb_editor 或 dqs_kb_operator）中的一种，才能登录到 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]。  
  
##  <a name="run-data-quality-client"></a><a name="Run"></a> 运行 Data Quality Client  
 若要在安装了 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] 的计算机上运行它，请执行以下操作：  
  
1.  在 " **开始** " 菜单中，选择 " **[!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]** **Data Quality Client**"。  
  
2.  在“连接到服务器”对话框中：  
  
    1.  指定您要将 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] 应用程序连接到的服务器。 选择 **(LOCAL)** 以便连接到本地计算机上的 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] 。 还可以单击向下箭头，选择 **\<Browse network for more servers>** 连接到不同的服务器 (或按名称) 连接到本地服务器。 将显示 **“查找服务器”** 对话框。 您可以在 **“本地服务器”** 选项卡或 **“网络服务器”** 选项卡中选择某一服务器。  
  
    2.  若要加密在 [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] 和 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]之间传输的数据，请单击“选项” ，然后选中“加密连接”  复选框。  
  
3.  单击“连接”  。  
  
 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] 主屏幕将出现。 有关详细信息，请参阅[数据质量客户端主屏幕](../data-quality-services/data-quality-client-home-screen.md)。  
  
  
