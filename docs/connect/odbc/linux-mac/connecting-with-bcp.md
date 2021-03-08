---
title: 使用 bcp 连接
description: 了解如何在 Linux 和 macOS 上结合使用 bcp 实用工具与 Microsoft ODBC Driver for SQL Server。
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- bcp
ms.assetid: 3eca5717-e50f-40db-be16-a1cebbdfee70
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f8bff2d5d9892d709885d0888e36ca042aa72242
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837391"
---
# <a name="connecting-with-bcp"></a>使用 bcp 连接
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

可以在 Linux 和 macOS 上结合使用 [bcp](../../../tools/bcp-utility.md) 与 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 本页介绍与 `bcp` 的 Windows 版本之间的区别。
  
- 字段终止符是制表符 ("\t")。  
  
- 行终止符是换行符 ("\n")。  
  
- 字符模式是不包含扩展字符的 `bcp` 格式化文件和数据文件的首选格式。  
  
> [!NOTE]  
> 命令行参数上的反斜杠“\\”必须带引号或经过转义。 例如，若要将某个换行符指定为自定义行终止符，必须使用以下某一机制：  
>   
> -   -r\\\n  
> -   -r"\n"  
> -   -r'\n'  
  
以下示例是将表行复制到文本文件的 `bcp` 命令调用：  
  
```  
bcp AdventureWorks2008R2.Person.Address out test.dat -Usa -Pxxxx -Sxxx.xxx.xxx.xxx  
```  
  
## <a name="available-options"></a>可用选项
在当前版本中，可以使用以下语法和选项：  

[_database_ **.** ]_schema_ **.** _table_ **in** _data\_file_ | **out** _data\_file_

- -a *packet_size*  
指定服务器发出或接收的每个网络数据包的字节数。  
  
- -b *batch_size*  
指定每批导入数据的行数。  
  
- -c  
使用字符数据类型。  
  
- -d *database_name*  
指定要连接到的数据库。  
  
- -d  
使值传递给将解释为数据源名称 (DSN) 的 `bcp` -S 选项。 有关详细信息，请参阅[使用 sqlcmd 进行连接](connecting-with-sqlcmd.md)中的“sqlcmd 和 bcp 中的 DSN 支持”。  
  
- -e error_file 指定错误文件的完整路径，此文件用于存储 `bcp` 实用工具无法从文件传输到数据库的所有行  。  
  
- -E  
将导入数据文件中的一个或多个标识值用于标识列。  
  
- -f *format_file*  
指定格式化文件的完整路径。  
  
- -F *first_row*  
指定要从表中导出或从数据文件导入的第一行的编号。  

- -G  
当连接到 Azure SQL 数据库或 Azure Synapse Analytics 时，客户端将使用此开关指定该用户使用 Azure Active Directory 身份验证来进行身份验证。 -G 开关至少需要 bcp 版本 17.6。 要确定你的版本，请执行 bcp -v。

> [!IMPORTANT]
> `-G` 选项仅适用于 Azure SQL 数据库和 Azure Synapse Analytics。
> Linux 或 macOS 目前不支持 AAD 交互式身份验证。 AAD 集成身份验证要求 [Microsoft ODBC Driver 17 for SQL Server](../download-odbc-driver-for-sql-server.md) 版本 17.6.1 或更高版本以及正确[配置的 Kerberos 环境](using-integrated-authentication.md#configure-kerberos)。

- -k  
指定在操作过程中空列应该保留 null 值，而不是所插入列的任何默认值。  
  
- -l  
指定登录超时。 -l 选项指定在尝试连接到服务器时登录 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的超时时间（以秒为单位）。 默认登录超时值为 15 秒。 登录超时必须是介于 0 和 65534 之间的数字。 如果提供的值不是数值或不在此范围内，则 `bcp` 将生成错误消息。 值 0 指定无限超时。
  
- -L *last_row*  
指定要从表中导出或从数据文件中导入的最后一行的编号。  
  
- -m *max_errors*  
指定可能出现的语法错误的上限，超过此上限即导致 `bcp` 操作取消。  
  
- -n  
使用数据的本机（数据库）数据类型执行大容量复制操作。  
  
- -P *password*  
指定登录 ID 的密码。  
  
- -S  
在 `bcp` 实用工具和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例之间的连接中，执行 SET QUOTED_IDENTIFIERS ON 语句。  
  
- -r *row_terminator*  
指定行终止符。  
  
- -R  
指定使用客户端计算机区域设置中定义的区域格式，将货币、日期和时间数据大容量复制到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中。  
  
- -S *server*  
指定要连接的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称，或者如果使用 -D，则指定 DSN。  
  
- -t *field_terminator*  
指定字段终止符。  
  
- -T  
指定 `bcp` 实用工具通过信任连接（集成安全性）连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
- -U *login_id*  
指定用于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的登录 ID。  
  
- -v  
报告 `bcp` 实用工具的版本号和版权。  
  
- -w  
使用 Unicode 字符执行大容量复制操作。  
  
在此版本中，支持 Latin-1 和 UTF-16 字符。  
  
## <a name="unavailable-options"></a>不可用选项
在当前版本中，以下语法和选项不可用：  

- -C  
指定该数据文件中数据的代码页。  
  
- -h *hint*  
指定向表或视图大容量导入数据时所使用的一个或多个提示。  
  
- -i *input_file*  
指定响应文件的名称。  
  
- -N  
对非字符数据使用数据的本机（数据库）数据类型，对字符数据使用 Unicode 字符。  
  
- -o *output_file*  
指定文件名称，该文件用于接收从命令提示符重定向来的输出。  
  
- -V (80 | 90 | 100)  
使用早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的数据类型。  
  
- -X  
结合使用该格式和 -f format_file 选项一起使用，可生成基于 XML 的格式化文件，而不是默认的非 XML 格式化文件。  
  
## <a name="see-also"></a>另请参阅

[使用 sqlcmd 进行连接  ](connecting-with-sqlcmd.md)  
[发行说明](release-notes-tools.md)