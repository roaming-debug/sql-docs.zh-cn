---
title: MSSQLSERVER_208 | Microsoft Docs
description: 找不到指定的对象，从而导致无效的对象名称消息。 了解错误说明和可能的解决方法。
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 208 (Database Engine error)
ms.assetid: 4b1895f5-3197-4da1-af86-954c93507956
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 55b31cdba6f7a96fb8eff7fba2974608b72675d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196407"
---
# <a name="mssqlserver_208"></a>MSSQLSERVER_208
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|208|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQ_BADOBJECT|  
|消息正文|对象名 '%.*ls' 无效。|  
  
## <a name="explanation"></a>说明  
找不到指定的对象。  
  
### <a name="possible-causes"></a>可能的原因  
此错误可能是由以下问题之一导致的：  
  
-   未正确指定对象。  
  
-   当前的数据库或指定的数据库中不存在该对象。  
  
-   该对象存在，但无法向用户公开。 例如，用户对于该对象可能不具有权限，或者该对象创建于 EXECUTE 语句内部，而用户却从 EXECUTE 语句的作用域外部访问它。  
  
## <a name="user-action"></a>用户操作  
查看以下信息并根据需要更正该语句。  
  
-   对象名的拼写是否正确。  
  
-   当前数据库的上下文是否正确。 如果没有为该对象指定数据库名称，则当前数据库中必须存在该对象。 有关设置数据库上下文的详细信息，请参阅 [USE (Transact-SQL)](~/t-sql/language-elements/use-transact-sql.md)。  
  
-   系统表中是否存在该对象。 若要验证是否存在表或其他架构范围内的对象，请查询 **sys.objects** 目录视图。 如果系统表中没有此对象，则表明此对象已删除或者用户没有查看此对象元数据的权限。 有关查看对象元数据的权限的详细信息，请参阅[元数据可见性配置](~/relational-databases/security/metadata-visibility-configuration.md)。  
  
-   该对象是否包含在用户的默认架构中。 如果不包含在默认架构中，则必须使用两部分格式 *schema_name.object_name* 来指定该对象。 请注意，必须始终使用至少由两部分组成的名称来调用标量值函数。  
  
-   数据库排序规则区分大小写。  
  
    当数据库使用区分大小写的排序规则时，对象名的大小写必须与数据库中对象的大小写一致。 例如，如果使用区分大小写的排序规则在数据库中将某个对象指定为 **MyTable**，那么，以 **mytable** 或 **Mytable** 形式引用该对象的查询将因对象名不匹配而导致返回错误 208。  
  
    您可以通过运行下面的语句来检验数据库排序规则。  
  
    ```  
    SELECT collation_name FROM sys.databases WHERE name = 'database_name';  
    ```  
  
    排序规则名称中的缩写 CS 指示排序规则区分大小写。 例如，Latin1_General_CS_AS 是一个区分大小写和重音符号的排序规则。 CI 指示排序规则不区分大小写。  
  
-   用户是否有权访问该对象。 若要验证用户是否有权访问该对象，请使用 **Has_Perms_By_Name** 系统函数。  
  
## <a name="see-also"></a>另请参阅  
[USE (Transact-SQL)](~/t-sql/language-elements/use-transact-sql.md)  
[元数据可见性配置](~/relational-databases/security/metadata-visibility-configuration.md)  
[HAS_PERMS_BY_NAME (Transact-SQL)](~/t-sql/functions/has-perms-by-name-transact-sql.md)  
  
