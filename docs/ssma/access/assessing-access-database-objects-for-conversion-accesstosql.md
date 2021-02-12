---
description: '评估 Access 数据库对象的转换 (AccessToSQL) '
title: 评估 Access 数据库对象的 AccessToSQL) 的转换 (|Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- assessing SQL
- assessing syntax
- assessment reports
- creating assessment reports
- estimating migration effort
- reports
- SQL, assessing
- syntax, assessing
ms.assetid: 8b9e23d6-da62-437a-8c05-8ad2628b9441
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07adac4e991afd70b70a5f087a881f1933baaae1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076418"
---
# <a name="assessing-access-database-objects-for-conversion-accesstosql"></a>评估 Access 数据库对象的转换 (AccessToSQL) 
在加载对象并将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之前，你应该确定要成功完成多少迁移，以及转换可能需要多长时间。 SSMA 可以创建一个评估报告，该报告显示已成功转换为的对象的百分比， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 语法和时间估计来执行迁移。 SSMA 还使你能够查看导致转换失败的特定问题。  
  
## <a name="creating-assessment-reports"></a>创建评估报表  
创建评估报表时，SSMA 会将选定的 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 语法，然后显示结果。  
  
**创建评估报表**  
  
1.  在 "Access 元数据资源管理器" 中，选择要评估的一个或哪些数据库。  
  
2.  若要省略单个对象，请清除不想评估的对象旁边的复选框。  
  
3.  右键单击 " **数据库**"，然后选择 " **创建报表**"。  
  
    您还可以通过右键单击对象，然后选择 " **创建报表**" 来分析各个对象。  
  
    SSMA 在窗口底部的状态栏中显示进度。 如果 "输出" 窗格可见，还会在 "输出" 窗格中看到消息。  
  
评估完成后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将显示 "访问迁移助手：评估报告" 窗口。  
  
## <a name="using-assessment-reports"></a>使用评估报表  
"评估报表" 窗口包含三个窗格： "资源管理器"、"详细信息" 窗格和 "消息" 窗格。  
  
-   "资源管理器" 窗格允许您浏览已评估的对象。 您可以单击此窗格中的项来深化到各个表、索引和键。  
  
-   "详细信息" 窗格显示所选对象的转换统计信息。  
  
-   "消息" 窗格显示转换的错误、警告和信息性消息，以及执行迁移和各个错误更正步骤的时间估算。  
  
您应更正错误，然后再次运行评估报告或转换架构。 若要查找错误，请单击 "消息" 窗格中的 " **错误** " 按钮，然后展开每个错误以查看出现错误的对象的列表。 如果单击 "消息" 窗格中的对象，则该对象的所有错误和警告将显示在 "详细信息" 窗格中。  
  
## <a name="next-step"></a>下一步  
[转换 Access 数据库对象](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
