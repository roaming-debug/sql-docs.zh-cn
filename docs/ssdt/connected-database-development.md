---
title: 连接的数据库开发
description: 了解 SQL Server Data Tools 如何与连接的数据库一起工作。 了解如何浏览实体、设计表、编辑脚本和执行其他任务。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 21f7f959-7b8e-4335-8681-bebcd957692c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 46dd6a4df85b57cbd18817debf55f95d6cf982e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077164"
---
# <a name="connected-database-development"></a>连接的数据库开发

本节介绍 SQL Server Data Tools 为设计和查询连接的数据库而提供的功能。  
  
利用 Visual Studio 中的 SQL Server 对象资源管理器，开发人员现在可以创建、编辑和浏览位于内部数据库服务器（例如 SQL Server 2008 或 Microsoft SQL Server 2012）或 SQL Azure 中的外部数据库服务器中的数据库对象。 开发人员可以轻松地将现有生产数据库克隆到一个测试实例、对其执行其他开发工作，最后将更改发布回生产数据库。  
  
> [!NOTE]  
> 本节中的操作指南主题包含一系列可按顺序完成的任务。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|描述|  
|---------|---------------|  
|[如何：连接到数据库并浏览现有对象](../ssdt/how-to-connect-to-a-database-and-browse-existing-objects.md)|连接到数据库并浏览其实体。|  
|[如何：使用表设计器创建数据库对象](../ssdt/how-to-create-database-objects-using-table-designer.md)|使用新的表设计器设计表和管理表关系。|  
|[如何：使用 Power Buffer 更新连接的数据库](../ssdt/how-to-update-a-connected-database-with-power-buffer.md)|更新连接的数据库而无需编写 ALTER 脚本。|  
|[“筛选和排序”对话框](../ssdt/filter-and-sort-dialog-box.md)|指定哪些数据应在数据视图中显示。|  
|[如何：使用查询创建新的数据库对象](../ssdt/how-to-create-new-database-objects-using-queries.md)|使用 Transact\-SQL 编辑器编辑和执行 Transact\-SQL 脚本。|  
|[如何：使用查询编辑现有表](../ssdt/how-to-edit-an-existing-table-using-queries.md)|编写 Transact\-SQL 脚本以便编辑表的定义或填充数据。|  
|[如何：查看和编辑表中的数据](../ssdt/how-to-view-and-edit-data-in-a-table.md)|使用数据编辑器查看表中数据或向表中输入数据。|  
|[如何：删除对象和解析依赖关系](../ssdt/how-to-delete-objects-and-resolve-dependencies.md)|重命名或删除数据库实体，并让 SQL Server Data Tools 自动解析对象依赖关系。|  
|[如何：克隆现有数据库](../ssdt/how-to-clone-an-existing-database.md)|从生产数据库创建一个开发数据库。|  
|[提取、发布和注册 .dacpac 文件](../ssdt/extract-publish-and-register-dacpac-files.md)|显示如何提取和发布 .dacpac 文件。|  
  
## <a name="related-sections"></a>相关章节

[管理表、关系和修复错误](../ssdt/manage-tables-relationships-and-fix-errors.md)