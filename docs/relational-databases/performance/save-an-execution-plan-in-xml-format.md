---
title: 以 XML 格式保存执行计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 以 XML 格式保存执行计划并将其打开以供查看。 必须具有合适的权限。
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- XML query plans [SQL Server]
- opening execution plans
- XML Showplans [SQL Server]
- execution plans [SQL Server], saving
- saving execution plans
ms.assetid: c439e53b-56f3-4442-97c6-dabd48a203d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3c9c95103e574f51ef530f17432125fe99a591e6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469168"
---
# <a name="save-an-execution-plan-in-xml-format"></a>以 XML 格式保存执行计划
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 可以将执行计划保存为 XML 文件，也可以打开这些执行计划进行查看。  
  
 若要使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中的执行计划功能或使用 XML 显示计划的 SET 选项，用户必须具有执行（要为其生成执行计划的） [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询的相应权限，还必须获得对该查询所引用的所有数据库的 SHOWPLAN 权限。  
  
### <a name="to-save-a-query-plan-by-using-the-xml-showplan-set-options"></a>使用 XML 显示计划的 SET 选项保存查询计划  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，打开查询编辑器并连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  使用以下语句打开 [SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)：  
  
    ```sql  
    SET SHOWPLAN_XML ON;  
    GO  
    ```  
  
    若要打开 [STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)，请使用以下语句：  
  
    ```sql  
    SET STATISTICS XML ON;  
    GO  
    ```  
  
     > [!NOTE] 
     > SHOWPLAN_XML 将会为查询生成编译时查询执行计划信息，但是不会执行查询。 这也称为估计的执行计划。 STATISTICS XML 将会为查询生成运行时查询执行计划信息，而且会执行查询。 这也称为实际的执行计划。  
  
3.  执行查询。 示例：  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    SET SHOWPLAN_XML ON;  
    GO  
    -- Execute a query.  
    SELECT BusinessEntityID   
    FROM HumanResources.Employee  
    WHERE NationalIDNumber = '509647174';  
    GO  
    SET SHOWPLAN_XML OFF;  
    ```  
  
4.  在“结果”窗格中，右键单击包含查询计划的“Microsoft SQL Server XML 显示计划”，然后单击“将结果另存为”。  
  
5.  在“保存 \<Grid or Text> 结果”对话框中的“保存类型”框中，单击“所有文件(\*.\*)”   。  
  
6.  在“文件名”框中，提供“\<name>.sqlplan”格式的名称，然后单击“保存”  。  

### <a name="to-save-an-execution-plan-by-using-sql-server-management-studio-options"></a>使用 SQL Server Management Studio 选项保存执行计划  
  
1.  使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]生成估计的执行计划或实际的执行计划。 有关详细信息，请参阅[显示估计的执行计划](../../relational-databases/performance/display-the-estimated-execution-plan.md)和[显示实际的执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)。  
  
2.  在“结果”窗格的“执行计划”选项卡上，右键单击图形执行计划，然后选择“将执行计划另存为”。  
  
     此外，还可以在 **“文件”** 菜单上选择 **“将执行计划另存为”** 。  
  
3.  在“另存为”对话框中，确保将“保存类型”设置为“执行计划文件(\*.sqlplan)”。  
  
4.  在“文件名”框中，提供“\<name>.sqlplan”格式的名称，然后单击“保存”  。  
  
### <a name="to-open-a-saved-xml-query-plan-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中打开保存的 XML 查询计划  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的 **“文件”** 菜单上，选择 **“打开”** ，然后单击 **“文件”** 。  
  
2.  在“打开文件”对话框中，将“文件类型”设置为“执行计划文件(\*.sqlplan)”，以筛选出保存的 XML 查询计划文件的列表。  
  
3.  选择要查看的 XML 查询计划文件，然后单击 **“打开”** 。  
  
     此外，还可以在 Windows 资源管理器中双击扩展名为 **.sqlplan** 的文件。 该计划便会在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中打开。  
  
## <a name="see-also"></a>另请参阅  
 [SET SHOWPLAN_XML (Transact-SQL)](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [SET STATISTICS XML (Transact-SQL)](../../t-sql/statements/set-statistics-xml-transact-sql.md)  
  
  
