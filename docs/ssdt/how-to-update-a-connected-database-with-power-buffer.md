---
title: 使用 Power Buffer 更新连接的数据库
description: 了解如何使用 Power Buffer 更新数据库。 请参阅如何在应用更改之前验证更改，以及如何将更改保存在脚本中以供以后部署。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.commitpreview.dialog
ms.assetid: 4048b7f8-71a9-47ad-b812-3fc1e8066240
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2ecad870723402e33026668055368dac5dfd02c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018087"
---
# <a name="how-to-update-a-connected-database-with-power-buffer"></a>如何：使用 Power Buffer 更新连接的数据库

SQL Server Data Tools Power Buffer 技术可通过存储你在当前会话中所做的所有编辑工作，方便你将更改应用于连接的数据库。 由于在 Power Buffer 窗口（在 Transact\-SQL 编辑器或表设计器中）中进行编辑所导致的任何错误都将立即显示在“错误列表”窗格中，这使你可以按照这些标识的错误来进一步进行故障排除。 您可以验证挂起的更改，直到可供将其应用于您的数据库。 在更新过程中，SSDT 会基于您的编辑自动创建 ALTER 脚本，并提醒您任何潜在的问题。 然后，您可以将在所有打开的 Power Buffer 窗口上累积的所有更改应用于相同的数据库，或者保存该 ALTER 脚本以便在以后部署。  
  
SSDT 还知道您在 Visual Studio 外对您的数据库架构所做的任何更改。 例如，如果你将一个新表添加到 SQL Server Management Studio 中的现有数据库，则此类更改会立即显示在 Visual Studio 的“SQL Server 对象资源 管理器”中，而无需手动刷新它。 偏差检测功能可确保你始终在“SQL Server 对象资源管理器”中查看数据库的最新架构定义。 请注意，在表设计器或 Transact\-SQL 编辑器中打开的供编辑的任何数据库对象都将不会刷新以显示 Visual Studio 外的更改。  
  
下面的过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)一节的前面的过程中创建的实体。  
  
### <a name="to-apply-the-changes-made-in-the-previous-procedures"></a>应用在之前的过程中进行的更改  
  
1.  单击工具栏上的绿色“更新”按钮（如果将鼠标指针悬停在该按钮上，则会显示“更新数据库”工具提示）。 工具栏位于表设计器的列网格上方。  
  
2.  此时将出现“预览数据库更新”对话框。 基于您的更改的部署脚本将在后台生成。 然后，该对话框将显示 SSDT 将要执行的操作的摘要（例如，创建或删除数据库实体），同时还将显示已发现的潜在问题（这不适用于我们的过程，但在您的数据库定义包含在解决前阻止更新操作运行的错误时会给您带来便利）。  
  
3.  如果此时不想更新数据库，则单击“取消”按钮退出“预览数据库更新”对话框。  
  
4.  如果对截止到目前所做的更改满意，则在“预览数据库更新”对话框中单击“更新数据库”按钮。 该部署脚本将代表您来执行，并且您的累积更改现在将应用于数据库。  
  
5.  如果要查看部署脚本以便进行验证，或者要在更新前进行某些更改，则在“预览数据库更新”对话框中单击“生成脚本”按钮。 生成的脚本将会在一个新的 Transact\-SQL 编辑器窗口中打开。 可以按 Transact\-SQL 编辑器工具栏中的“执行查询”按钮以便运行此查询。 这类似于在步骤 4 中按下“更新数据库”按钮所完成的工作。  
  
    > [!WARNING]  
    > 如果您对部署脚本进行任何更改并且执行该脚本，则此类更改将不会显示在任何打开的数据库实体中。 例如，如果在部署脚本中重命名 `Customers` 表的某一列并执行该脚本以便更新数据库，并且如果 `Customers` 表在表设计器中打开，则在单击“更新数据库”按钮时，该列名仍将是旧名称。 您必须手动关闭表设计器并且不在本地将其保存为脚本。 在从“SQL Server 对象资源管理器”中重新打开表时，将注意到该数据库实际上已用你在部署脚本中进行的更改更新。  
  
6.  在 Transact\-SQL 编辑器的“输出”窗格（如果正在自己执行部署脚本，也可以是“消息”窗格）中，请注意指示更新成功的以下内容。  
  
**正在创建 [dbo].[Customers]...正在创建 [dbo].[Products]...正在创建 [dbo].[Suppliers]...正在创建 FK_Products_SupplierId...正在创建 FK_Products_CustomerId...正在创建 CK_Products_ShelfLife 数据库更新的事务部分成功。根据新创建的 constraintsUpdate 检查现有数据完成。**  
  
7.  在“SQL Server 对象资源管理器”中，注意新表已显示在“Trade”数据库的“表”节点下。  
  
### <a name="to-view-changes-made-to-a-database-outside-visual-studio"></a>查看对 Visual Studio 外的数据库进行的更改  
  
1.  打开 SQL Server Management Studio。 在“连接到服务器”对话框中，输入你已连接到 Visual Studio 中的相同数据库服务器，然后单击“连接”。  
  
2.  在“SQL Server 对象资源管理器”中，展开“数据库”，然后导航到“Trade”数据库。  
  
3.  右键单击“Trade”下的“表”，然后选择“新建表”。 在表设计器中，输入 id 作为列名称，输入 int 作为数据类型。  
  
4.  在工具栏中单击“保存”图标以便保存该表。 接受默认名称，然后单击“确定”。  
  
    返回 Visual Studio。 在“SQL Server 对象资源管理器”中查看“Trade”数据库之下的“表”节点。 请注意新创建的“Table_1”表的外观。  
  
5.  右键单击“Table_1”并选择“删除”。 在“预览数据库更新”对话框中，单击“更新数据库”。  
  
## <a name="see-also"></a>另请参阅  
[如何：修复错误](../ssdt/how-to-fix-errors.md)  
  
