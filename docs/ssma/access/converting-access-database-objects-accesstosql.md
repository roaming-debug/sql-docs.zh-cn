---
title: 将 Access 数据库对象转换 (AccessToSQL) |Microsoft Docs
description: 了解如何在连接到 SQL Server/Azure SQL 数据库后选择访问数据库对象，然后将架构转换为 SQL Server/SQL 数据库架构。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, converting schemas
- conversion
- conversion, converting schemas
- indexes, altering
- metadata
- metadata, altering
- metadata, converting
- migrating databases, one-click
- one-click migration
- schemas
- schemas, converting
- SQL
- SQL, converting
- syntax
- syntax, converting
- tables, altering
- translating Access to SQL Azure
- translating Access to SQL Server
ms.assetid: e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 38385657de137e9e13bf03f7a4209be1303452d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076072"
---
# <a name="converting-access-database-objects-accesstosql"></a>将 Access 数据库对象转换 (AccessToSQL) 
添加访问数据库并连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 后，SSMA 将显示 Access 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库对象的元数据。 你现在可以选择 "访问数据库对象"，然后将架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 架构。  
  
## <a name="the-conversion-process"></a>转换过程  
转换数据库对象将从访问元数据获取对象定义，并将其转换为等效 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的语法，然后将此信息加载到项目中。 然后，可以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器查看或 SQL Azure 对象及其属性。  
  
> [!IMPORTANT]  
> 转换对象不会在或 SQL Azure 中创建对象 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 它只转换对象定义，并将信息存储在 SSMA 项目中。  
  
在转换过程中，SSMA 会将状态输出到 "输出" 窗格，并将错误、警告和信息性消息输出到 "错误列表" 窗格中。 使用此信息来确定是否需要修改 Access 数据库或转换过程以获取所需的转换结果。 你还可以使用为 [迁移准备 Access 数据库](preparing-access-databases-for-migration-accesstosql.md) 主题中的信息来确定不会转换哪些内容。  
  
## <a name="setting-conversion-options"></a>设置转换选项  
转换对象之前，请在 " **项目设置** " 对话框中查看项目转换选项。 使用此对话框，可以设置 SSMA 如何转换已编制索引的备注列、主键、外键约束、时间戳和无索引的表。 有关详细信息，请参阅 [项目设置 (转换) ](./project-settings-conversion-accesstosql.md)  
  
## <a name="conversion-results"></a>转换结果  
下表显示了要转换的访问对象以及生成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的对象：  
  
|Access 对象|产生 SQL Server 对象|  
|-----------------|-------------------------------|  
|表|表|  
|列|列|  
|索引|索引|  
|外键 (foreign key)|外键 (foreign key)|  
|query|view<br /><br />大多数 SELECT 查询都转换为视图。 其他查询（如更新查询）不会迁移。<br /><br />不会转换带参数的 SELECT 查询，也不会进行交叉选项卡查询。|  
|report|未转换|  
|表单|未转换|  
|宏|未转换|  
|name|未转换|  
|默认值|默认值|  
|允许零长度列属性|check 约束|  
|列验证规则|check 约束|  
|表验证规则|check 约束|  
|主键 (primary key)|主键 (primary key)|  
  
## <a name="converting-access-objects"></a>转换 Access 对象  
若要转换 Access 数据库对象，您必须首先选择要转换的对象，然后让 SSMA 执行转换。 若要在转换过程中查看输出消息，请在 " **视图** " 菜单上选择 " **输出**"。  
  
**选择 Access 数据库对象并将其转换为 SQL Server 或 SQL Azure 语法**  
  
1.  在 Access 元数据资源管理器中，展开 " **access-元数据库**"，然后展开 " **数据库**"。  
  
2.  执行以下一项或多项操作：  
  
    -   若要转换所有数据库，请选中 " **数据库**" 旁边的复选框。  
  
    -   若要转换或省略单个数据库，请选中或清除数据库名称旁边的复选框。  
  
    -   若要转换或省略查询，请展开数据库，然后选中或清除 " **查询** " 复选框。  
  
    -   若要转换或省略单个表，请展开数据库，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
3.  执行下列操作之一：  
  
    -   若要转换架构，请右键单击 " **数据库** "，然后选择 " **转换架构**"。  
  
        您还可以转换单个对象。 若要转换对象，无论选择了哪个对象，右键单击该对象并选择 " **转换架构**"。  
  
        当对象已转换后，它将在 "访问元数据资源管理器" 中显示为粗体。  
  
    -   若要在一个步骤中转换、加载和迁移架构和数据，请右键单击 "数据库"，然后选择 " **转换"、"加载" 和 "迁移**"。  
  
4.  在 " **输出** " 窗格中查看消息，在 " **错误列表** " 窗格中查看所有错误和警告。  
  
## <a name="altering-tables-and-indexes"></a>更改表和索引  
在将访问元数据转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据，并在将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之前，可以更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表和索引。  
  
**更改表或索引属性**  
  
1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中，选择要更改的表或索引。  
  
2.  在 " **表** " 选项卡上，单击要更改的属性，然后输入或选择新设置。 例如，可以将 nvarchar (15) 更改为 nvarchar (20) ，或选中复选框以使表列可以为 null。  
  
    将光标移出已更改的属性单元。 可以通过单击另一行或按 Tab 键来执行此操作。  
  
3.  单击“应用”。  
  
你现在可以在 " **SQL** " 选项卡上查看代码中的更改。  
  
## <a name="next-steps"></a>后续步骤  
迁移过程的下一步是将已 [转换的数据库对象加载到 SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
