---
description: MSSQLSERVER_33129
title: MSSQLSERVER_33129 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33129 (Database Engine error)
ms.assetid: 83b5f368-f1a1-4a40-9bb6-c77e2dec690f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: dd8ba9d3bce316b77db7fa6ae3ec01190607b858
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190911"
---
# <a name="mssqlserver_33129"></a>MSSQLSERVER_33129
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|33129|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SEC_CANNOT_DISABLE_WIN_GROUP|  
|消息正文|不能使用带 DISABLE 参数的 ALTER_LOGIN 来拒绝对 Windows 组的访问。|  
  
## <a name="explanation"></a>说明  
尝试禁用某一 Windows 组的登录名时，会出现此消息。  
  
## <a name="user-action"></a>用户操作  
Windows 组的登录名不能禁用。 若要暂时删除对某一 Windows 组授予的访问权限，则使用 REVOKE 命令吊销对 Windows 组的该登录名的 CONNECT 权限。 Windows 用户仍可能通过其单独的登录名或通过另一个 Windows 组具有访问权限。 下面的示例吊销 WESTCOAST 域的 Accounting 组的连接权限。  
  
```Transact-SQL  
REVOKE CONNECT TO [WESTCOAST\Accounting];  
```  
  
