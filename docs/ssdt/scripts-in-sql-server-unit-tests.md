---
title: SQL Server 单元测试中的脚本
description: 熟悉 SQL Server 单元测试脚本。 了解预先测试脚本、测试脚本和后期测试脚本，以及公用脚本 TestInitialize 和 TestCleanup。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 80c5cf62-a9c9-4e9d-8c6f-8eed50a595a7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: eb725a491eb87a0116413a786c514a0ede98361f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066942"
---
# <a name="scripts-in-sql-server-unit-tests"></a>SQL Server 单元测试中的脚本

每个 SQL Server 单元测试都包含单个预先测试操作、测试操作和后期测试操作。 其中每个操作又包含以下内容：  
  
-   对数据库执行的 Transact\-SQL 脚本。  
  
-   评估从脚本执行返回的结果的零个或多个测试条件。  
  
测试操作中的 Transact\-SQL 测试脚本是唯一一个必须包括在每个 SQL Server 单元测试中的组件。 除了测试脚本本身外，您可能还希望指定测试条件以验证测试脚本是否返回了预期的一个值或一组值。 测试操作运用或更改该数据库中的特定对象，然后评估该更改。  
  
对于每个测试操作，可以包括一个预先测试操作和一个后期测试操作。 与测试操作类似，每个预先测试操作和每个后期测试操作都包含一个 Transact\-SQL 脚本和零个或多个测试条件。 可以使用预先测试操作来确保数据库的状态允许测试操作运行并能返回有意义的结果。 例如，可以在测试脚本对数据执行操作之前使用预先测试操作验证表是否包含数据。 在预先测试操作准备好数据库并且测试操作返回了有意义的结果后，可以使用后期测试操作将数据库返回到运行预先测试之前的状态。 或者，在一些情况下，您可能使用后期测试操作来验证测试操作的结果。 这是因为后期测试操作可以拥有比测试操作更大的数据库特权。 有关详细信息，请参阅[连接字符串和权限概述](../ssdt/overview-of-connection-strings-and-permissions.md)。  
  
除了这三种操作外，还有两个测试脚本（称为公用脚本），它们在每个 SQL Server 单元测试运行之前和之后运行。 因此，在单个 SQL Server 单元测试的执行期间，最多可以运行五个 Transact\-SQL 脚本。 仅测试操作中包含的 Transact\-SQL 脚本是必需的；公用脚本以及预先测试和后期测试操作脚本是可选的。  
  
下表提供了与任何 SQL Server 单元测试关联的脚本的完整列表。  
  
|**Action**|**脚本类型**|**说明**|  
|--------------|-------------------|-------------------|  
|TestInitialize|公用脚本（初始化）|（可选）此脚本在单元测试中的所有预先测试和测试操作之前。 TestInitialize 脚本在给定测试类中的每个单元测试之前运行。 此脚本使用特权上下文执行。|  
|预先测试|测试脚本|（可选）此脚本是单元测试的一部分。 预先测试脚本在单元测试中的测试操作之前运行。 此脚本使用特权上下文执行。|  
|测试|测试脚本|（必需）此脚本是单元测试的一部分。 例如，此脚本可能运行获取、插入或更新表值的存储过程。 此脚本使用执行上下文执行。|  
|后期测试|测试脚本|（可选）此脚本是单元测试的一部分。 后期测试脚本在单个单元测试之后运行。 此脚本使用特权上下文执行。|  
|TestCleanup|公用脚本（清理）|（可选）此脚本在单元测试之后。 TestCleanup 脚本在给定测试类中的所有单元测试之后运行。 此脚本使用特权上下文执行。|  
  
有关其中每个脚本执行的不同安全上下文的详细信息，请参阅[连接字符串和权限概述](../ssdt/overview-of-connection-strings-and-permissions.md)和 [SQL Server Data Tools 所需权限](../ssdt/required-permissions-for-sql-server-data-tools.md)中的 SQL Server 单元测试权限部分。  
  
## <a name="order-in-which-scripts-are-run"></a>脚本的运行顺序  
必须了解每个脚本的运行顺序。 虽然您无法更改该顺序，但可以决定要运行的脚本。 下图包括可以在包含两个 SQL Server 单元测试的测试运行中使用的脚本选择并显示了它们的运行顺序：  
  
![两个数据库单元测试](../ssdt/media/twodatabaseunittests.png "两个数据库单元测试")  
  
> [!NOTE]  
> 如果已配置 SQL Server 数据库项目部署，则该部署将基于特权上下文连接字符串在测试运行开始时发生。 有关详细信息，请参阅[操作说明：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。  
  
## <a name="initialization-and-cleanup-scripts"></a>初始化和清理脚本  
在 SQL Server 单元测试设计器中，TestInitialize 和 TestCleanup 脚本被称为公用脚本。 前面的示例假定两个单元测试是同一测试类的一部分。 因此，它们共享相同的 TestInitialize 和 TestCleanup 脚本。 对于单个测试类中的所有单元测试，始终是这种情况。 不过，如果测试运行包含来自不同测试类的单元测试，则相关测试类的公用脚本将在单元测试运行之前和之后运行。  
  
如果仅使用 SQL Server 单元测试设计器编写单元测试，则可能不熟悉测试类的概念。 每次通过打开“测试”菜单并单击“新建测试”来创建单元测试时，SQL Server Data Tools 都会生成测试类。 测试类显示在“解决方案资源管理器”中，并使用你指定的测试名称后跟 .cs 或 .vb 扩展名。 在每个测试类中，各个单元测试存储为测试方法。 不过，不管测试方法（即单元测试）的数量是多少，每个测试类可以具有零个或一个 TestInitialize 和 TestCleanup 脚本。  
  
您可以使用 TestInitialize 脚本来准备测试数据库，并可以使用 TestCleanup 脚本使测试数据库返回到已知状态。 例如，您可以在测试脚本中使用 TestInitialize 来创建稍后运行的帮助器存储过程以测试不同的存储过程。  
  
## <a name="pre-test-and-post-test-scripts"></a>预先测试和后期测试脚本  
与预先测试和后期测试操作关联的脚本可能因单元测试而异。 您可以使用这些脚本来制定对数据库的增量更改，然后清理这些更改。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
  
