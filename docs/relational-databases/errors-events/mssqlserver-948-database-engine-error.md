---
description: MSSQLSERVER_948
title: MSSQLSERVER_948 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "948"
helpviewer_keywords:
- 948 (Database Engine error)
ms.assetid: 95c4ad45-a518-4165-a5c4-6e6b932b0570
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ca8e3c0b4053378d4ec442e993b3626b271a0bfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187652"
---
# <a name="mssqlserver_948"></a>MSSQLSERVER_948
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|948|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|NA|  
|消息正文|数据库 '%.*ls' 的版本为 %d，无法打开。 此服务器支持 %d 版及更低版本。 不支持降级路径。|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的某些功能会影响数据库文件的结构。 将数据库附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的另一个实例时，文件格式可能与不同版本的[!INCLUDE[ssDE](../../includes/ssde-md.md)]不兼容。  
  
例如，如果在较高版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中使用 vardecimal 存储格式，并尝试在低于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 的版本中附加数据库文件，便可能导致此错误。  
  
## <a name="user-action"></a>用户操作  
确定在发起服务器上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，右键单击服务器，然后单击“属性”，或在查询窗口中键入 **SELECT @@VERSION** 。 通过使用原始版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 打开数据库。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中调查已在原始数据库中启用的功能。 修改这些设置以适用于将在其中附加数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。  
  
