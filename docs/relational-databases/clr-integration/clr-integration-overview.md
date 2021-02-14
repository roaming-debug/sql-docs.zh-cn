---
title: CLR 集成概述 |Microsoft Docs
description: 承载 CLR Microsoft SQL Server 称为 CLR 集成。 在托管代码中创作可以提高性能。 SQL Server 使用 CAS 来帮助保护托管代码。
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], about CLR integration
- extended stored procedures [SQL Server], vs. managed code
- objects [CLR integration]
- Transact-SQL vs. managed code
- managed code [SQL Server], vs. Transact-SQL
- managed code [SQL Server], vs. extended stored procedures
- execution at client vs. execution at server [CLR integration]
ms.assetid: 5aa176da-3652-4afa-a742-4c40c77ce5c3
author: rothja
ms.author: jroth
ms.openlocfilehash: 667ded44c653f71902893f160b6231ad8b751789
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341456"
---
# <a name="clr-integration---overview"></a>CLR 集成 - 概述
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  公共语言运行时 (CLR) 是 Microsoft .NET Framework 的核心，它为所有 .NET Framework 代码提供执行环境。 在 CLR 中运行的代码称为托管代码。 CLR 提供执行程序所需的各种函数和服务，包括实时 (JIT) 编译、分配和管理内存、强制类型安全、异常处理、线程管理和安全性。  有关详细信息，请参阅 .NET Framework SDK。  
  
 使用在 Microsoft SQL Server 中驻留的 CLR（称为 CLR 集成），可以在托管代码中编写存储过程、触发器、用户定义函数、用户定义类型和用户定义聚合。 由于托管代码在执行之前才编译为本机代码，因此在某些情形下可以大幅提高性能。  
  
 托管代码使用代码访问安全性 (CAS) 来阻止程序集执行某些操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 CAS 来帮助保护托管代码，并阻止对操作系统或数据库服务器的侵害。  
  
## <a name="advantages-of-clr-integration"></a>CLR 集成的优点  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 是为了在数据库中直接进行数据访问和操纵而专门设计的。 虽然 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在数据访问和管理方面表现很好，但它不是完整的编程语言。 例如，[!INCLUDE[tsql](../../includes/tsql-md.md)] 不支持数组、集合、for-each 循环、移位或类。 虽然可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中模拟某些这样的构造，但托管代码已经集成了对这些构造的支持。 根据具体情况，这些功能足以为在托管代码中实现某些数据库功能提供充分理由。  
  
 Microsoft Visual Basic .NET 和 Microsoft Visual C# 提供面向对象的功能，例如包装、继承和多态性。 现在可以很容易将相关代码组织到类和命名空间中。 如果正在处理大量服务器代码，这将允许您更容易组织和维护代码。  
  
 托管代码比 [!INCLUDE[tsql](../../includes/tsql-md.md)] 更适合用于计算和复杂的执行逻辑，并可提供对很多复杂任务（包括字符串处理和正则表达式）的广泛支持。 使用在 .NET Framework 库中提供的功能，可以访问数千个预建的类和例程。 可以很容易从任何存储过程、触发器或用户定义函数访问它们。 基类库 (BCL) 包括可为字符串操纵、高级数学操作、文件访问、加密等提供功能的类。  
  
> [!NOTE]  
>  虽然可以在 SQL Server 中从 CLR 代码的内部使用很多这样的类，但那些不适合用于服务器端的类（例如，窗口类）将不可用。 有关详细信息，请参阅 [支持的 .NET Framework 库](../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md)。  
  
 托管代码的一个好处是类型安全，即确保代码仅以明确定义和许可的方式访问类型。 在执行托管代码之前，CLR 将验证该代码是否安全。 例如，系统将检查代码以确保其不会读取以前未曾写入的内存。 CLR 还可以帮助确保代码不会操纵非托管内存。  
  
 CLR 集成为提高性能提供了可能。 有关信息，请参阅 [CLR 集成的性能](../../relational-databases/clr-integration/clr-integration-architecture-performance.md)。  
 
> [!WARNING]
>  CLR 在 .NET Framework 中使用代码访问安全性 (CAS)（不可再作为安全边界）。 使用 `PERMISSION_SET = SAFE` 创建的 CLR 程序集可以访问外部系统资源、调用非托管代码以及获取 sysadmin 特权。 从 [!INCLUDE[sssql17](../../includes/sssql17-md.md)] 开始，引入了名为 `clr strict security` 的 `sp_configure` 选项，以增强 CLR 程序集的安全性。 默认启用 `clr strict security`，并将 `SAFE` 和 `EXTERNAL_ACCESS` 程序集与标记为 `UNSAFE` 的程序集同等对待。 可禁用 `clr strict security` 选项以实现后向兼容性，但不建议这样做。 Microsoft 建议所有程序集都通过证书或非对称密钥进行签名，且该证书或非对称密钥具有已在主数据库中获得 `UNSAFE ASSEMBLY` 权限的相应登录名。 有关详细信息，请参阅 [CLR 严格安全性](../../database-engine/configure-windows/clr-strict-security.md)。 
  
## <a name="choosing-between-transact-sql-and-managed-code"></a>选择 Transact-SQL 还是托管代码  
 编写存储过程、触发器和用户定义函数时，必须决定是使用传统的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 还是使用诸如 Visual Basic .NET 或 Visual C# 这样的 .NET Framework 语言。 如果代码主要执行没有或只有很少过程逻辑的数据访问，请使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 如果要编写有复杂逻辑并且 CPU 占用量大的函数和过程，或者想使用 .NET Framework 的 BCL，则使用托管代码。  
  
### <a name="choosing-between-execution-in-the-server-and-execution-in-the-client"></a>选择在服务器中执行还是在客户端中执行  
 影响使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 还是托管代码的另一个因素是您想将代码驻留在服务器计算机上，还是在客户端计算机上。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和托管代码都可以在服务器上运行。 这种方式可以将代码和数据靠近放在一起，并允许您利用服务器的强大处理能力。 另一方面，您可能希望避免将处理器占用量大的任务放在数据库服务器上。 目前大多数客户端计算机都有非常强大的功能，因此您可能希望通过将尽可能多的代码放在客户端上，来利用这种处理能力。 托管代码可以在客户端计算机上运行，而 [!INCLUDE[tsql](../../includes/tsql-md.md)] 不能。  
  
## <a name="choosing-between-extended-stored-procedures-and-managed-code"></a>选择扩展存储过程还是托管代码  
 可以生成扩展存储过程来执行使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程无法实现的功能。 但是，扩展存储过程可能有损于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的完整性，而经过验证确定为类型安全的托管代码则不会。 进一步来说，在 CLR 的托管代码与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间更深入地集成了内存管理、线程及纤程的调度以及同步服务。 如果所编写的存储过程需要执行在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中不可能完成的任务，则 CLR 集成有比扩展存储过程更安全的方式来实现它。 有关 CLR 集成和扩展存储过程的详细信息，请参阅 [Clr 集成的性能](../../relational-databases/clr-integration/clr-integration-architecture-performance.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)   
 [CLR 集成的体系结构](./clr-integration-architecture-clr-hosted-environment.md)   
 [从 CLR 数据库对象进行数据访问](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)   
 [CLR 集成入门](../../relational-databases/clr-integration/database-objects/getting-started-with-clr-integration.md)  
  
