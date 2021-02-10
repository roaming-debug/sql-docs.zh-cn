---
description: 评估 Oracle 架构以进行转换 (OracleToSQL)
title: 评估用于转换 (OracleToSQL) 的 Oracle 架构 |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Analyzing Conversion Problems
ms.assetid: 4de9bcf6-1346-4740-87f9-7f24a8226357
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 1ac1ea4b1193fbf2a65d948983c071aad817ffbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058942"
---
# <a name="assessing-oracle-schemas-for-conversion-oracletosql"></a>评估 Oracle 架构以进行转换 (OracleToSQL)
在加载对象并将数据迁移到之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，您应该确定迁移的复杂程度以及迁移所需的时间。 SSMA 可以创建一个评估报表，显示将成功转换的对象的百分比。 SSMA 还使你能够查看导致转换失败的特定问题。  
  
## <a name="creating-assessment-reports"></a>创建评估报表  
当它创建此评估报表时，SSMA 会将选定的 Oracle 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法，然后显示结果。  
  
**创建评估报表**  
  
1.  在 Oracle 元数据资源管理器中，选择要评估的架构。  
  
2.  若要省略单个对象，请清除这些对象旁边的复选框。  
  
3.  右键单击 " **架构**"，然后选择 " **创建报表**"。  
  
    您还可以通过右键单击对象，然后选择 " **创建报表**" 来分析各个对象。  
  
    SSMA 会在窗口底部的状态栏中显示进度。 如果 "输出" 窗格可见，还会在 "输出" 窗格中看到消息。  
  
    评估完成后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将显示 "Oracle：评估报表的迁移助手" 窗口。  
  
## <a name="using-assessment-reports"></a>使用评估报表  
"评估报表" 窗口包含三个窗格：  
  
-   左窗格包含评估报表中包含的对象的层次结构。 您可以浏览层次结构，并选择对象和对象类别来查看转换统计信息和代码。  
  
-   右窗格的内容取决于在左窗格中选择的项目。  
  
    如果选择了一组对象（如架构），或者如果选择了表，则右窗格将包含 "转换统计信息" 窗格和 "按类别分类的对象" 窗格。 "转换统计信息" 窗格显示所选对象的转换统计信息。 "按类别列出的对象" 窗格显示对象或对象类别的转换统计信息。  
  
    如果选择了函数、包、过程、序列或视图，则右窗格包含统计信息、源代码和目标代码。  
  
    -   顶部区域显示对象的总体统计信息。 若要查看此信息，你可能必须展开 " **统计** 信息"。  
  
    -   源区域显示在左窗格中选择的对象的源代码。 突出显示的区域显示有问题的源代码。  
  
    -   目标区域显示转换后的代码。 红色文本显示有问题的代码和错误消息。  
  
-   底部窗格显示转换消息，按消息编号分组。 您可以单击 " **错误**"、" **警告**" 或 " **信息** " 以查看消息类别，然后展开一组消息。 单击单个消息可在左窗格中选择该对象，并在右窗格中显示详细信息。  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>使用评估报表分析转换问题  
"转换统计信息" 窗格显示转换统计信息。 如果任何类别的百分比低于100%，则应该确定转换不成功的原因。  
  
**查看转换问题**  
  
1.  使用前一过程中的说明创建评估报告。  
  
2.  在左侧窗格中，展开具有红色错误图标的架构或文件夹。 继续展开项，直到选择了失败转换的单个项。  
  
3.  在源窗格顶部，单击 " **下一问题**"。  
  
    突出显示有问题的代码，就像目标导航窗格中的相关代码一样。  
  
4.  查看任何错误消息，然后确定要如何处理导致转换问题的对象：  
  
    -   更新 SSMA 中的 Oracle 语法。 您可以更新过程、函数、触发器、打包函数和打包过程的语法。 若要更新语法，请在 "Oracle 元数据资源管理器" 窗格中选择对象，单击 " **sql** " 选项卡，然后修改 sql 代码。 当您离开该项时，系统将提示您保存已更新的语法。 您可以在 " **报表** " 选项卡上查看对象的报告错误。  
  
    -   在 Oracle 中，您可以修改 Oracle 对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md)。  
  
    -   可以从迁移中排除对象。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 和 "Oracle 元数据资源管理器" 中，清除项旁的复选框，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle 并从 Oracle 迁移数据。  
  
## <a name="next-step"></a>下一步  
[将 Oracle 架构转换 &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Oracle 数据库迁移到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
