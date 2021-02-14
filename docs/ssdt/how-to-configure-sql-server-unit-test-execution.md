---
title: 配置 SQL Server 单元测试执行
description: 了解如何配置 SQL Server 单元测试。 了解如何指定连接字符串以及如何部署数据库架构。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: e0179429-13ce-4d23-ae27-e6419de0a575
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 569262aece2d8b1212704c0b2ba443c6c63f2b07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018407"
---
# <a name="how-to-configure-sql-server-unit-test-execution"></a>如何：配置 SQL Server 单元测试执行

通过配置测试项目，可指定若干控制 SQL Server 单元测试的运行方式方面的设置。 这些配置设置存储在测试项目的 app.config 文件中。 如果您直接编辑此文件，则新值将出现在“测试配置”对话框中。  
  
您的解决方案可包含多个测试项目。 每个测试项目包含一个 app.config 文件（即，一组配置设置）。 因此，您的解决方案可包含各种配置为以不同方式运行的单元测试组（一组测试对应一个测试项目）。  
  
这些设置控制着测试如何连接到要测试的数据库、如何将数据库项目中的架构部署到该数据库：  
  
-   数据库连接。 使用此设置可指定用于连接到要测试的数据库的连接字符串。 有关详细信息，请参阅[指定连接字符串](#SpecifyConnectionStrings)  
  
-   架构部署。 数据库项目是数据库的脱机表示形式。 数据库项目表示数据库对象的结构，但不包含任何数据。 在数据库项目中做出架构更改后，您可在实际数据库中测试这些更改。 在架构部署步骤中，将要测试的数据库对象将会从您的数据库项目复制到您要对其运行测试的数据库中。 有关架构部署的更多信息，请参见[部署数据库架构](#DeployingDBSchema)。  
  
    > [!NOTE]  
    > 测试不在解决方案文件夹中运行，而是在本地硬盘上的一个单独的文件夹中运行。 虽然您可配置测试部署的各个方面，但通常不需要为单元测试配置它们。 有关测试部署的更多信息，请参见[运行测试](/previous-versions/visualstudio/visual-studio-2010/dd286680(v=vs.100))。  
  
## <a name="specify-connection-strings"></a><a name="SpecifyConnectionStrings"></a>指定连接字符串  
  
#### <a name="to-specify-database-connection-strings"></a>指定数据库连接字符串  
  
1.  在“解决方案资源管理器”中右键单击单元测试项目，然后单击“SQL Server 测试配置”。  
  
    将显示“SQL Server 测试配置 -‘<projectname>’”对话框。  
  
2.  在“数据库连接”下，可以执行下列操作：  
  
    -   单击要针对其执行单元测试的数据库连接。  
  
    -   如果要针对不同的数据库连接验证测试执行，则选中“使用辅助数据连接验证单元测试”复选框，然后单击列表中的数据库连接。  
  
    -   单击“新建连接”以向列表中添加一个连接。 还可单击“编辑连接”以修改现有连接的设置。  
  
    此步骤将创建一个用于在单元测试中执行测试脚本的 `ExecutionContext` 连接字符串。 如果您还指定了辅助连接，则还将创建 `PrivilegedContext` 连接字符串。 此连接用于在单元测试中的测试脚本之外测试与数据库的交互。 有关详细信息，请参阅[连接字符串和权限概述](../ssdt/overview-of-connection-strings-and-permissions.md)。  
  
3.  单击“确定”以关闭“SQL Server 测试配置 -‘<projectname>’”对话框。  
  
4.  重新生成测试项目以应用配置更改。  
  
## <a name="deploy-a-database-schema"></a><a name="DeployingDBSchema"></a>部署数据库架构  
  
#### <a name="to-deploy-to-a-database-the-schema-of-a-database-project"></a>将数据库项目的架构部署到数据库  
  
1.  在“解决方案资源管理器”中，右键单击数据库项目，然后单击“生成”。  
  
    生成数据库项目时，将生成 Transact\-SQL 脚本。 在对某个数据库运行此脚本时，将在该数据库中重新创建数据库项目的结构。  
  
2.  选择要配置的测试项目。  
  
3.  在“解决方案资源管理器”中右键单击单元测试项目，然后单击“SQL Server 测试配置”。  
  
    将显示“SQL Server 测试配置 -‘<projectname>’”对话框。  
  
4.  在“部署”下，可执行下列操作：  
  
    -   选中“运行测试前自动部署数据库项目”复选框以确保在运行测试前提交已对数据库项目进行的任何架构更改。  
  
    -   在“数据库项目”下，单击要部署的数据库项目，或单击省略号以通过浏览查找其他项目。 数据库项目文件的扩展名为 .dbproj。  
  
    -   在“部署配置”下，单击要针对其进行部署的项目配置。 可以选择“调试”、“默认值”或“发布”。 但是，如果您为单元测试创建配置，则该配置也将作为选项显示。  
  
5.  单击“确定”以关闭“SQL Server 测试配置 -‘<projectname>’”对话框。  
  
    在测试运行开始时，将运行在步骤 1 中生成的 Transact\-SQL 脚本。 此操作会将架构部署到目标数据库。  
  
6.  重新生成单元测试项目以应用配置更改。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
