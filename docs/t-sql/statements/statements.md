---
description: Transact-SQL 语句
title: 语句 | Microsoft Docs
ms.custom: ''
ms.date: 04/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: d8d6f62a-e815-425c-a80e-a63fd34ec275
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3ce83d95f98102c087affc91581082857f21250
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481748"
---
# <a name="transact-sql-statements"></a>Transact-SQL 语句

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL 语句是工作的原子单元，要么完全成功，要么完全失败。 SQL 语句是一组指令，由标识符、参数、变量、名称、数据类型和成功编译的 SQL 保留字组成。 如果 `BeginTransaction` 命令未指定启动事务，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 会为 SQL 语句创建隐式事务。 如果此语句成功，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 会始终提交隐式事务；如果此命令失败，则会回滚隐式事务。  

语句有很多种类型。 也许最重要的是 [SELECT](../queries/select-transact-sql.md)，它从数据库中检索行，并支持从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的一个或多个表内选择一个或多个行或列。 本文总结了除 `SELECT` 语句外还用于 Transact-SQL (T-SQL) 的语句类别。 左侧导航栏中列出了所有语句。

## <a name="backup-and-restore"></a>备份和还原

备份和还原语句提供创建备份和从备份中还原的方法。  有关详细信息，请参阅[备份和还原概述](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)。

## <a name="data-definition-language"></a>数据定义语言

数据定义语言 (DDL) 语句定义数据结构。 使用以下语句在数据库中创建、更改或删除数据结构。 这些语句包括：

- ALTER
- 排序规则
- CREATE
- DROP
- DISABLE TRIGGER
- ENABLE TRIGGER
- RENAME
- UPDATE STATISTICS
- TRUNCATE TABLE

## <a name="data-manipulation-language"></a>数据操作语言

数据操作语言 (DML) 影响存储在数据库中的信息。 使用以下语句在数据库中插入、更新和更改行。

- BULK INSERT
- DELETE
- INSERT
- SELECT
- UPDATE
- MERGE

## <a name="permissions-statements"></a>权限语句

权限语句决定哪些用户和登录名可以访问数据并执行操作。 有关身份验证和访问权限的详细信息，请参阅[安全中心](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)。

## <a name="service-broker-statements"></a>Service Broker 语句

Service Broker 是一项功能，可为消息和队列应用程序提供本机支持。 有关详细信息，请参阅 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)。

## <a name="session-settings"></a>会话设置

SET 语句决定当前会话处理运行时设置的方式。 如需查看概述，请参阅 [SET 语句](set-statements-transact-sql.md)。
