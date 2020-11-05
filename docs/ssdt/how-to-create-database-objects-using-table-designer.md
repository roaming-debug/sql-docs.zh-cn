---
title: 使用表设计器创建数据库对象
description: 了解如何在 SQL Server 对象资源管理器中创建新数据库。 请参阅如何在表设计器中创建新的表、约束和外键引用。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.scriptpanel
- sql.data.tools.design.table.context.view
- Microsoft.Data.Relational.Design.PW.RelationshipsDescriptor.OnDelete
ms.assetid: 9c9479c1-9bfc-4039-837e-e53fce67723d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 410b2674f407018b895ed84781bedf5fa8766feb
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364389"
---
# <a name="how-to-create-database-objects-using-table-designer"></a>如何：使用表设计器创建数据库对象

SQL Server 对象资源管理器中的新的“SQL Server”节点不但在外观上与 SSMS 十分相似，而且可以使用在功能上与其 SSMS 对应项类似的上下文菜单创建新对象。  
  
例如，可以在“数据库”节点下创建一个新数据库。 同样，您可以选择特定的数据库，并且使用新的表设计器即时创建或编辑表定义及其相关编程对象。 从表设计器中，您可以切换到脚本窗格，从该窗格中，您可以直接编辑定义此表的脚本。  
  
### <a name="to-create-a-new-database"></a>创建新数据库  
  
1.  在 SQL Server 对象资源管理器中，在“SQL Server”节点下，展开你的连接的服务器实例。  
  
2.  右键单击“数据库”节点，然后选择“添加新数据库”。  
  
3.  将这个新数据库重命名为 Trade。  
  
### <a name="to-create-new-tables-using-the-table-designer"></a>使用表设计器创建新表  
  
1.  展开新创建的“Trade”节点。 右键单击“表”节点，选择“添加新表”。  
  
2.  表设计器随即将在一个新窗口中打开。 该设计器由列网格、脚本窗格和上下文窗格构成。 列网格将列出表中的所有列。 我们将在以后过程中重新访问该设计器的其他组件。  
  
3.  在脚本窗格中，将这个新表重命名为 `Suppliers`。 具体来说，将  
  
    ```  
    CREATE TABLE [dbo].[Table1]  
    ```  
  
    替换为  
  
    ```  
    CREATE TABLE [dbo].[Suppliers]  
    ```  
  
4.  单击列网格中的空行以便向该表添加新列。  为“名称”字段输入 CompanyName，为“数据类型”字段输入 nvarchar (128)，取消选中“允许 Null 值”字段。 在您按 Tab 键离开这些字段时，请注意脚本窗格将立即更新。  
  
5.  添加另一个新列。 为“名称”字段输入 Address，为“数据类型”字段输入 nvarchar (MAX)，取消选中“允许 Null 值”字段。  
  
    > [!WARNING]  
    > 在您正在从连接的数据库编辑对象时，不要将这些对象保存到您的本地驱动器。 若要将所做的更改正确保存到数据库中，请按照接下来的[如何：使用 Power Buffer 更新连接的数据库](../ssdt/how-to-update-a-connected-database-with-power-buffer.md)过程中的步骤进行操作。  
  
6.  重复上述步骤，创建另一个名为“Customer” 的表。 这一次，您将使用列网格向该 Customer 表添加以下列。 并且记住要更改脚本，以便该表的名称为 `[dbo].[Customer]`。  
  
    |名称|数据类型|**允许 Null 值**|  
    |--------|-------------|-------------------|  
    |ID|int|unchecked|  
    |名称|nvarchar (128)|unchecked|  
  
7.  再创建一个名为“Products”的表。 使用列网格向该 Products 表添加以下列。 并且记住要更改脚本，以便该表的名称为 `[dbo].[Products]`。  
  
    |名称|数据类型|**允许 Null 值**|  
    |--------|-------------|-------------------|  
    |ID|int|unchecked|  
    |名称|nvarchar (128)|unchecked|  
    |ShelfLife|int|已选中|  
    |SupplierId|int|已选中|  
    |CustomerId|int|已选中|  
  
### <a name="to-create-a-new-check-constraint-using-the-table-designer"></a>使用表设计器创建新的 CHECK 约束  
  
1.  表设计器的上下文窗格将为您提供表定义（键、约束、触发器等）的逻辑视图，使您可以选择一个对象以便突出显示其与单独列的关系。  
  
    对于 Products 表，在表设计器的上下文窗格中右键单击“CHECK 约束”节点，然后选择“添加新的 CHECK 约束”。  
  
2.  请注意，节点计数将自动递增 1。  
  
3.  单击脚本窗格，并且使用以下内容替换该约束的默认定义。  
  
    ```  
    CONSTRAINT [CK_Products_ShelfLife] CHECK ([ShelfLife] <5),  
    ```  
  
    该约束会将某一行的 ShelfLife 值限制为低于 5。  
  
### <a name="to-create-new-foreign-key-references-using-the-table-designer"></a>使用表设计器创建新的外键引用  
  
1.  对于 Products 表，在上下文窗格中右键单击“外键”节点，然后选择“添加新的外键”。  
  
2.  请注意，节点计数将自动递增 1。  
  
3.  单击脚本窗格，并且使用以下内容替换该外键引用的默认定义。  
  
    ```  
    CONSTRAINT [FK_Products_SupplierId] FOREIGN KEY ([SupplierId]) REFERENCES [dbo].[Suppliers] ([Id]),  
    ```  
  
4.  重复上面的步骤来添加对 Products 表的另一个外键引用。 这次，用以下内容替换默认定义。  
  
    ```  
    CONSTRAINT [FK_Products_CustomerId] FOREIGN KEY ([CustomerId]) REFERENCES [dbo].[Customer] ([Id])  
    ```  
  
## <a name="see-also"></a>另请参阅  
[管理表、关系和修复错误](../ssdt/manage-tables-relationships-and-fix-errors.md)  
  
