---
title: 为函数、触发器和存储过程创建 SQL Server 单元测试
description: 了解如何使用 SQL Server 对象资源管理器从数据库函数、触发器或存储过程创建 SQL Server 单元测试。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: bda57c10-a1ab-4a1a-8a71-42085a3cb793
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2f99946dfb56e3fc1e3e67f42ca93ff5daa11cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018267"
---
# <a name="how-to-create-sql-server-unit-tests-for-functions-triggers-and-stored-procedures"></a>如何：为函数、触发器和存储过程创建 SQL Server 单元测试

您可以编写单元测试来评估对任何数据库对象进行的更改。 但是，SQL Server Data Tools 包括从 SQL Server 对象资源管理器中的数据库项目节点为数据库函数、触发器和存储过程创建测试的附加支持。 Transact\-SQL 代码存根可自动为你生成以便进行自定义。  
  
### <a name="to-create-a-sql-server-unit-test-from-a-function-trigger-or-stored-procedure"></a>从函数、触发器或存储过程创建 SQL Server 单元测试  
  
1.  有关为存储过程添加单元测试的示例，请参阅“创建针对存储过程的 SQL Server 单元测试”部分中的[演练：创建并运行 SQL Server 单元测试](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)。  
  
    无结论的测试条件是添加到每个测试中的默认条件。 包含此测试条件是为了指示尚未执行测试验证。 在添加其他测试条件之后，请将此测试条件从测试中删除。 有关详细信息，请参阅[操作说明：向 SQL Server 单元测试添加测试条件](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[如何：创建空的 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
  
