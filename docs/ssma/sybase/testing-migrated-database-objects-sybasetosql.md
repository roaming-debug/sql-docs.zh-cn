---
description: 测试迁移的数据库对象 (SybaseToSQL)
title: " (SybaseToSQL) 中测试已迁移的数据库对象 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4937f6b4-86bd-4070-88df-3d216306c33a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f889538e5232eedaf3bab8006c54d56e1649ea4c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014783"
---
# <a name="testing-migrated-database-objects-sybasetosql"></a>测试迁移的数据库对象 (SybaseToSQL)
Microsoft SQL Server 迁移助手用于 Sybase 测试人员 (SSMA 测试器) 会自动测试数据库对象转换和 SSMA 进行的数据迁移。 完成所有 SSMA 迁移步骤后，使用 SSMA 测试人员验证转换后的对象是否以相同方式工作，并确保所有数据都已正确传输。  
  
> [!NOTE]  
> 在 Azure 连接的情况下，将禁用测试人员组件。  
  
可以通过 SSMA 测试人员测试以下对象类型：  
  
-   表  
  
-   存储过程  
  
-   视图。  
  
-   独立语句。  
  
SSMA 测试人员执行选择用于在 Sybase 上测试的对象及其在 SQL Server 中的相应对象。 之后，它会根据以下条件比较结果：  
  
-   表数据中的更改是否相同？  
  
-   过程和函数的输出参数的值是否相同？  
  
-   函数是否返回相同的结果？  
  
-   结果集是否相同？  
  
> [!NOTE]  
> 注意! 切勿在生产系统上使用 SSMA 测试人员。 在测试执行过程中，将修改源架构和数据。 同时，对于某些类型的已测试代码，完全还原原始状态可能是不可能的。  
  
## <a name="prerequisites"></a>先决条件  
如果要使用 SSMA 测试器，请在打开 " **安装测试人员数据库** " 选项的情况下安装 SSMA Sybase 扩展包。  
  
此外，请验证以下各项：  
  
-   在运行的计算机上安装了 Sybase OLE DB 提供程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   数据库引擎上已启用 CLR) 集成 (公共语言运行时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
请注意，当前版本的 SSMA 测试人员不支持同一源或目标服务器上的不同用户并行执行。  
  
## <a name="getting-started"></a>入门  
[&#40;SybaseToSQL&#41;创建测试用例 ](../../ssma/sybase/creating-test-cases-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅  
[在 SQL Server 上安装 SSMA 组件 &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
[&#40;转换的项目设置&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
