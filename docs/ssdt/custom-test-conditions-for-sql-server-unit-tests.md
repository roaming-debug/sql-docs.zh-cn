---
title: SQL Server 单元测试的自定义测试条件
description: 了解如何安装 SQL Server 单元测试的自定义测试条件。 查看安装未创建的测试条件所涉及的风险。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 32a15d61-e908-4ae1-a238-4fd0f988d8c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d7935744497dddce8cb2c9ce75566a9da8e13b42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077729"
---
# <a name="custom-test-conditions-for-sql-server-unit-tests"></a>SQL Server 单元测试的自定义测试条件

可以添加 SQL Server 单元测试的自定义测试条件。 但是，在可以使用测试条件前您必须首先安装它，无论您创建了扩展还是正在安装其他人创建的扩展。  
  
在安装您未创建的测试条件前，应了解以下风险：  
  
-   测试条件的安装程序可能是恶意的，它基于您的安装权限可能获得访问受保护资源的权限。  
  
-   测试条件本身可能是恶意的，如果使用扩展的用户具有足够权限，它可能获得受保护资源的控制权。  
  
要尽可能减小风险，您应仅安装来自已知来源的自定义测试条件。 如果您获得来自不受信任的源的测试条件，应在安装前检查测试条件和安装程序（如果有）的源代码。  
  
要安装自定义测试条件，请将已签名的程序集 (.dll) 复制到 %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions 文件夹。 如果该文件夹不存在，则创建它。 为了复制到此目录，您需要针对您的计算机的管理权限。  
  
> [!NOTE]  
> 在以下情况时，需要安装 Visual Studio 2010 和 Visual Studio 2012 版本的测试条件：  
>   
> -   你在可能用于生成 SQL Server 单元测试的计算机上安装自定义测试条件。  
> -   这些单元测试用于 Visual Studio 2010 和 Visual Studio 2012 中。  
  
有关 SQL Server 单元测试的自定义测试条件的更多信息，请参见：  
  
-   [如何：为 SQL Server 单元测试设计器创建测试条件](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md)  
  
-   [如何：将 Visual Studio 2010 自定义测试条件从早期版本升级到 SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md)  
  
-   [演练：使用自定义测试条件来验证存储过程的结果](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md)  
  
## <a name="see-also"></a>另请参阅  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
  
