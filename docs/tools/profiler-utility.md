---
title: Profiler 实用工具
description: Profiler 实用工具可启动 SQL Server Profiler 工具。 利用可选参数，可以控制应用程序的启动方式。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab40ea190dab072d7415ded1f399cfa4a4b78568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335940"
---
# <a name="profiler-utility"></a>Profiler 实用工具
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  **Profiler** 实用工具可以启动 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 工具。 利用此主题后面列出的可选参数，可以控制应用程序的启动方式。  
  
> [!NOTE]  
>  **Profiler** 实用工具不适用于脚本跟踪。 有关详细信息，请参阅 [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>参数  
 **/?**  
 显示 **Profiler** 自变量的语法摘要。  
  
 **/U** *login_id*  
 用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证的用户登录 ID。 登录 ID 区分大小写。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].  
  
 **/P** *password*  
 指定用户指定的用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证的密码。  
  
 **/E**  
 指定使用当前用户的凭据与 Windows 身份验证连接。  
  
 **/S**  *sql_server_name*  
 指定一个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例。 Profiler 会使用在 **/U** 和 **/P** 开关或 **/E** 开关中指定的身份验证信息，自动连接到指定的服务器。 若要连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的命名实例，请使用 **/S** *sql_server_name*\\*instance_name*。  
  
 **/A**  *analysis_services_server_name*  
 指定一个 Analysis Services 实例。 Profiler 会使用在 **/U** 和 **/P** 开关或 **/E** 开关中指定的身份验证信息，自动连接到指定的服务器。 若要连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的命名实例，请使用 **/A** *analysis_services_server_name\instance_name*。  
  
 **/D** *database*  
 指定要用于连接的数据库的名称。 如果不指定任何数据库，该选项将为指定的用户选择默认数据库。  
  
 **/B "** *trace_table_name* **"**  
 指定要在事件探查器启动时加载的跟踪表。 必须指定数据库、用户或架构以及表。  
  
 **/T"** *template_name* **"**  
 指定要加载以便配置跟踪的模板。 模板名称必须括在引号中。 模板名称必须位于系统模板目录或用户模板目录下。 如果这两个目录中同时存在同名的两个模板，则将加载系统目录下的模板。 如果不存在具有指定名称的模板，则将加载标准模板。 请注意，模板的文件扩展名 (.tdf) 不能指定为 *template_name* 的一部分。 例如：  
  
```  
/T "standard"  
```  
  
 **/F"** *filename* **"**  
 指定要在事件探查器启动时加载的跟踪文件的路径和文件名。 整个路径和文件名必须置于引号中。 此选项不能与 **/O** 一起使用。  
  
 **/O "** *filename*  **"**  
 指定应在其中写入跟踪结果的文件的路径和文件名。 整个路径和文件名必须置于引号中。 此选项不能与 **/F.** 一起使用。  
  
 **/L** *locale_ID*  
 不可用。  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 指定跟踪停止的日期和时间。 停止时间必须括在引号中。 根据下表中的参数指定停止时间：  
  
|参数|定义|  
|---------------|----------------|  
|MM|两位数月份|  
|DD|两位数日期|  
|YY|两位数年份|  
|hh|24 小时制的两位数小时|  
|mm|两位数分钟|  
|ss|两位数秒数|  
  
> [!NOTE]  
>  仅当在 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 中启用了“使用区域设置来显示日期和时间值”选项时，才能使用“YY-MM-DD- hh:mm:ss”格式。 如果未启用此选项，则必须使用“YYYY-MM-DD hh:mm:ss”日期和时间格式。  
  
 **/R**  
 启用跟踪文件滚动。  
  
 **/Z**  *file_size*  
 指定跟踪文件的大小（以兆字节 (MB) 为单位）。 默认大小是 5 MB。 若启用了滚动功能，则所有滚动文件的大小将被限制为此参数所指定的值。  
  
## <a name="remarks"></a>备注  
 若要使用特定的模板启动跟踪，请同时使用 **/S** 和 **/T** 选项。 例如，若要使用 MyServer\MyInstance 上的 Standard 模板启动跟踪，请在命令提示符处输入以下内容：  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>另请参阅  
 [命令提示实用工具参考（数据库引擎）](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
