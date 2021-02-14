---
title: 创建项目的快照
description: 熟悉数据层应用程序文件或快照，并了解如何使用它们。 了解如何创建或导入快照，以及如何对其进行比较。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlProjectImportSnapshotSummaryDialog.dialog
- sql.data.tools.SqlProjectImportSnapshotDialog.dialog
ms.assetid: bed670a3-13bd-4d88-91a1-58d5b9524a97
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0a3c615e98f16c5894924817fc2d4d5acf37b962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018338"
---
# <a name="how-to-create-a-snapshot-of-a-project"></a>如何：创建项目的快照

数据层应用程序文件为你在创建数据库架构时提供数据库架构的只读表示形式。 该文件实质上被视为可以将其中的架构对象导入回某一项目的一种数据库架构。 您还可以将其与某一数据库或项目的架构进行比较，并且更新该数据库或项目以便反映在该快照中定义的架构。  
  
如果某一源数据库项目出现用户错误，还可将该源项目恢复到创建快照时的状态。 您也可以在不同的开发阶段中出于基准目的建立快照。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
### <a name="to-create-a-snapshot"></a>要创建快照，请执行以下操作  
  
1.  在“解决方案资源管理器”中，右键单击“TradeDev”项目，然后选择“数据层应用程序(\*.dacpac)…”  。  
  
2.  SSDT 将首先尝试生成该项目。 如果没有生成错误，则在“解决方案资源管理器”中创建一个“快照”文件夹。 在此文件夹中，SSDT 将使用“<Project Name>_YYYYMMDD_HH-MM-SS.dacpac”的名称格式创建一个 .dacpac 文件。  
  
3.  右键单击 .dacpac 文件，然后选择“重命名”。 将该默认文件名更改为“TradeDev1.dacpac”。  
  
4.  在“解决方案资源管理器”中右键单击 GetProductsBySupplier 函数，然后选择“删除”以便从项目中删除它。  
  
5.  按照前面的步骤，创建名为 TradeDev2.dacpac 的一个新快照。  
  
### <a name="to-import-a-snapshot"></a>导入快照  
  
1.  在“解决方案资源管理器”中，右键单击“TradeDev”项目，选择“导入”，然后从上下文菜单中选择“数据层应用程序(\*.dacpac)…”   。  
  
2.  在“导入数据层应用程序”对话框中，单击“浏览”以便选择要用作导入的源的 TradeDev1.dacpac。  
  
    请注意，“目标项目”部分已被禁用，因为当前项目是默认目标。 单击“启动”以开始导入。  
  
3.  在“摘要”页中单击“完成”。 在“解决方案资源管理器”中，请注意删除的表已还原到该项目。  
  
    > [!WARNING]  
    > 导入快照将快照架构中的所有数据库实体都导入到项目中。 因此这可以创建重复的实体。 例如，每个表和视图现在都包含一个名为 <ObjectName_1> 的自身的另外一个副本。 在“解决方案资源管理器”中右键单击上述各重复对象，然后选择“删除”以便从项目中删除它。  
  
### <a name="to-compare-snapshots"></a>比较快照  
  
1.  在“解决方案资源管理器”中右键单击 TradeDev1.dacpac，然后选择“架构比较”。 将打开“架构比较”窗口。  
  
2.  使用“数据层应用程序文件”选项来设置源和目标架构。 请确保“源架构”在“数据层应用程序文件”中设置为 TradeDev1.dacpac，“目标架构”设置为 TradeDev2.dacpac。  
  
3.  单击“确定”以开始比较。 请注意，删除的函数将作为旧快照和新快照之间的差异而被突出显示。  
  
    通过使用架构比较，您可以轻松找到不同快照之间的差异。 在本例中，您可以了解在开发过程中您的项目的发展情况。  
  
## <a name="see-also"></a>另请参阅  
[如何：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
