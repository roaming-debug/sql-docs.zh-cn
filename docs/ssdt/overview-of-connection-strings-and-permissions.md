---
title: 连接字符串和权限
description: 了解运行 SQL Server 单元测试所需的连接字符串、帐户和权限。 请参阅如何配置连接字符串。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ceff114e-a738-46ad-9785-b6647a2247f9
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 70387a9e18888a47522613b7a76a2483aec0d287
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080808"
---
# <a name="overview-of-connection-strings-and-permissions"></a>连接字符串和权限概述

若要运行 SQL Server 单元测试，必须通过使用一个或两个特定连接字符串来连接数据库服务器。 每个连接字符串均表示一个具有特定权限的帐户，您必须具有相关权限才能在特定脚本中执行作为测试的一部分的某个任务/某一组任务。 可以在“SQL Server 测试配置”对话框中指定这些字符串，或者通过手动编辑测试项目的 app.config 文件来指定这些字符串。  
  
## <a name="connection-strings"></a>连接字符串  
在“SQL Server 测试配置”对话框中，可以为下列每个帐户指定连接字符串。  
  
> [!NOTE]  
> 只有在使用 SQL Server 身份验证的情况下执行上下文和特权上下文才会不同。 如果使用 Windows 身份验证，则将对这两个连接字符串使用相同的凭据。  
  
-   执行上下文（必需）- 运行测试脚本的用户帐户。 此连接字符串应具有您预期用户会拥有的相同凭据。 这一点非常重要，因为它将确保会将适当的权限应用于您的数据库。 有关详细信息，请参阅[操作说明：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。  
  
    在测试项目的 app.config 文件中，这是 `ExecutionContext` 元素。  
  
-   特权上下文（可选）- 在同一个数据库上具有用于运行预先测试操作、后期测试操作、TestInitialize 和 TestCleanup 脚本的更高权限的帐户。 这些脚本可设置数据库状态，并且对于后期测试操作，可用来这些脚本验证数据库中的对象。 此连接字符串还可用来部署数据库更改和生成数据。  
  
    在测试项目的 app.config 文件中，这是 `PrivilegedContext` 元素。 如果你的 SQL Server 单元测试只运行测试脚本，则不必指定特权上下文。  
  
您在“项目配置”对话框中指定的字符串将会存储在测试项目的 app.config 文件中。 您还可以直接编辑此文件并重新生成项目，在完成此操作之后，新的值将会出现此对话框中。  
  
## <a name="windows-authentication-versus-sql-server-authentication"></a>Windows 身份验证和 SQL Server 身份验证  
当指定连接字符串时，您必须在使用 Windows 身份验证和 SQL 身份验证之间进行选择。 选择 Windows 身份验证的一个原因是它对团队使用测试的支持优于 SQL Server 身份验证。 如果选择 SQL Server 身份验证，则会使用数据保护 API (DPAPI) 基于你的用户凭据来对连接字符串进行加密。 这意味着在签入此测试项目中的测试后，将只为你（而非通过源控制系统获取测试的团队成员）运行这些测试。 若要运行此测试项目中测试，团队中的其他成员必须使用自己的凭据重新配置测试项目。 为此，他们可以编辑 app.config 文件的副本或使用“项目配置”对话框。  
  
## <a name="permissions"></a>权限  
测试脚本在执行上下文权限级别运行，在数据库上运行的用户命令的典型用法也是在此权限级别运行。 预先测试操作、后期测试、TestInitialize 和 TestCleanup 脚本在特权上下文权限级别运行。  
  
由于对后期测试操作脚本使用了更高权限连接，因此您可以在其中执行验证。 在此脚本中，您还可以运行测试权限的脚本命令。 有关权限的详细信息，请参阅 [SQL Server Data Tools 所需权限](../ssdt/required-permissions-for-sql-server-data-tools.md)的 SQL Server 单元测试部分。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)  
[SQL Server 单元测试文件](../ssdt/sql-server-unit-test-files.md)  
  
