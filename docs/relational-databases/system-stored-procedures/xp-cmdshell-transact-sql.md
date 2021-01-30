---
description: xp_cmdshell (Transact-SQL)
title: xp_cmdshell (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 12/01/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_cmdshell
- xp_cmdshell_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: 18935cf4-b320-4954-b6c1-e007fcefe358
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73bea06c7919c40b1080458f3e6982d2c10ebe47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124999"
---
# <a name="xp_cmdshell-transact-sql"></a>xp_cmdshell (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  生成 Windows 命令 shell 并以字符串的形式传递以便执行。 任何输出都作为文本的行返回。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_cmdshell { 'command_string' } [ , no_output ]  
```  
  
## <a name="arguments"></a>参数  
 **"** *command_string* **"**  
 包含要传递到操作系统的命令的字符串。 *command_string* 是 **varchar (8000)** 或 **nvarchar (4000)**，无默认值。 *command_string* 不能包含一组以上的双引号。 如果在 *command_string* 中引用的文件路径或程序名称中有空格，则需要使用一对引号。 如果不方便使用内含的空格，则可考虑使用 FAT 8.3 文件名作为解决方法。  
  
 **no_output**  
 可选参数，指定不应向客户端返回任何输出。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 执行以下 `xp_cmdshell` 语句将返回当前目录的目录列表。  
  
```  
EXEC xp_cmdshell 'dir *.exe';  
GO  
```  
  
 行在 **nvarchar (255)** 列中返回。 如果使用 **no_output** 选项，则仅返回以下内容：  
  
```  
The command(s) completed successfully.  
```  
  
## <a name="remarks"></a>备注  
 **Xp_cmdshell** 生成的 Windows 进程与服务帐户具有相同的安全权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
 
> [!IMPORTANT]
>  **xp_cmdshell** 是一项功能强大的功能，默认情况下已禁用。 可以使用基于策略的管理或通过执行 **sp_configure** 来启用和禁用 **xp_cmdshell** 。 有关详细信息，请参阅 [外围应用配置](../../relational-databases/security/surface-area-configuration.md) 和 [Xp_cmdshell 服务器配置选项](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)。  
  
 **xp_cmdshell** 同步操作。 在命令 shell 命令执行完毕之前，不会将控制权返回给调用方。 
 
> [!IMPORTANT]
>  如果 **xp_cmdshell** 在批处理中执行并返回错误，则批处理将失败。 这是行为的更改。 在早期版本的中， [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 批处理将继续执行。  
 
## <a name="xp_cmdshell-proxy-account"></a>xp_cmdshell 代理帐户  
 如果由不是 **sysadmin** 固定服务器角色成员的用户调用， **xp_cmdshell** 将使用名为 **# #xp_cmdshell_proxy_account # #** 的凭据中存储的帐户名和密码连接到 Windows。 如果此代理凭据不存在， **xp_cmdshell** 将会失败。  
  
 可以通过执行 **sp_xp_cmdshell_proxy_account** 来创建代理帐户凭据。 此存储过程将 Windows 用户名和密码作为参数使用。 例如，以下命令为具有 Windows 密码 `SHIPPING\KobeR` 的 Windows 域用户 `sdfh%dkc93vcMt0` 创建代理凭据。  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'SHIPPING\KobeR','sdfh%dkc93vcMt0';  
```  
  
 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_xp_cmdshell_proxy_account ](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 由于恶意用户有时尝试使用 **xp_cmdshell** 提升其权限，因此默认情况下 **xp_cmdshell** 处于禁用状态。 使用 **sp_configure** 或 **基于策略的管理** 来启用它。 有关详细信息，请参阅 [xp_cmdshell 服务器配置选项](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)。  
  
 首次启用时， **xp_cmdshell** 需要 CONTROL SERVER 权限才能执行，而 **Xp_cmdshell** 创建的 Windows 进程与服务帐户具有相同的安全上下文 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]通常，对于由 **xp_cmdshell** 创建的进程所执行的工作而言，服务帐户具有的权限更多。 若要增强安全性，应将对 **xp_cmdshell** 的访问权限限制为具有高权限的用户。  
  
 若要允许非管理员使用 **xp_cmdshell**，并允许使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 低特权帐户的安全令牌创建子进程，请执行以下步骤：  
  
