---
title: SQL Server 单元测试文件
description: 了解组成 SQL Server 单元测试的文件，如源代码文件、资源文件、配置文件和设置文件。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cee093c9-b97d-4fb0-b80f-806d071259dc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 48a8ba38c1af97f868162bdbc955006dbf646f55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066892"
---
# <a name="sql-server-unit-test-files"></a>SQL Server 单元测试文件

与托管代码的单元测试一样，SQL Server 单元测试位于测试项目中。 可以在“解决方案资源管理器”中查看在测试项目的层次结构中构成 SQL Server 单元测试的项。  
  
SQL Server 单元测试由包含在若干个文件中的多个项组成。 下表描述了这些相互作用以构成 SQL Server 单元测试的文件。  
  
|**File**|**说明**|  
|------------|-------------------|  
|.cs 或 .vb|此源代码文件包含一个使用 [TestClass] 属性修饰的类。 每个包含的 SQL Server 单元测试在此类中都有一个对应的测试方法。 这些方法可以使用 [TestMethod] 属性修饰。<br /><br />每个测试方法均包含用于执行 Transact\-SQL 测试脚本的适当代码。 此代码是在创建测试方法时生成的，您可以进行修改。<br /><br />**注意：** 如果在“解决方案资源管理器”中双击此文件，则会在 SQL Server 单元测试设计器中打开相应的测试类。 若要打开 .cs 或 .vb 文件以查看其源代码，请在“解决方案资源管理器”中右键单击该文件，然后单击“查看代码”。|  
|.resx|此资源文件为关联的 .cs 或 .vb 文件中的所有测试包含 Transact\-SQL 脚本。 这一组脚本包括预先测试脚本、测试脚本和后期测试脚本。 此资源文件包含可以编辑的 XML。 此资源文件将会编译到测试程序集中。<br /><br />应该使用“SQL Server 单元测试设计器”来编写 Transact\-SQL 脚本代码。 有关在 SQL Server 单元测试中使用的脚本的详细信息，请参阅 [SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)。|  
|app.config|此文件存储测试项目的数据库连接字符串以及其他 SQL Server 单元测试配置设置，例如命令超时。有关详细信息，请参阅 [SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)。|  
|SQLDatabaseSetup.cs 或 SQLDatabaseSetup.vb|该文件包含为测试项目中所有 SQL Server 单元测试准备测试环境的类。 基于 app.config 文件中的配置设置，它可以将 SQL Server 数据库项目部署到测试数据库。|  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)  
  
