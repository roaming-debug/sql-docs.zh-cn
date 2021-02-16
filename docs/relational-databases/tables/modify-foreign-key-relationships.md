---
description: 修改外键关系
title: 修改外键关系 | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65538
- vdt.ppg.relationships
helpviewer_keywords:
- foreign keys [SQL Server], modifying
- modifying foreign keys
ms.assetid: 0c9ca80d-d79b-44c4-a21e-0fce39c398ec
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 457f4db2126b314237ccc09678718e14ffc25069
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354127"
---
# <a name="modify-foreign-key-relationships"></a>修改外键关系

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  您可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改关系的外键端。 修改表的外键会更改与主键表中的列相关的列。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **修改外键，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 新的外键列必须与和其相关的主键列的数据类型和大小相匹配，但以下情况例外：  
  
-   **char** 列或 **sysname** 列可以与 **varchar** 列相关。  
  
-   **binary** 列可以与 **varbinary** 列相关。  
  
-   别名数据类型可以与其基类型相关。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-a-foreign-key"></a>修改外键  
  
1.  在 **“对象资源管理器”** 中，展开具有外键的表，再展开 **“键”**。  
  
2.  右键单击要修改的外键，然后选择“修改”。  
  
3.  在 **“外键关系”** 对话框中，可以进行以下修改。  
  
     **选定的关系**  
     列出现有的关系。 选择一个关系将在右侧的网格中显示其属性。 如果该列表为空，则表示尚未为该表定义关系。  
  
     **添加**  
     创建新关系。 必须先设置 **“表和列规范”** ，之后该关系才会生效。  
  
     **删除**  
     在“选定的关系”列表中删除所选的关系。 若要取消添加关系，请使用此按钮来移除关系。  
  
     **常规类别**  
     展开此项可显示“在创建或重新启用时检查现有数据”以及“表和列规范”。  
  
     **在创建或重新启用时检查现有数据**  
     根据约束对创建或重新启用约束前表中的全部已有数据进行验证。  
  
     **表和列规范类别**  
     展开此项可显示哪些表中的哪些列用作关系中的外键和主键（或唯一键）。 若要编辑或定义这些值，请单击属性字段右侧的省略号按钮 (…)。  
  
     **外键基表**  
     显示哪个表包含用作所选关系中外键的列。  
  
     **外键列**  
     显示哪个列用作所选关系的外键。  
  
     **主/唯一键基表**  
     显示哪个表包含用作所选关系中主键（或唯一键）的列。  
  
     **主/唯一键列**  
     显示哪个列用作所选关系的主键（或唯一键）。  
  
     **标识类别**  
     展开此项可显示“名称”和“说明”的属性字段。  
  
     **名称**  
     显示关系的名称。 在创建新关系时，将基于 **表设计器** 的活动窗口中的表为其指定默认名称。 可随时更改名称。  
  
     **说明**  
     描述该关系。 若要编写更详细的说明，请单击“说明”，再单击属性字段右侧显示的省略号 **(...)**。 这可以提供一个更大的文本编写区域。  
  
     **表设计器类别**  
     展开此项可显示“在创建或重新启用时检查现有数据”和“强制用于复制”的信息。  
  
     **强制用于复制**  
     指示当复制代理对此表执行插入、更新或删除操作时是否强制约束。  
  
     **强制外键约束**  
     指示如果对关系中列数据的更改会使外键关系的完整性失效，是否允许进行这样的更改。 如果不允许这样的更改，则选择 **“是”** ；如果允许这样的更改，则选择 **“否”** 。  
  
     **INSERT 和 UPDATE 规范类别**  
     展开此项可显示关系的“删除规则”和“更新规则”的信息。  
  
     **删除规则**  
     指定当用户尝试删除某一行而该行包含外键关系涉及的数据时将发生的情况。  
  
    -   **不执行任何操作** 错误消息告知用户不允许进行删除并将回滚 DELETE。  
  
    -   **级联** 删除所有包含外键关系所涉及数据的行。 如果该表将包含在使用逻辑记录的合并发布中，则不要指定 CASCADE。  
  
    -   **设置 Null** 如果表的所有外键列都可接受 Null 值，则将该值设置为 Null。  
  
    -   **设置默认值** 如果表的所有外键列均已定义默认值，则将该值设置成为该列定义的默认值。  
  
     **更新规则**  
     指定当用户尝试更新某一行而该行包含外键关系涉及的数据时将发生的情况。  
  
    -   **不执行任何操作** 错误消息告知用户不允许进行更新并将回滚 UPDATE。  
  
    -   **级联** 更新所有包含外键关系所涉及数据的行。 如果该表将包含在使用逻辑记录的合并发布中，则不要指定 CASCADE。  
  
    -   **设置 Null** 如果表的所有外键列都可接受 Null 值，则将该值设置为 Null。  
  
    -   **设置默认值** 如果表的所有外键列均已定义默认值，则将值设置成为该列定义的默认值。  
  
4.  在“文件”菜单上，单击“保存表名称”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **修改外键**  
  
 若要使用 Transact-SQL 修改 FOREIGN KEY 约束，必须先删除现有的 FOREIGN KEY 约束，然后再用新定义重新创建该约束。 有关详细信息，请参阅 [Delete Foreign Key Relationships](../../relational-databases/tables/delete-foreign-key-relationships.md) 和 [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md)。  
  
###  <a name="TsqlExample"></a>  
