---
description: MSSQLSERVER_17112
title: MSSQLSERVER_17112
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17112 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 870f9b9f4d3fcc8186ed1d16faee861ed63e4135
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418687"
---
# <a name="mssqlserver_17112"></a>MSSQLSERVER_17112
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|17112|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|INIT_INVCOMMAND|
|消息正文|从注册表或命令提示符提供的启动选项无效。 请更正或删除此选项。|
||

## <a name="explanation"></a>说明

此错误表示指定的[数据库引擎服务启动选项](/sql/database-engine/configure-windows/database-engine-service-startup-options)无效。 如果未正确指定启动选项，则 SQL Server 无法启动或可能不会按预期运行。 还会引发错误 17112。

在某些情况下，实例可能会启动，但当你查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志时，启动参数看起来不正确：

> \<Datetime> 服务器注册表启动参数：  
\<Datetime> Server -d D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\master.mdf  
\<Datetime> Server -e D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\LOG\ERRORLOG  
\<Datetime> Server -l D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\mastlog.ldf  
\<Datetime> Server -T1118 -g512

请注意最后两个启动参数是如何在同一行上的。

在某些情况下，你可能会注意到，添加所需的启动参数不会对服务器行为产生预期的影响。

## <a name="possible-causes"></a>可能的原因

遇到这些问题的原因如下：

- 使用有效启动参数列表中未显示的启动参数
- 指定未带有正确分隔符 [;] 的启动参数
- 从引入了一些不可见的特殊字符（例如，-T 前面的空格）的文本编辑器复制粘贴启动参数
- 没有对启动参数使用正确的大小写格式

## <a name="user-action"></a>用户操作

使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager 工具来提供和验证为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例指定的启动参数。 确保正确分隔每个启动参数且不存在任何特殊字符。

## <a name="more-information"></a>详细信息

请参阅下列主题，详细了解此主题：

- [数据库引擎服务启动选项](/sql/database-engine/configure-windows/database-engine-service-startup-options)
- [SCM 服务 - 配置服务器启动选项](/sql/database-engine/configure-windows/scm-services-configure-server-startup-options)
