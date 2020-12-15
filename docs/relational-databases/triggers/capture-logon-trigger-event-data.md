---
description: 捕获登录触发器事件数据
title: 捕获登录触发器事件数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: e05b1ab4-c10b-402a-9591-f6ec1e3db8c0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 429afe57e13c6eb9285256f54f2e83e2609ff7c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403236"
---
# <a name="capture-logon-trigger-event-data"></a>捕获登录触发器事件数据
[!INCLUDE[tsql-appliesto-ss2008-appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]
  要捕获有关 LOGON 事件的 XML 数据以在登录触发器内部使用，请使用 [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md) 函数。 LOGON 事件将返回以下事件数据架构：  
  
 `<EVENT_INSTANCE>`  
  
 `<EventType>event_type</EventType>`  
  
 `<PostTime>post_time</PostTime>`  
  
 `<SPID>spid</SPID>`  
  
 `<ServerName>server_name</ServerName>`  
  
 `<LoginName>login_name</LoginName>`  
  
 `<LoginType>login_type</LoginType>`  
  
 `<SID>sid</SID>`  
  
 `<ClientHost>client_host</ClientHost>`  
  
 `<IsPooled>is_pooled</IsPooled>`  
  
 `</EVENT_INSTANCE>`  
  
 `<EventType>`  
 包含 `LOGON`。  
  
 `<PostTime>`  
 包含请求建立会话时的时间。  
  
 `<SID>`  
 包含指定登录名的安全标识号 (SID) 的 Base 64 编码二进制流。  
  
 `<ClientHost>`  
 包含建立连接的客户端的主机名。 如果客户端和服务器名称相同，则此值为“`<local_machine>`”。 否则，此值为客户端的 IP 地址。  
  
 `<IsPooled>`  
 如果通过使用连接池重用连接，则此值为 `1` 。 否则，此值为 `0`。  
  
  
