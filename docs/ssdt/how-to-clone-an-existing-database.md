---
title: 克隆现有数据库
description: 了解如何克隆数据库。 查看创建新数据库、复制数据库架构及其数据所涉及的步骤。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: aad3594a-11cf-4e68-a622-071a93d43875
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9dd341612f075c3a92cd01b5cfca1133910f2c57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018447"
---
# <a name="how-to-clone-an-existing-database"></a>如何：克隆现有数据库

此任务将使用您在以前的过程中学习的某些步骤来创建新数据库并将现有数据移植到其中。 此外，它还使用[如何：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)中讨论的步骤以同步源和项目数据库的架构。  
  
通过使用这些步骤，您可以轻松地从具有相同架构和数据的生产数据库创建开发或测试数据库。 然后，您可以继续在连接模式下开发测试数据库，或者为脱机开发和测试创建数据库项目，所有这些工作都无需中断生产数据库的操作。  
  
> [!WARNING]  
> 下面的过程使用了前面[连接的数据库开发](../ssdt/connected-database-development.md)一节介绍的过程中创建的实体。  
  
### <a name="to-create-a-development-database"></a>创建开发数据库  
  
1.  在 SQL Server 对象资源管理器中，在“SQL Server”节点下，展开你的连接的服务器实例。  
  
2.  右键单击“数据库”节点，然后选择“添加新数据库”。  
  
3.  将这个新数据库重命名为 TradeDev。  
  
4.  右键单击“SQL Server 对象资源管理器”中的“Trade”数据库，然后选择“架构比较”。 按照[如何：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)主题中的步骤，选择原始的 Trade 数据库作为源，选择新的 TradeDev 数据库作为目标。 这将使用来自 Trade 的架构更新 TradeDev。  
  
### <a name="to-replicate-data"></a>复制数据  
  
1.  上一步仅将生产数据库的架构复制到开发数据库。 在本过程中，您将生产数据复制到开发数据库。  
  
    在 Trade 数据库中右键单击 Suppliers 表，然后选择“查看数据”。 数据编辑器随即打开。  
  
2.  在工具栏中单击“最大行”旁边的“脚本”按钮。  
  
3.  在脚本窗口打开后，请确保 Connected 显示在 Transact\-SQL 脚本窗格之下的状态栏中。 如果显示 Disconnected，则单击“连接”按钮（工具栏中最左侧的按钮）并且输入你的服务器信息和凭据。  
  
4.  在“连接”/“断开连接”按钮旁的“数据库”下拉菜单中，选择 TradeDev。 这类似于 Transact\-SQL`USE` 语句，并且将确保代码编辑器中的脚本将对 TradeDev 数据库执行。  
  
5.  单击“执行查询”按钮以执行 `INSERT` 语句。 这会将来自 `Suppliers` 数据库的 `Trade` 表的所有行插入到 `Suppliers` 数据库的 `TradeDev` 表中。  
  
6.  为 `Trade` 数据库中的所有表重复上述步骤，以便它们复制到 `TradeDev` 数据库中。  
  
7.  使用数据编辑器确认新的 `TradeDev` 数据库中的所有表都已填充。  
  
## <a name="see-also"></a>另请参阅  
[如何：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
