---
description: 评估用于转换 (SybaseToSQL) 的 SAP ASE 数据库对象
title: 评估用于转换 (SybaseToSQL) 的 SAP ASE 数据库对象 |Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eb996b7c-1eef-4f73-b5e6-2fa6faf7336c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e0926c350d02bf2cdbb5ae7fe4fdefbda3ef6863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077218"
---
# <a name="assessing-sap-ase-database-objects-for-conversion-sybasetosql"></a>评估用于转换 (SybaseToSQL) 的 SAP ASE 数据库对象
在加载对象并将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 之前，你应该确定迁移的复杂性以及所需的时间。 SSMA 可以创建一个评估报表，显示将成功转换为的对象和过程的百分比 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 SSMA 还允许您查看可能导致转换失败的特定问题。  
  
## <a name="create-assessment-reports"></a>创建评估报表  
创建此评估报告时，SSMA 会将所选 SAP 自适应服务器企业 (ASE) 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 语法，然后显示结果。  
  
**创建评估报表**  
  
1.  在 "Sybase 元数据资源管理器" 中，选择要评估的数据库。  
  
2.  若要省略单个对象，请清除不想评估的对象旁边的复选框。  
  
3.  右键单击 " **数据库**"，然后选择 " **创建报表**"。  
  
    您还可以通过右键单击对象，然后选择 " **创建报表**" 来分析各个对象。  
  
    SSMA 在窗口底部的状态栏中显示进度。 如果 "输出" 窗格可见，则还会看到任何相关的消息。  
  
    评估完成后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将显示 "Sybase：评估报表" 窗口的迁移助手。  
  
## <a name="use-assessment-reports"></a>使用评估报告  
"评估报表" 窗口包含三个窗格：  
  
-   左窗格包含评估报表中包含的对象的层次结构。 您可以浏览层次结构并选择对象和对象类别，以查看转换统计信息和代码。  
  
-   右窗格的内容因在左窗格中选择的项而异。  
  
    如果选择了一组对象 (如架构) 或表，则右窗格将显示两个窗格。 " **转换统计信息** " 窗格显示所选对象的转换统计信息。 " **按类别** 列出的对象" 窗格显示对象或对象类别的转换统计信息。  
  
    如果选择了 "存储过程"、"视图" 或 "触发器"，则右窗格包含统计信息、源代码和目标代码。  
  
    -   顶部区域显示对象的总体统计信息。 若要查看此信息，你可能必须展开 " **统计** 信息"。 
    -   源区域显示在左窗格中选择的对象的源代码。 突出显示的区域显示有问题的源代码。  
    -   目标区域显示转换后的代码。 红色文本显示有问题的代码和错误消息。  
  
-   底部窗格显示转换消息，按消息编号分组。 选择 " **错误**"、" **警告**" 或 " **信息** " 以查看消息类别，然后展开一组消息。 单击单个消息，在左窗格中选择该对象，然后在右窗格中显示详细信息。  
  
## <a name="analyze-conversion-problems-by-using-the-assessment-report"></a>使用评估报表分析转换问题  
" **转换统计信息" 窗格** 显示转换统计信息。 如果任何类别的百分比低于100%，则应该确定转换不成功的原因。  
  
**查看转换问题**  
  
1.  使用前一过程中的说明创建评估报告。  
  
2.  在左侧窗格中，展开具有红色错误图标的架构或文件夹。 继续展开项，直到选择了失败转换的单个项。  
  
3.  在 "源" 窗格的顶部，选择 " **下一个问题**"。  
    突出显示有问题的代码，就像 **目标导航** 窗格中的相关代码一样。  
  
4.  查看任何错误消息，然后确定要如何处理导致转换问题的对象：  
  
    -   更新 SSMA 中的 ASE 语法。 只能为存储过程和触发器更新语法。 若要更新语法，请在 "Sybase 元数据资源管理器" 窗格中选择对象，单击 " **sql** " 选项卡，然后编辑 sql 代码。 当您离开该项时，系统将提示您保存已更新的语法。 在 " **报表** " 选项卡上查看对象的报告错误。  
  
    -   在 ASE 中，可以更改 ASE 对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 SYBASE ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)。  
  
    -   可以从迁移中排除对象。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 元数据资源管理器和 "Sybase 元数据资源管理器" 中，先清除项旁边的复选框，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 中，并从 ASE 迁移数据。
  
## <a name="next-steps"></a>后续步骤  
[将 SAP ASE 数据库对象转换 &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 SAP ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
