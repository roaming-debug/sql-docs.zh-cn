---
description: 创建 DML 触发器
title: 创建 DML 触发器 | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], DML triggers
- deferred name resolution, DML triggers
- WITH ENCRYPTION clause
- IF UPDATE
- SET statement, DML triggers
- DML triggers, programming
- testing column changes
- results [SQL Server], DML triggers
ms.assetid: b2b52258-642b-462e-8e0f-18c09d2eccf4
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48d3f41bb6001676492423385b96e212de068d6f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403250"
---
# <a name="create-dml-triggers"></a>创建 DML 触发器
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题介绍了如何通过使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] CREATE TRIGGER 语句来创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] DML 触发器。  
  
##  <a name="before-you-begin"></a><a name="Top"></a> 开始之前  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
 有关与创建 DML 触发器相关的限制和局限的列表，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对要创建触发器的表或视图具有 ALTER 权限。  
  
##  <a name="how-to-create-a-dml-trigger"></a><a name="Procedures"></a> 如何创建 DML 触发器  
 您可以使用以下项之一：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”**，展开 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库，展开 **“表”** ，然后展开表 **Purchasing.PurchaseOrderHeader**。  
  
3.  右键单击“触发器”，然后选择“新建触发器”。  
  
4.  在 **“查询”** 菜单上，单击 **“指定模板参数的值”** 。 或者，你可以按下 (Ctrl-Shift-M) 以便打开“指定模板参数的值”对话框。  
  
5.  在 **“指定模板参数的值”** 对话框中，输入下列所示的参数值。  
  
    |参数|值|  
    |---------------|-----------|  
    |作者|*您的姓名*|  
    |创建日期|*今天的日期*|  
    |说明|在允许插入具有供应商的新采购订单之前，请检查供应商信用等级。|  
    |Schema_Name|购买|  
    |Trigger_Name|NewPODetail2|  
    |Table_Name|PurchaseOrderDetail|  
    |Data_Modification_Statement|从列表中删除 UPDATE 和 DELETE。|  
  
6.  单击“确定”。   
  
7.  在 **“查询编辑器”** 中，使用以下语句替换注释 `-- Insert statements for trigger here` ：  
  
    ```sql  
    IF @@ROWCOUNT = 1  
    BEGIN  
       UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal + LineTotal  
       FROM inserted  
       WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
  
    END  
    ELSE  
    BEGIN  
          UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal +   
          (SELECT SUM(LineTotal)  
          FROM inserted  
          WHERE PurchaseOrderHeader.PurchaseOrderID  
           = inserted.PurchaseOrderID)  
       WHERE PurchaseOrderHeader.PurchaseOrderID IN  
          (SELECT PurchaseOrderID FROM inserted)  
    END;  
    ```  
  
8.  若要验证语法是否有效，请在 **“查询”** 菜单上单击 **“分析”**。 如果返回错误消息，则请将该语句与上述信息进行比较，视需要进行更正并且重复此步骤。  
  
9. 若要创建 DML 触发器，请在 **“查询”** 菜单上单击 **“执行”**。 该 DML 触发器作为数据库中的对象创建。  
  
10. 若要查看在“对象资源管理器”中列出的 DML 触发器，请右键单击“触发器”，然后选择“刷新”。  

 [开始之前](#Top)  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  从 **“文件”** 菜单中，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例将创建与上面相同的存储的 DML 触发器。  
  
    ```sql  
    -- Trigger valid for multirow and single row inserts  
    -- and optimal for single row inserts.  
    USE AdventureWorks2012;  
    GO  
    CREATE TRIGGER NewPODetail3  
    ON Purchasing.PurchaseOrderDetail  
    FOR INSERT AS  
    IF @@ROWCOUNT = 1  
    BEGIN  
       UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal + LineTotal  
       FROM inserted  
       WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
  
    END  
    ELSE  
    BEGIN  
          UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal +   
          (SELECT SUM(LineTotal)  
          FROM inserted  
          WHERE PurchaseOrderHeader.PurchaseOrderID  
           = inserted.PurchaseOrderID)  
       WHERE PurchaseOrderHeader.PurchaseOrderID IN  
          (SELECT PurchaseOrderID FROM inserted)  
    END;  
    ```  
  
 
  
