---
description: MSSQLSERVER_15404
title: MSSQLSERVER_15404 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15404 (Database Engine error)
ms.assetid: 69677f02-bc81-4e4a-99b8-5c1bd1de36df
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8c6a71643180463fbe6176d73778026cfdae9019
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196911"
---
# <a name="mssqlserver_15404"></a>MSSQLSERVER_15404
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|15404|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SEC_NTGRP_ERROR|  
|消息正文|无法获取有关 Windows NT 组/用户“*user*”的信息，错误代码 *code*。|  
  
## <a name="explanation"></a>说明  
在指定无效主体时，15404 用于身份验证。 或者，Windows 帐户的模拟失败，因为在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 Windows 帐户的域之间没有完全信任关系。  
  
## <a name="user-action"></a>用户操作  
检查该 Windows 主体存在且没有拼写错误。  
  
如果此错误是由于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 Windows 帐户的域之间缺乏完全信任关系所导致的，则执行以下操作之一可更正该错误：  
  
-   使用与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的 Windows 用户处于相同域中的帐户。  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在使用 Network Service 或 Local System 之类的计算机帐户，则包含 Windows 用户的域必须信任该计算机。  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户。  
  
