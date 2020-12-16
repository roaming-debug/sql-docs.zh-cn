---
description: T-SQL 教程：删除数据库对象
title: T-SQL 教程：删除数据库对象 |Microsoft Docs
ms.custom: ''
ms.date: 07/30/2018
ms.prod: sql
ms.technology: t-sql
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deleting database objects
ms.assetid: ecf26dd5-4535-4ed6-86fc-c73f9d9dedec
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 517042032a8b7b146f02b620ca6d47a90ae074a0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462268"
---
# <a name="lesson-3-delete-database-objects"></a>第 3 课：删除数据库对象
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
本课程很短，它删除您在第 1 课和第 2 课中创建的对象，再删除数据库。  
  
在删除对象之前，请确保使用正确的数据库：
  
  ```sql  
  USE TestData;  
  GO  
  ```  

## <a name="revoke-stored-procedure-permissions"></a>撤销存储过程权限
  
使用 `REVOKE` 语句删除 `Mary` 对存储过程的执行权限：
  
  ```sql  
  REVOKE EXECUTE ON pr_Names FROM Mary;  
  GO  
  ```  
  
## <a name="drop-permissions"></a>删除权限

1. 使用 `DROP` 语句删除 `Mary` 对 `TestData` 数据库的访问权限：
  
   ```sql  
   DROP USER Mary;  
   GO  
   ```  


2. 使用 `DROP` 语句删除 `Mary` 对此 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]实例的访问权限。
  
   ```sql  
   DROP LOGIN [<computer_name>\Mary];  
   GO   
   ```  
  
3. 使用 `DROP` 语句删除存储过程 `pr_Names`：  
  
   ```sql  
   DROP PROC pr_Names;  
   GO   
   ```  
  
4. 使用 `DROP` 语句删除视图 `vw_Names`：  
  
   ```sql  
   DROP VIEW vw_Names;  
   GO  
   ```  

## <a name="delete-table"></a>删除表
  
1. 使用 `DELETE` 语句删除 `Products` 表中的所有行：  
  
    ```sql  
    DELETE FROM Products;  
    GO  
    ```  
  
2.  使用 `DROP` 语句删除 `Products` 表：  
  
    ```sql  
    DROP TABLE Products;  
    GO    
    ```  

## <a name="remove-database"></a>删除数据库
  
正使用 `TestData` 数据库时，无法删除该数据库；因此，请首先将上下文切换到其他数据库，再使用 `DROP` 语句删除 `TestData` 数据库：  
  
  ```sql  
  USE MASTER;  
  GO  
  DROP DATABASE TestData;  
  GO   
  ```  
  
“编写 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句”教程到此结束。 请记住，本教程只是简要概述，它并未介绍所用语句的所有选项。 设计和创建有效的数据库结构以及配置对数据的安全访问，需要比本教程中显示的数据库更复杂的数据库。  

  
  
