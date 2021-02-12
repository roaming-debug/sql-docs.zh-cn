---
description: 'SSMA 控制台中的命令行选项 (AccessToSQL) '
title: SSMA 控制台中的命令行选项 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c1f3b3f0-0f3e-4e07-b745-2fbdde85c67e
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 5e091085616078a93a65922bfaa2176ae4a6b16b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076338"
---
# <a name="command-line-options-in-the-ssma-console-accesstosql"></a>SSMA 控制台中的命令行选项 (AccessToSQL) 
Microsoft 为你提供了一组强大的命令行选项来执行和控制 SSMA 活动。 后续部分提供了更多详细信息。  
  
## <a name="command-line-options-in-the-ssma-console"></a>SSMA 控制台中的命令行选项  
本文介绍了控制台命令选项。  
  
出于本部分的目的，术语 "option" 也称为 "switch"。  
  
选项不区分大小写，并且可以以 " **-** " 或 " **/** " 字符开头。  
  
如果指定了选项，则必须指定相应的选项参数。  
  
选项参数必须与选项字符之间用空格分隔。  
  
**语法示例：**  
  
`C:\> SSMAforAccessConsole.EXE -s scriptfile`  
  
`C:\> SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\VariableValueFileSample.xml" -c "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ServersConnectionFileSample.xml"`  
  
应将包含空格的文件夹或文件名指定为双引号。  
  
命令行项和错误消息的输出存储在 STDOUT 或指定文件中。  
  
### <a name="script-file-option--sscript"></a>脚本文件选项：-s/脚本  
脚本文件路径/名称是必需的开关，指定 SSMA 要执行的命令序列的脚本。  
  
**语法示例：**  
  
`C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="variable-value-file-option--vvariable"></a>变量值文件选项：-v/variable  
变量值文件包含脚本文件中使用的变量。 开关是可选的。 如果变量未在变量文件中声明并在脚本文件中使用，则应用程序将生成错误并终止控制台执行。  
  
**语法示例：**  
  
-   在多个变量值文件中定义的变量，可能有一个具有默认值的变量，另一个是特定于实例的值（如果适用）。 在命令行参数中指定的最后一个变量文件采用首选项，以防出现重复的变量：  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml" -v c:\migration`  
  
    `projects\global_variablevaluefile.xml -v "c:\migrationprojects\instance_variablevaluefile.xml"`  
  
### <a name="server-connection-file-option--cserverconnection"></a>服务器连接文件选项：-c/microsoft.sqlserver.management.common.serverconnection>  
此文件包含每个服务器的服务器连接信息。 每个服务器定义由唯一的服务器 ID 标识。 脚本文件中引用了与连接相关的命令的服务器 Id。  
  
服务器定义可以是服务器连接文件和/或脚本文件的一部分。 脚本文件中的服务器 id 优先于服务器连接文件，以防出现服务器 id 重复。  
  
**语法示例：**  
  
-   服务器 Id 在脚本文件中使用。 它们在单独的服务器连接文件中定义。 此文件使用变量值文件中定义的变量：  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -v`  
  
    `c:\SsmaProjects\myvaluefile1.xml -c`  
  
    `c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
-   服务器定义嵌入到脚本文件中：  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>XML output 选项：-x/xmloutput [xmloutputfile]  
此命令用于将 xml 格式的命令输出消息输出到控制台或 xml 文件。  
  
Xmloutput 提供了两个选项，即：  
  
-   如果在 xmloutput 开关之后提供了 filepath，则会将输出重定向到文件。  
  
    **语法示例：**  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -x d:\xmloutput\project1output.xml`  
  
-   如果在 xmloutput 开关之后未提供 filepath，则 xmlout 将显示在控制台上。  
  
    **语法示例：**  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -xmloutput`  
  
### <a name="log-file-option--llog"></a>日志文件选项：-l/日志  
控制台应用程序中的所有 SSMA 操作都记录在一个日志文件中，并且开关是可选的。 如果在命令行中指定了日志文件及其路径，则会在指定的位置生成日志。 否则，它将在其默认位置生成。  
  
**语法示例：**  
  
`C:\>SSMAforAccessConsole.EXE`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option--eprojectenvironment"></a>项目环境文件夹选项：-e/projectenvironment  
此可选开关表示当前 SSMA 项目的项目环境设置文件夹。  
  
**语法示例：**  
  
`C:\>SSMAforAccessConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option--psecurepassword"></a>安全密码选项：-p/securepassword  
此选项指示服务器连接的加密密码。 它与所有其他选项的不同之处在于，它不会执行任何与迁移相关的活动中的任何脚本或帮助，但有助于管理迁移项目中使用的服务器连接的密码加密。  
  
不能输入任何其他选项或密码作为命令行参数。 否则，会导致错误。 有关详细信息，请参阅 [管理密码](managing-passwords-accesstosql.md) 部分。  
  
支持以下子选项 `-p/securepassword` ：  
  
-   若要为指定的服务器 ID 或服务器连接文件中定义的所有服务器 Id 添加密码或更新现有密码，请执行以下操作：  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-s|-script <server-connection-file> [-v|variable <variable-value-file>] [-o|overwrite]`  
  
-   若要从指定服务器 ID 或所有服务器 Id 的受保护存储中删除加密密码：  
  
    `-p/securepassword -r/remove {<server_id> [, ...n] | all}`  
  
-   若要显示为其加密密码的服务器 Id 的列表，请执行以下操作：  
  
    `-p/securepassword -l/list`  
  
-   将存储在受保护的存储中的密码导出到加密文件中。 此文件是用用户指定的密码加密的。  
  
    `-p/securepassword -e/export {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
-   使用用户指定的密码短语，先前导出的加密文件将导入到本地受保护的存储中。 文件解密后，它将存储在新文件中，后者将在本地计算机上加密。  
  
    `-p/securepassword -i/import {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
    可以使用逗号分隔符指定多个服务器 Id。  
  
### <a name="help-option--help"></a>帮助选项：-？/Help  
显示 SSMA 控制台选项的语法摘要：  
  
`C:\>SSMAforAccessConsole.EXE -?`  
  
如要表格显示 SSMA 控制台命令行选项，请参阅 [附录 1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md)。  
  
### <a name="securepassword-help-option--securepassword--help"></a>SecurePassword 帮助选项：-SecurePassword-？/Help  
显示 SSMA 控制台选项的语法摘要：  
  
`C:\>SSMAforAccessConsole.EXE -securepassword -?`  
  
如要表格显示 SSMA 控制台命令行选项，请参阅 [附录-1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md)  
  
### <a name="next-steps"></a>后续步骤  
下一步取决于项目要求：  
  
1.  若要指定密码或导出/导入密码，请参阅 [管理密码 &#40;AccessToSQL&#41;](../../ssma/access/managing-passwords-accesstosql.md)。  
  
2.  有关生成报表的详细 &#40;，请参阅 [&#41;中生成报表 ](../../ssma/access/generating-reports-accesstosql.md)。  
  
3.  有关控制台中问题的疑难解答，请参阅 [排查 &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md)。  
  
