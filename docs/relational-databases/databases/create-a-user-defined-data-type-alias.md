---
title: 创建用户定义的数据类型别名 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 2019 中创建用户定义的数据类型别名。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.userdefineddatatype.general.f1
- sql13.swb.new.datatype.properties.general.f1
helpviewer_keywords:
- alias data types [SQL Server], creating
ms.assetid: b1dd8413-0cd0-411b-a79b-1bb043ccc62d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85c93be3f041e470a7c4d1838467a997ff7dd56f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476498"
---
# <a name="create-a-user-defined-data-type-alias"></a>创建用户定义的数据类型别名
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建新的用户定义数据类型别名。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建用户定义的数据类型别名，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   用户定义的数据类型别名必须符合标识符的规则。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求在当前数据库中具有 CREATE TYPE 权限，以及具有对 *schema_name* 的 ALTER 权限。 如果未指定 *schema_name* ，则将应用用于确定当前用户的架构的默认名称解析规则。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-user-defined-data-type"></a>创建用户定义数据类型  
  
1.  在“对象资源管理器”中，依次展开“数据库”、一个数据库、“可编程性”和“类型”，右键单击“用户定义数据类型”，然后单击“新建用户定义数据类型”    。  
  
     **允许 Null**  
     指定用户定义数据类型是否可以接受 Null 值。 不能编辑现有的用户定义数据类型的为 NULL 性。  
  
     **Data type**  
     从列表框中选择基本数据类型。 该列表框显示除 **geography**、 **geometry**、 **hierarchyid**、 **sysname**、 **timestamp** 和 **xml** 数据类型之外的所有数据类型。 不能编辑现有的用户定义数据类型的数据类型。  
  
     **Default**  
     （可选）选择要绑定到用户定义数据类型别名的默认值。  
  
     **长度/精度**  
     相应地显示数据类型的长度或精度。 **长度** 适用于基于字符的用户定义数据类型； **精度** 仅适用于基于数字的用户定义数据类型。 该标签会根据先前所选的数据类型而相应地改变。 如果所选数据类型的长度或精度是固定的，则不能编辑此框。  
  
     不会显示 **nvarchar(max)** 、 **varchar(max)** 或 **varbinary(max)** 数据类型的长度。  
  
     **名称**  
     如果创建新的用户定义数据类型别名，请键入用于在整个数据库中表示用户定义数据类型的唯一名称。 最大字符数必须符合系统的 **sysname** 数据类型的要求。 不能编辑现有的用户定义数据类型别名的名称。  
  
     **规则**  
     （可选）选择要绑定到用户定义数据类型别名的规则。  
  
     **缩放**  
     指定可以在小数点右存储的十进制数字的最大位数。  
  
     **架构**  
     从包含当前用户的所有可用架构的列表中选择架构。 默认选择是当前用户的默认架构。  
  
     **存储**  
     显示用户定义数据类型别名的最大存储大小。 最大存储大小会根据精度的不同而变化。  
  
    |Precision|最大存储大小|  
    |-|-|  
    |1 - 9|5|  
    |10 - 19|9|  
    |20 - 28|13|  
    |29 - 38|17|  
  
     对于 **nchar** 和 **nvarchar** 数据类型，存储值始终是 **“长度”** 中值的两倍。  
  
     不会显示 **nvarchar(max)** 、 **varchar(max)** 或 **varbinary(max)** 数据类型的存储。  
  
2.  在“新建用户定义数据类型”对话框的“架构”框中，键入此数据类型别名所属的架构，或使用浏览按钮选择架构 。  
  
3.  在 **“名称”** 框中，键入新数据类型别名的名称。  
  
4.  在 **“数据类型”** 框中，选择新数据类型别名所基于的数据类型。  
  
5.  如果该数据类型适用，请填写 **“长度”** 框、 **“精度”** 框和 **“小数位数”** 框。  
  
6.  如果新数据类型别名允许 Null 值，请选中 **“允许 NULL”** 。  
  
7.  如果希望为新数据类型别名绑定默认值或规则，请在 **“绑定”** 区域中，填写 **“默认值”** 框或 **“规则”** 框。 默认值和规则不能在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建。 改用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模板资源管理器中提供了创建默认值和规则的示例代码。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-user-defined-data-type-alias"></a>创建用户定义的数据类型别名  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例基于系统提供的 `varchar` 数据类型创建一个数据类型别名。 `ssn` 数据类型别名用于那些保存 11 位数字的社会保障号 (999-99-9999) 的列。 该列不能为 NULL。  
  
```sql  
CREATE TYPE ssn  
FROM varchar(11) NOT NULL ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库标识符](../../relational-databases/databases/database-identifiers.md)   
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)  
  
  
