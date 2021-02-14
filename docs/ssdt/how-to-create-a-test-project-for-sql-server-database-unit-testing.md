---
title: 为 SQL Server 数据库单元测试创建测试项目
description: 了解如何为 SQL Server 数据库单元测试创建测试项目。 查看将测试项目添加到包含数据库项目的解决方案的不同方式。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4b3e7ba8-b565-4689-af1a-34cc255b7c60
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fe5cea5559f687b74fac93239b694a7736c6fcd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018327"
---
# <a name="how-to-create-a-test-project-for-sql-server-database-unit-testing"></a>如何：为 SQL Server 数据库单元测试创建测试项目

在开始编写评估数据库对象的单元测试之前，您必须首先创建测试项目。 该项目包含 SQL Server 单元测试，但它可以包含其他类型的测试。  
  
你可以将针对给定数据库项目的所有 SQL Server 单元测试置于单个测试项目中。 不过，根据您对以下问题的回答，您可能需要创建其他测试项目：  
  
|问题|决策|  
|-|-|   
|不同的 SQL Server 单元测试是否需要访问不同的数据库连接才能执行或验证测试？|如果是，则需要创建多个测试项目。 您无法为执行测试指定多个数据库连接。 不过，您可以为验证测试指定不同的数据库连接。|  
|您是否要为不同的单元测试部署不同的数据库项目？|如果是，则需要创建多个测试项目。 一个测试项目只能部署单个数据库项目。|  
  
有关上述每个问题的详细信息，请参见[如何：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。 作为创建多个测试项目的替代方法，你还可以提供自己的 [DatabaseTestService](/previous-versions/visualstudio/visual-studio-2010/dd154755(v=vs.100)) Microsoft.Data.Schema.UnitTesting.DatabaseTestService implementation 实现。  
  
您可以采用三种方法将测试项目添加到包含数据库项目的解决方案中：  
  
-   将测试项目添加到解决方案中。 测试项目包含标准单元测试，您可以删除该测试。 此项目不包含 SQL Server 单元测试类，你必须添加该类。  
  
-   从“测试”菜单添加新的 SQL Server 单元测试。 在添加单元测试时，SQL Server Data Tools 还会创建测试项目（如果需要的话）。 此项目包含 SQL Server 单元测试类。 SQL Server 单元测试类包含一个或多个单元测试。  
  
-   在 SQL Server 对象资源管理器中，从一个打开的项目的存储过程、函数或触发器中创建单元测试。 在创建单元测试时，SQL Server Data Tools 还会创建测试项目（如果需要的话）。 此项目包含 SQL Server 单元测试类。 SQL Server 测试类包含一个或多个单元测试。  
  
以下过程概述了每种方法。  
  
### <a name="to-add-a-test-project-to-an-existing-solution"></a>将测试项目添加到现有解决方案中  
  
1.  在 **“文件”** 菜单中，指向 **“新建”** ，然后单击 **“项目”** 。  
  
    将显示“新建项目”对话框。  
  
2.  在“已安装的模板”下，扩展“SQL Server”节点，然后选择“SQL Server 数据库项目”。  
  
3.  在“名称”中，键入项目名称。  
  
### <a name="to-create-a-test-project-with-a-sql-server-unit-test-class"></a>创建包含 SQL Server 单元测试类的测试项目  
  
-   请按照[如何：创建空 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)或[如何：为函数、触发器和存储过程创建 SQL Server 单元测试](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)中概述的过程进行操作。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
