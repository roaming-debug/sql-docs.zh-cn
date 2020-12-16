---
title: 创建数据库主密钥 | Microsoft Docs
description: 使用 Transact-SQL 在 SQL Server 中创建数据库主密钥。 请确保具有所需的权限。
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], creating
ms.assetid: 8cb24263-e97d-4e4d-9429-6cf494a4d5eb
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5a5c3a93771a24a2cf3d1f5debb77b7026bef963
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463118"
---
# <a name="create-a-database-master-key"></a>创建数据库主密钥

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建数据库主密钥。

## <a name="security"></a>安全性

### <a name="permissions"></a>权限

要求对数据库具有 CONTROL 权限。

## <a name="using-transact-sql"></a>“使用 Transact-SQL”

### <a name="to-create-a-database-master-key"></a>创建数据库主密钥

1. 选择密码来对存储于该数据库中的主密钥副本进行加密。
2. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。
3. 展开“系统数据库”，右键单击 `master`，然后单击“新建查询” 。
4. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。

   ```sql
     -- Creates the master key.
     -- The key is encrypted using the password "23987hxJ#KL95234nl0zBe".  
     CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   ```

有关详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)。
