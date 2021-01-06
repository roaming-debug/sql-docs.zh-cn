---
title: SqlLocalDB 实用工具
description: SqlLocalDB 实用工具是一款命令行工具，可使用户和开发人员创建和管理 SQL Server Express LocalDB 实例。
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- SqlLocalDB utility [SQL Server]
- local database runtime utility
- LocalDB, SqlLocalDB Utility
ms.assetid: d785cdb7-1ea0-4871-bde9-1ae7881190f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 65646c84e7e0840e2dfeb3ec4e926deb847237fe
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643370"
---
# <a name="sqllocaldb-utility"></a>SqlLocalDB 实用工具
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  SqlLocalDB 实用工具可用于创建 [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssExpCurrent](../includes/ssexpcurrent-md.md)] LocalDB 实例。 SqlLocalDB 实用工具 (SqlLocalDB.exe) 是一款简易命令行工具，可方便用户和开发人员创建和管理 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] LocalDB 实例。 有关如何使用 **LocalDB** 的信息，请参阅 [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md)。  
  
## <a name="syntax"></a>语法  
  
```  
SqlLocalDB.exe   
{  
      [ create   | c ] \<instance-name>  \<instance-version> [-s ]  
    | [ delete   | d ] \<instance-name>  
    | [ start    | s ] \<instance-name>  
    | [ stop     | p ] \<instance-name>  [ -i ] [ -k ]  
    | [ share    | h ] [" <user_SID> " | " <user_account> " ] " \<private-name> " " \<shared-name> "  
    | [ unshare  | u ] " \<shared-name> "  
    | [ info     | i ] \<instance-name>  
    | [ versions | v ]  
    | [ trace    | t ] [ on | off ]  
    | [ help     | -? ]  
}  
```  
  
## <a name="arguments"></a>参数  
 [ **create** | **c** ] *\<instance-name>* *\<instance-version>* [ **-s** ]  
 新建 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** 实例。 SqlLocalDB 使用由 \<instance-version> 参数指定的 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] 版本的二进制文件。 版本号以数字格式指定，至少有一个小数点。 次要版本号 (Service Pack） 是可选的。 例如，下面的两个版本号均可接受：11.0 或 11.0.1186。 必须在计算机上安装指定的版本。 如果未指定，版本号默认为 **SqlLocalDB** 实用工具的版本。 添加 -s 可启动新的 LocalDB 实例。  
  
 [ **共享** | **h** ]  
 使用指定的共享名称共享指定的 **LocalDB** 私有实例。 如果省略该用户 SID 或帐户名称，则默认为当前用户。  
  
 [ **unshared** | **u** ]  
 停止共享指定的 **LocalDB** 共享实例。  
  
 [ **delete** | **d** ] *\<instance-name>*  
 删除指定的 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** 实例。  
  
 [ **start** | **s** ] " *\<instance-name>* "  
 启动指定的 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** 实例。 成功后，该语句返回 **LocalDB** 的命名管道地址。  
  
 [ **stop** | **p** ] *\<instance-name>* [ **-i** ] [ **-k** ]  
 停止指定的 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** 实例。 添加 -i 可请求使用 NOWAIT 选项关闭实例。 添加 -k 可终止实例进程，而无需联系它。  
  
 [ **info** | **i** ] [ *\<instance-name>* ]  
 列出当前用户拥有的所有 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** 实例。  
  
 \<instance-name> 返回指定的 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]LocalDB 实例的名称、版本、状态（正在运行或已停止）、上次启动时间，以及 LocalDB 的本地管道名称。  
  
 [ **trace** | **t** ] **on** | **off**  
 **trace on** 为当前用户启用针对 **SqlLocalDB** API 调用的跟踪。 **trace off** 禁用跟踪。  
  
 **-?**  
 返回每个 **SqlLocalDB** 选项的简短说明。  
  
## <a name="remarks"></a>备注  
 *实例名称* 参数必须遵循 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符规则，或者必须将该参数放入双引号。  
  
 执行不带参数的 SqlLocalDB 将返回帮助文档。  
  
 只能在属于当前已登录用户的实例上执行启动以外的其他操作。 共享的 SQLLOCALDB 实例只能由该实例的所有者启动和停止。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-an-instance-of-localdb"></a>A. 创建 LocalDB 实例  
 下面的示例使用 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**二进制文件创建了一个名为** 的 `DEPARTMENT` LocalDB [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 实例，并启动该实例。  
  
```  
SqlLocalDB.exe create "DEPARTMENT" 12.0 -s  
```  
  
### <a name="b-working-with-a-shared-instance-of-localdb"></a>B. 使用 LocalDB 的共享实例  
 使用管理员权限打开一个命令提示符。  
  
```  
SqlLocalDB.exe create "DeptLocalDB"  
SqlLocalDB.exe share "DeptLocalDB" "DeptSharedLocalDB"  
SqlLocalDB.exe start "DeptLocalDB"  
SqlLocalDB.exe info "DeptLocalDB"  
REM The previous statement outputs the Instance pipe name for the next step  
sqlcmd -S np:\\.\pipe\LOCALDB#<use your pipe name>\tsql\query  
CREATE LOGIN NewLogin WITH PASSWORD = 'Passw0rd!!@52';   
GO  
CREATE USER NewLogin;  
GO  
EXIT  
```  
  
 执行以下代码，使用 **登录名连接到** LocalDB `NewLogin` 的共享实例。  
  
```  
sqlcmd -S (localdb)\.\DeptSharedLocalDB -U NewLogin -P Passw0rd!!@52  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md)  
[命令行管理工具：SqlLocalDB.exe](../relational-databases/express-localdb-instance-apis/command-line-management-tool-sqllocaldb-exe.md)  
