---
title: 启用了外部脚本的服务器配置选项
description: 了解 SQL Server 中的“已启用外部脚本”选项。 启用后，可以执行用支持的语言（如 R 或 Python）编写的外部脚本。
ms.date: 06/30/2020
ms.prod: sql
ms.technology: machine-learning-services
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- external scripts enabled
- external_scripts_enabled_TSQL
helpviewer_keywords:
- external scripts enabled option
ms.assetid: 9d0ce165-8719-4007-9ae8-00f85cab3a0d
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 8ebdef5d75c430ec5ba208613b72771939fdfc9b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481478"
---
# <a name="external-scripts-enabled-server-configuration-option"></a>启用了外部脚本的服务器配置选项
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

使用 **external scripts enabled** 选项启用包含某些远程语言扩展的脚本执行。 此属性默认处于禁用状态。 安装“机器学习服务”后，安装程序可以根据需要将此属性设置为 true。

## <a name="remarks"></a>备注

在使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 过程执行外部脚本之前，必须启用“已启用外部脚本”选项。 使用 sp_execute_external_script 执行以受支持的语言（如 R 或 Python）编写的脚本。 

+ 对于 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]

    [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 支持 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中的 R 语言，以及一组 R 工作站工具和连接库。

    在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中安装“R 服务”功能，以允许执行 R 脚本。

+ 适用于 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 和更高版本

    [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] 支持 R 和 Python 语言。

    在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中安装“机器学习服务”功能，以允许执行外部脚本。 确保在初始安装过程中至少选择一种语言：R 和/或 Python。
    
+ 适用于 [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] 和更高版本 [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]支持所有 R、Python、Java 和其他第三方语言。

在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的过程中安装机器学习服务和语言扩展功能，以允许执行任何受支持语言的外部脚本。

## <a name="additional-requirements"></a>其他需求

安装后，要启用外部脚本，请执行下面的脚本：

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

有关详细信息，请参阅[在 Windows 上](../../machine-learning/install/sql-machine-learning-services-windows-install.md)或[在 Linux 上安装 SQL Server 机器学习服务（Python 和 R）](../../linux/sql-server-linux-setup-machine-learning-docker.md?toc=/sql/machine-learning/toc.json)。

## <a name="see-also"></a>另请参阅

+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
+ [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)
+ [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [SQL 机器学习文档](../../machine-learning/index.yml)
