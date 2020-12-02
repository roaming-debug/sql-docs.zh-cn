---
description: 使用 SQL Server 目标大容量加载数据
title: 使用 SQL Server 目标大容量加载数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SQL Server destination
- loading data
- destinations [Integration Services], SQL Server
- inserting data
- bulk load [Integration Services]
ms.assetid: 8f982f85-a82e-4e2d-9cd8-cd2f85402d8e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3bce4cae8fc71c93487f65a726bb42f58618d5a7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127320"
---
# <a name="bulk-load-data-by-using-the-sql-server-destination"></a>使用 SQL Server 目标大容量加载数据

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  若要添加并配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标，则包必须已包含至少一个数据流任务和一个数据源。  
  
### <a name="to-load-data-using-a-sql-server-destination"></a>使用 SQL Server 目标加载数据  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，再将 **目标从**“工具箱” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 拖动到设计图面。  
  
4.  将连接线拖到目标，从而将目标连接到数据流中的源或前一转换。  
  
5.  双击此目标。  
  
6.  在 **“SQL Server 目标编辑器”** 中的 **“连接管理器”** 页上，选择一个现有的 OLE DB 连接管理器或单击 **“新建”** 创建新的连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
7.  若要指定把数据加载到其中的表或视图，请执行下列操作之一：  
  
    -   选择一个现有的表或视图。  
  
    -   单击“新建”，并在“创建表”对话框中写入创建表或视图的 SQL 语句。  
  
        > [!NOTE]  
        >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将基于连接数据源生成一个默认的 CREATE TABLE 语句。 即使源表包含一个已声明了 FILESTREAM 属性的列，此默认 CREATE TABLE 语句也不会包含 FILESTREAM 属性。 若要运行具有 FILESTREAM 属性的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件，首先要在目标数据库上实现 FILESTREAM 存储。 然后在 **“创建表”** 对话框中将 FILESTREAM 属性添加到 CREATE TABLE 语句中。 有关详细信息，请参阅[二进制大型对象 (Blob) 数据 (SQL Server)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)。  
  
8.  单击 **“映射”** ，并将列从一个列表拖动到另一个列表，从而将 **“可用输入列”** 列表中的列映射到 **“可用目标列”** 列表中的列。  
  
    > [!NOTE]  
    >  目标自动映射名称相同的列。  
  
9. 单击 **“高级”** ，并设置大容量加载选项： **“保留标识”**、 **“保留空值”**、 **“表锁”**、 **“检查约束”** 和 **“激发触发器”**。  
  
     也可以指定要插入的第一个和最后一个输入行、在插入操作停止前可以出现的最大错误数以及插入据以排序的列。  
  
    > [!NOTE]  
    >  排序顺序由列所列出的顺序确定。  
  
10. 单击“确定”。  
  
11. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Destination](../../integration-services/data-flow/sql-server-destination.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Integration Services 路径](../../integration-services/data-flow/integration-services-paths.md)   
 [数据流任务](../../integration-services/control-flow/data-flow-task.md)  
  
  
