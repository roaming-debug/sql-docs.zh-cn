---
title: 还原数据库主密钥 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 和 Transact-SQL 在 SQL Server 中还原数据库主密钥。
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], importing
ms.assetid: 16897cc5-db8f-43bb-a38e-6855c82647cf
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 65441ecd309abbe9b1545de622f0f7d9d4c2d4bc
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250507"
---
# <a name="restore-a-database-master-key"></a>还原数据库主密钥
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中还原数据库主密钥。  
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
  
- 还原主密钥之后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会对使用当前活动的主密钥加密的所有密钥进行解密，然后使用还原后的主密钥对这些密钥进行加密。 这种大量消耗资源的操作应当安排在资源需求较低的时段执行。 如果当前的数据库主密钥未打开或无法打开，或者无法对任何使用该主密钥加密的密钥进行解密，则还原操作将失败。  
  
- 如果有任意一种解密操作失败，则还原操作将会失败。 您可以使用 FORCE 选项忽略错误，但是该选项会使无法进行解密的数据丢失。  
  
- 如果主密钥通过服务主密钥进行加密，则还原后的主密钥也通过该服务主密钥进行加密。  
  
- 如果当前数据库中没有主密钥，则 RESTORE MASTER KEY 将创建一个主密钥。 新的主密钥不会自动使用服务主密钥进行加密。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限
要求对数据库具有 CONTROL 权限。  
  
## <a name="using-sql-server-management-studio-with-transact-sql"></a>将 SQL Server Management Studio 与 Transact-SQL 一起使用  
  
### <a name="to-restore-the-database-master-key"></a>还原数据库主密钥  
  
1. 从物理备份介质或本地文件系统上的某个目录检索备份的数据库主密钥的副本。  
  
2. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
3. 在标准菜单栏上，单击 **“新建查询”** 。  
  
4. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  

    ```sql
    -- Restores the database master key of the AdventureWorks2012 database.  
    USE AdventureWorks2012;  
    GO  
    RESTORE MASTER KEY   
        FROM FILE = 'c:\backups\keys\AdventureWorks2012_master_key'   
        DECRYPTION BY PASSWORD = '3dH85Hhk003#GHkf02597gheij04'   
        ENCRYPTION BY PASSWORD = '259087M#MyjkFkjhywiyedfgGDFD';  
    GO  
    ```  
  
    > [!NOTE]  
    > 指向密钥和密钥的密码（如果有）的文件路径不同于以上所指示的路径。 请确保这两个路径都是针对您的服务器和密钥设置的。  
  
 有关详细信息，请参阅 [RESTORE MASTER KEY (Transact-SQL)](../../../t-sql/statements/restore-master-key-transact-sql.md)。  
