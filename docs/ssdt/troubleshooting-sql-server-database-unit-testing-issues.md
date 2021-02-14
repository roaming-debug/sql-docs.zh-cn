---
title: 解决 SQL Server 数据库单元测试问题
description: 查看 SQL Server 单元测试中可能出现的问题（例如超时失败和数据库部署到意外目标）的故障排除提示。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cf4c9cd1-7e73-4c3b-922a-68b9247e7b33
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0bd87bb75255ca50ceaed8e46c356555cdedf799
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066862"
---
# <a name="troubleshooting-sql-server-database-unit-testing-issues"></a>解决 SQL Server 数据库单元测试问题

当你对某一数据库使用 SQL Server 单元测试时可能会遇到本主题中介绍的问题：  
  
-   [在运行单元测试时单元测试和 App.Config 更改被忽略](#UnitTestingAndAppConfigChanges)  
  
-   [在运行单元测试时数据库部署到意外的目标](#DatabaseDeploymentInUnitTests)  
  
-   [在运行数据库单元测试时发生超时](#TimeoutsDuringUnitTests)  
  
## <a name="unit-testing-and-appconfig-changes-ignored-when-you-run-unit-tests"></a><a name="UnitTestingAndAppConfigChanges"></a>在运行单元测试时单元测试和 App.Config 更改被忽略  
如果您在测试项目中修改 App.Config 文件，则必须首先重新生成该测试项目，然后这些更改才会生效。 这些更改包括你通过使用“SQL Server 测试配置”对话框对 App.Config 进行的更改。 如果您没有重新生成测试项目，则在运行单元测试时将不会应用这些更改。  
  
## <a name="database-deployment-to-unexpected-target-when-you-run-unit-tests"></a><a name="DatabaseDeploymentInUnitTests"></a>在运行单元测试时数据库部署到意外的目标  
如果您在运行单元测试时从某一数据库项目部署数据库，则通过使用在您的单元测试配置中指定的连接字符串信息来部署数据库。 在“数据库项目调试”属性中指定的连接信息不用于此任务，这允许你对同一个数据库的不同实例运行 SQL Server 单元测试。  
  
## <a name="timeouts-when-you-run-database-unit-tests"></a><a name="TimeoutsDuringUnitTests"></a>在运行数据库单元测试时发生超时  
如果您的数据库单元测试由于超时而失败，则可以通过在您的测试项目中更新 app.config 文件来增加超时期限。 在连接字符串上定义的连接超时指定单元测试连接到服务器时要等待多长时间。 命令超时（必须在 app.config 文件中直接定义）指定在单元测试执行 Transact\-SQL 脚本时要等待多长时间。 如果您有长时间运行的单元测试方面的问题，则尝试在相应上下文元素中增加该命令超时值。 例如，若对于 PrivilegedContext 元素将命令超时指定为 120 秒，则按如下所示更新 app.config：  
  
```  
<SqlUnitTesting_VS2010>  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\..\..\..\Visual Studio 2010\Projects\Database10\Database10\AdventureWorks.sqlproj"  
        Configuration="Debug" />  
    <DataGeneration ClearDatabase="true" />  
    <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="30" />  
    <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="120" />  
</SqlUnitTesting_VS2010>  
```  
  
## <a name="see-also"></a>另请参阅  
[如何：为函数、触发器和存储过程创建 SQL Server 单元测试](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[如何：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
  
