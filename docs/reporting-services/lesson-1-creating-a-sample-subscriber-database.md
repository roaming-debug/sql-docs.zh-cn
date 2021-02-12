---
title: 第 1 课：创建示例订阅服务器数据库 | Microsoft Docs
description: 了解如何创建小型“订阅者”数据库以存储将由数据驱动的订阅使用的订阅数据。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 47a882b7-efe5-4ee6-bef4-06118eb56903
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8be94f5ae47a7b7545cd553299673c823993402a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062982"
---
# <a name="lesson-1-creating-a-sample-subscriber-database"></a>第 1 课：创建示例订阅服务器数据库

本 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 教程课程中，会创建一个小型“订阅服务器”数据库，以存储将由数据驱动订阅使用的订阅数据。 处理订阅时，报表服务器将检索此数据并使用它来自定义报表输出。 例如，数据行包含用于筛选器的特定订单编号，以及生成报表将以何种文件格式进行创建。  
  
本课程假定使用 [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] 创建 SQL Server 数据库。  
  
### <a name="to-create-a-sample-subscriber-database"></a>创建示例订阅服务器数据库  
  
1.  启动 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]，打开到 [!INCLUDE[ssDEnoversion_md](../includes/ssdenoversion-md.md)]实例的连接。  
  
2.  右键单击“数据库”，选择“新建数据库”。  
  
3.  在“新建数据库”对话框的“数据库名称”中，键入“订阅者”。 
4. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  在工具栏中，单击“新建查询”按钮。  
  
6.  将下列 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句复制到空查询中：  
  
    ```  
    Use Subscribers  
    CREATE TABLE [dbo].[OrderInfo] (  
        [SubscriptionID] [int] NOT NULL PRIMARY KEY ,  
        [Order] [nvarchar] (20) NOT NULL,  
        [FileType] [bit],  
        [Format] [nvarchar] (20) NOT NULL ,  
    ) ON [PRIMARY]  
    GO  
  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('1', 'so43659', '1', 'IMAGE')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('2', 'so43664', '1', 'MHTML')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('3', 'so43668', '1', 'PDF')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('4', 'so71949', '1', 'Excel')  
    GO  
    ```  
  
7.  单击工具栏上的“!在工具栏上执行。  
  
8.  使用 SELECT 语句查看您是否有三行数据。 例如： `select * from OrderInfo`  
  
## <a name="next-steps"></a>后续步骤  
+ 您已成功创建了订阅数据，这些数据将为每个订阅服务器驱动报表分发并改变报表输出。 
+ 下一步，修改报表的数据源属性，以使用已存储凭据。 
+ 您还将修改报表设计以便包括订阅将用于订阅服务器数据的参数。 [第 2 课：修改报表数据源属性](../reporting-services/lesson-2-modifying-the-report-data-source-properties.md)。  

## <a name="next-steps"></a>后续步骤

[创建数据驱动的订阅](../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)  
[创建数据库](../relational-databases/databases/create-a-database.md)  
[创建基本表报表](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
