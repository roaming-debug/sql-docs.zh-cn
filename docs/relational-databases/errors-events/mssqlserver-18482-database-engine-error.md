---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ff7d1cf9be26bd9301d16ae21dcbd6af96f57bc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345554"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|18482|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|REMLOGIN_INVALID_SITE|
|消息正文|由于没有将 '%.ls' 定义为远程服务器，所以无法连接到服务器 '%. ls'。 请确保指定的服务器名称正确无误。 %.*ls|
||

## <a name="explanation"></a>说明

当你尝试从一台服务器到另一台服务器执行远程过程调用 (RPC) 时（通过使用诸如 `EXEC SERV_REMOTE.pubs..byroyalty` 之类的语句在远程计算机上执行存储过程），将出现此错误。 将向用户报告如下错误消息

> 错误 18482：由于没有将 \<SERV_REMOTE> 定义为远程服务器，所以无法连接到服务器 \<SERV_REMOTE>。 请确保指定的服务器名称正确无误。

## <a name="cause"></a>原因

当 SQL Server 无法执行远程过程调用时，将发生此错误。 这可能是由未正确配置的本地服务器导致的。 若要进行远程过程调用，SQL Server 首先通过在 sysservers 中查找 srvid = 0 的服务器名称来确定本地服务器的身份。 如果在 sysservers 中找不到具有 srvid = 0 的条目，或者 srvid = 0 的服务器名称属于不同于本地 Windows 计算机名称的服务器名称，则会收到错误。

## <a name="user-action"></a>用户操作

若要确定是否正确配置了本地服务器，请检查 master..sysservers 中的 `srvstatus` 列。 对于本地服务器，此值应为 0。

例如，假设本地服务器名为 SERV_LOCAL，远程服务器名为 SERV_REMOTE，并且 sysservers 包含以下信息：

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

在上面的输出中，SERV_LOCAL 是本地服务器，但它的 srvid 为 1；应为 0。 为更正此错误，请按照下列步骤进行操作：

1. 运行 `sp_dropserver` local_server_name, droplogins（此示例将运行 `sp_dropserver SERV_LOCAL, droplogins`）。
1. 运行 `sp_addserver` local_server_name, LOCAL（此示例将运行 `sp_addserver SERV_LOCAL, LOCAL`）。
1. 停止并重新启动 SQL Server。

运行这些步骤后，sysservers 表应如下所示：

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> 对于本地服务器，服务器 ID (srvid) 应为 0。

在某些情况下，sysservers 表中的条目看似正确，但当你运行 `select @@servername` 时，它将返回 NULL。 在这些情况下，仍应运行上面列出的步骤 1 到步骤 3 来解决此问题。

## <a name="more-information"></a>详细信息

安装复制时可能会收到此错误消息，因为安装过程会在复制所涉及的服务器之间进行远程过程调用。
