---
description: MSSQLSERVER_15581
title: MSSQLSERVER_15581
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha, VenCher
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 15581 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f1221474d86d95400ca955d64b4a0812cffe1c0d
ms.sourcegitcommit: f87f2f0f1edc91fe400040d8e3a5810347aa8d70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96868890"
---
# <a name="mssqlserver_15581"></a>MSSQLSERVER_15581
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|15581|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|SEC_NODBMASTERKEYERR|
|消息正文|在执行此操作之前，请在数据库中创建一个主密钥或在会话中打开该主密钥。|
||

## <a name="explanation"></a>说明

当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法恢复为透明数据加密 (TDE) 启用的数据库时，将引发错误 15581。 将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中记录如下错误消息

> 2020-01-14 22:16:26.47 spid20s Error:15581, Severity:16，状态：3.  
2020-01-14 22:16:26.47 spid20s 在执行此操作之前，请在数据库中创建一个主密钥或在会话中打开该主密钥。

## <a name="possible-cause"></a>可能的原因

当运行以下命令时，如果删除了 master 数据库中数据库主密钥的服务主密钥加密，则会出现此问题：

```sql
Use master
go
alter master key drop encryption by service master key
```

服务主密钥用于对数据库主密钥使用的证书进行加密。 对使用启用了 TDE 的数据库的任何尝试都需要访问 master 数据库中的数据库主密钥。 必须使用 [OPEN MASTER KEY (Transact-SQL)](/sql/t-sql/statements/open-master-key-transact-sql) 语句，并对需要访问主密钥的每个会话使用一个密码，来打开未使用服务主密钥进行加密的主密钥。 由于此命令不能在系统会话上运行，因此不能在启用了 TDE 的数据库上完成恢复。

## <a name="user-action"></a>用户操作

若要解决此问题，请启用主密钥的自动解密。 为此，请运行以下命令：

```sql
Use master
go
open master key DECRYPTION BY PASSWORD = 'password'
alter master key add encryption by service master key
```

使用以下查询来确定是否已为 master 数据库禁用服务主密钥对主密钥的自动解密：

```sql
select is_master_key_encrypted_by_server from sys.databases where name = 'master'
```

如果此查询返回的值为 0，则禁用了服务主密钥对主密钥的自动解密。

## <a name="more-information"></a>详细信息

在某些情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例可能会无响应。 如果查询 `sys.dm_exec_requests` 动态管理视图，你会注意到 `LogWriter` 线程和正在执行 DML 操作的其他线程正在无限期等待，并显示 WRITELOG wait_type。 其他会话尝试获取锁时也可能正在等待。
