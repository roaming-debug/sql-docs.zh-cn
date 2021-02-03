---
description: MSSQLSERVER_3169
title: MSSQLSERVER_3169 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "3169"
helpviewer_keywords:
- 3169 (Database Engine error)
ms.assetid: 7d4dbed6-bb94-4908-bc03-2040a9cf63bc
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 176ca9ec55225ea49dbcf6c7fdf5de6c5a2f2192
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194197"
---
# <a name="mssqlserver_3169"></a>MSSQLSERVER_3169
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3169|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|NA|  
|消息正文|该数据库是在运行版本 %ls 的服务器上备份的。 该版本与此服务器(运行版本 %ls)不兼容。 请在支持该备份的服务器上还原该数据库，或者使用与此服务器兼容的备份。|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的某些功能会影响数据库文件的结构。 将数据库还原到另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，文件格式可能会与不同版本的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]不兼容。  
  
例如，如果在较高版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中使用 vardecimal 存储格式，然后尝试在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 之前的版本中还原数据库文件，便可能导致此错误。  
  
## <a name="user-action"></a>用户操作  
确定在发起服务器上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，右键单击服务器，然后单击“属性”，或在查询窗口中键入 SELECT \@\@VERSION。 通过使用原始版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 打开数据库。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中调查已在原始数据库中启用的功能。 修改这些设置以使该设置能用于数据库要在其中进行还原的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。  
  
