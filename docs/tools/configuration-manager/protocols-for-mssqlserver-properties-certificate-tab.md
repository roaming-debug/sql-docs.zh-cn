---
title: MSSQLSERVER 属性的协议（“证书”选项卡）
description: 使用“MSSQLSERVER 协议属性”对话框中的“证书”选项卡来选择 SQL Server 的证书，或者查看证书属性。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.computermgr.cert.general.f1
helpviewer_keywords:
- MSSQLSERVER property protocols
ms.assetid: 776addd6-25f3-4875-9a71-064035787090
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7656957aa0a45b4456ae2e27408d3176dd0c5b6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463808"
---
# <a name="protocols-for-mssqlserver-properties-certificate-tab"></a>MSSQLSERVER 属性的协议（“证书”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用“MSSQLSERVER 协议的属性”对话框中的“证书”选项卡可以选择 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 证书，也可以查看证书的属性。 在选择证书之前，所有字段均为空。  
  
 用户的证书存储在本地计算机上。 若要加载供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用的证书，您必须在与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务所用的同一用户帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器。  
  
## <a name="page-header"></a>页眉  
 **视图**  
 通过它可以访问有关证书的其他详细信息。 只有在 **“证书”** 框中选择某个证书后，此选项才可用。 有关证书详细信息的其他信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 文档。  
  
 **Clear**  
 从“证书”框中删除所选证书。  
  
 **证书**  
 由安全提供程序确定的证书名称。 选择某个证书可以在属性网格中查看详细信息。  
  
## <a name="options"></a>选项  
 过期日期  
 证书有效期最后一天的日期。  
  
 友好名称  
 获得证书的个人或证书颁发机构的友好名称或公用名称。  
  
 颁发者  
 有关颁发证书的证书颁发机构的信息。  
  
 颁发给  
 有关证书接收者的信息。  
  
  
