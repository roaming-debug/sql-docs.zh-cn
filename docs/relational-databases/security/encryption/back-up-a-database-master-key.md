---
title: 备份数据库主密钥 | Microsoft Docs
description: 了解如何使用 Transact-SQL 在 SQL Server 中备份数据库主密钥。 这个重要的密钥加密其他密钥和证书。
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], exporting
ms.assetid: 7ad9a0a0-6e4f-4f7b-8801-8c1b9d49c4d8
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 92e8753033e37561b23f2e1d15cb06cef4b662d7
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250678"
---
# <a name="back-up-a-database-master-key"></a>备份数据库主密钥
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中备份数据库主密钥。 数据库主密钥用于加密数据库里面的其他密钥和证书。 如果主密钥被删除或损坏，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可能无法解密数据库里面的其他密钥，并且使用这些密钥加密的数据就如同丢失一样。 出于这个原因，您应备份数据库主密钥，并将备份存储在另外一个安全的位置。  
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
  
- 主密钥必须为打开状态，因此在备份主密钥之前应对其进行解密。 如果主密钥使用服务主密钥进行加密，则不必显式打开。 但如果主密钥仅使用密码进行加密，则必须显式打开。  
  
- 我们建议您在创建主密钥之后立即对其进行备份，并存储于另外一个安全的位置中。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限
要求对数据库具有 CONTROL 权限。  
  
## <a name="using-sql-server-management-studio-with-transact-sql"></a>将 SQL Server Management Studio 与 Transact-SQL 一起使用  
  
### <a name="to-back-up-the-database-master-key"></a>备份数据库主密钥  
  
1. 在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中，连接到包含需要备份的数据库主密钥的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
2. 选择将用于在备份介质上加密数据库主密钥的密码。 此密码应通过复杂性检查。  
  
3. 获得一个用于存储密钥备份副本的可移动备份介质。  
  
4. 确定将在其下创建密钥备份的 NTFS 目录。 这就是在下一步中指定的要创建文件的位置。 应使用高限制级访问控制列表 (ACL) 来保护目录。  
  
5. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
6. 在标准菜单栏上，单击 **“新建查询”** 。  
  
7. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql
    -- Creates a backup of the "AdventureWorks2012" master key. Because this master key is not encrypted by the service master key, a password must be specified when it is opened.  
    USE AdventureWorks2012;   
    GO  
    OPEN MASTER KEY DECRYPTION BY PASSWORD = 'sfj5300osdVdgwdfkli7';   
  
    BACKUP MASTER KEY TO FILE = 'c:\temp\exportedmasterkey'   
        ENCRYPTION BY PASSWORD = 'sd092735kjn$&adsg';   
    GO  
    ```  
  
    > [!NOTE]  
    > 指向密钥和密钥的密码（如果有）的文件路径不同于以上所指示的路径。 请确保这两个路径都是针对您的服务器和密钥设置的。  
  
8. 将文件复制到备份介质上并验证该副本是否完好。  
  
9. 将备份存储在另外一个安全的位置。  
  
 有关详细信息，请参阅 [OPEN MASTER KEY (Transact-SQL)](../../../t-sql/statements/open-master-key-transact-sql.md) 和 [BACKUP MASTER KEY (Transact-SQL)](../../../t-sql/statements/backup-master-key-transact-sql.md)。  
