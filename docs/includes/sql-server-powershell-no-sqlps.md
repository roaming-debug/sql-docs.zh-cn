---
title: 在 PowerShell 中为 SQL Server 代理使用 NOSQLPS
description: 消息说明在 SQL Server 代理中使用 sqlserver powershell cmdlet 而不是 sqlps cmdlet
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839396"
---
从 SQL Server 2019 开始，可以禁用 SQLPS。 可以在 PowerShell 类型的作业步骤的第一行添加 `#NOSQLPS`，这将阻止 SQL 代理自动加载 SQLPS 模块。 现在，SQL 代理作业将运行安装在计算机上的 PowerShell 版本，然后你可以使用自己喜欢的任何其他 PowerShell 模块。

要在 SQL 代理作业步骤中使用 [SqlServer](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) 模块，可以将此代码放在脚本的前两行。

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```