1.  使用进程要求的最小特权创建并自定义一个 Windows 本地用户帐户或一个域帐户。  
  
2.  使用 **sp_xp_cmdshell_proxy_account** 系统过程将 **xp_cmdshell** 配置为使用该最小特权帐户。  
  
    > [!NOTE]  
    >  你还可以使用来配置此代理帐户 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，方法是在对象资源管理器中右键单击服务器名称上的 "**属性**"，然后在 "**服务器代理帐户**" 部分的 "**安全**" 选项卡上查找。  
  
3.  在中 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ，使用 master 数据库执行语句，为 `GRANT exec ON xp_cmdshell TO N'<some_user>';` 特定的非 **sysadmin** 用户授予执行 **xp_cmdshell** 的能力。 指定的用户必须存在于 master 数据库中。  
  
 现在，非管理员可以使用 **xp_cmdshell** 启动操作系统进程，并且这些进程将以您配置的代理帐户的权限运行。 具有 CONTROL SERVER 权限的用户 (**sysadmin** 固定服务器角色的成员) 将继续接收 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 由 **xp_cmdshell** 启动的子进程的服务帐户的权限。  
  
 若要确定在启动操作系统进程时 **xp_cmdshell** 使用的 Windows 帐户，请执行以下语句：  
  
```  
xp_cmdshell 'whoami.exe'  
  
```  
  
 若要确定其他登录名的安全上下文，请执行以下语句：  
  
```  
EXECUTE AS LOGIN = '<other_login>' ;  
GO  
xp_cmdshell 'whoami.exe' ;  
REVERT ;  
  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-a-list-of-executable-files"></a>A. 返回可执行文件列表  
 以下示例显示执行目录命令的 `xp_cmdshell` 扩展存储过程。  
  
```  
EXEC master..xp_cmdshell 'dir *.exe'  
```  
  
### <a name="b-returning-no-output"></a>B. 不返回任何输出  
 以下示例使用 `xp_cmdshell` 执行命令字符串，且不向客户端返回输出。  
  
```  
USE master;  
  
EXEC xp_cmdshell 'copy c:\SQLbcks\AdvWorks.bck  
    \\server2\backups\SQLbcks', NO_OUTPUT;  
GO  
```  
  
### <a name="c-using-return-status"></a>C. 使用返回状态  
 在下面的示例中， `xp_cmdshell` 扩展存储过程还建议返回状态。 返回代码值存储在变量 `@result` 中。  
  
```  
DECLARE @result int;  
EXEC @result = xp_cmdshell 'dir *.exe';  
IF (@result = 0)  
   PRINT 'Success'  
ELSE  
   PRINT 'Failure';  
```  
  
### <a name="d-writing-variable-contents-to-a-file"></a>D. 将变量内容写入文件中  
 以下示例将 `@var` 变量的内容写入当前服务器目录下名为 `var_out.txt` 的文件中。  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'Hello world';  
SET @cmd = 'echo ' + @var + ' > var_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
### <a name="e-capturing-the-result-of-a-command-to-a-file"></a>E. 将命令的结果捕获到文件中  
 以下示例将当前目录的内容写入当前服务器目录下名为 `dir_out.txt` 的文件中。  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'dir/p';  
SET @cmd = @var + ' > dir_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_cmdshell 服务器配置选项](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)   
 [外围应用配置器](../../relational-databases/security/surface-area-configuration.md)   
 [sp_xp_cmdshell_proxy_account &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)  
  
  
