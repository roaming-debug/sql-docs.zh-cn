---
description: 教程：编写 Transact-SQL 语句
title: 教程：编写 Transact-SQL 语句 | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: quickstart
helpviewer_keywords:
- Transact-SQL statements, tutorials
- Transact-SQL tutorials
- tutorials [Transact-SQL]
ms.assetid: 2addc9be-67d0-423d-a457-192fe9d7d058
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f2f4ddc1c090e7a7f8497f419590f906584a7f9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483819"
---
# <a name="tutorial-writing-transact-sql-statements"></a>教程：编写 Transact-SQL 语句
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
欢迎学习“编写 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句”教程。 本教程适用于对编写 SQL 语句不熟悉的用户。 本教程通过回顾一些用于创建表和插入数据的基本语句，帮助新用户入门。 本教程使用 [!INCLUDE[tsql](../includes/tsql-md.md)]，后者是 SQL 标准的 [!INCLUDE[msCoName](../includes/msconame-md.md)] 实现。 本教程旨在简介 [!INCLUDE[tsql](../includes/tsql-md.md)] 语言，但不是要取代 [!INCLUDE[tsql](../includes/tsql-md.md)] 课程。 本教程特意选用了简单的语句，因此它们不能代表标准生产数据库中存在的语句的复杂程度。  
  
>**注意：** 如果你是初学者，可能会发现使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 比编写 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句更容易。  
  
## <a name="finding-more-information"></a>查找详细信息  
若要查找有关任何特定语句的详细信息，请在 SQL Server 联机丛书中按名称搜索该语句，或使用“目录”浏览在 [Transact-SQL 引用（数据库引擎）](./language-reference.md)下按字母顺序列出的 1,800 个语言元素。 另一种查找信息的好办法是搜索与您感兴趣的主题相关的关键字。 例如，如果想要知道如何返回日期的一部分（例如月份），请在索引中搜索 **dates [SQL Server]**，然后选择 **dateparts**。 这会让你转到主题[日期部分 (Transact-SQL)](../t-sql/functions/datepart-transact-sql.md)。 作为另一个示例，若要了解如何使用字符串，请搜索 **string functions**。 这会让你转到主题[字符串函数 (Transact-SQL)](../t-sql/functions/string-functions-transact-sql.md)。  
  
## <a name="what-you-will-learn"></a>学习内容  
本教程将介绍如何创建数据库、在数据库中创建表、将数据插入到表中、更新数据、读取数据、删除数据，然后删除表。 您将创建视图和存储过程，并为数据库和数据配置用户。  
  
本教程分为三课：  
  
[第 1 课：创建数据库对象](../t-sql/lesson-1-creating-database-objects.md)  
在本课中，将介绍如何创建数据库、在数据库中创建表、将数据插入到表中、更新数据，然后读取数据。  
  
[第 2 课：配置数据库对象的权限](../t-sql/lesson-2-configuring-permissions-on-database-objects.md)  
在本课中，将介绍如何创建登录名和用户。 还将创建视图和存储过程，再将用户权限授予存储过程。  
  
[第 3 课：删除数据库对象](../t-sql/lesson-3-deleting-database-objects.md)  
在本课中，将介绍如何删除对数据的访问权限、从表中删除数据、删除表，然后删除数据库。  
  
## <a name="requirements"></a>要求  
为了完成本教程，您不必知道 SQL 语言，但是应该理解基本的数据库概念，如表。 在学习本教程的过程中，将创建一个数据库，并创建一个 Windows 用户。 这些任务需要高级别的权限；因此，您应该以管理员身份登录到计算机。  
  
您的系统必须安装以下软件：  
  
-   任何版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
-  [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)  
  

 
  
  
