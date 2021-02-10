---
description: " (AccessToSQL) 管理密码"
title: " (AccessToSQL) 管理密码 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 07/01/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: b099d0f9-dd37-4c87-8b6f-ed0177881ea4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1d6fa64888898221ced921a2306278b7c4503fc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058912"
---
# <a name="managing-passwords-accesstosql"></a> (AccessToSQL) 管理密码
本部分介绍如何保护数据库密码，以及如何在服务器之间导入或导出数据库密码：  
  
1.  保护密码  
  
2.  导出或导入加密的密码  
  
## <a name="securing-password"></a>保护密码  
SSMA 允许保护数据库的密码。  
  
使用以下过程来实现安全连接：  
  
使用以下三种方法之一指定有效密码：  
  
1.  **清除文本：** 在 "密码" 节点的 "值" 属性中键入数据库密码。 它在脚本文件或服务器连接文件的服务器部分中的 "服务器定义" 节点下找到。  
  
    明文形式的密码不安全。 因此，您将在控制台输出中看到以下警告消息： *"服务器 &lt; 服务器 id &gt; 密码以非安全明文形式提供"，SSMA 控制台应用程序提供了通过加密来保护密码的选项，有关详细信息，请参阅 securepassword 选项。*  
  
    **加密密码：** 在此示例中，指定的密码以加密形式存储在 ProtectedStorage. ssma 中的本地计算机上。  
  
    -   **保护密码**  
  
        -   在 `SSMAforAccessConsole.exe` `-securepassword` 命令行上执行，并在服务器定义部分传递包含 password 节点的服务器连接或脚本文件，并在命令行上执行。  
  
        -   在提示符下，要求用户输入数据库密码并进行确认。  
  
            服务器定义 id 及其相应的加密密码存储在本地计算机上的文件中  

            &nbsp;

            _示例 1：_
            
            指定密码

            ```console
            C:\SSMA\SSMAforAccessConsole.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ VariableValueFileSample.xml"
            ```

            输入 server_id "XXX_1" 的密码： xxxxxxx-xxxx .。。
                
            重新输入 server_id "XXX_1" 的密码： xxxxxxx-xxxx .。。  

            &nbsp;

            _示例 2：_

            ```console
            C:\SSMA\SSMAforAccessConsole.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ VariableValueFileSample.xml" -o
            ```

            输入 server_id "source_1" 的密码： xxxxxxx-xxxx .。。
                
            重新输入 server_id "source_1" 的密码： xxxxxxx-xxxx .。。
                
            输入 server_id "target_1" 的密码： xxxxxxx-xxxx .。。
                
            重新输入 server_id "目标 _1" 的密码： xxxxxxx-xxxx .。。  
  
    -   **删除加密的密码**  
  
        在 `SSMAforAccessConsole.exe` `-securepassword` `-remove` 命令行传递服务器 id 的情况下执行，并从本地计算机上的受保护存储文件中删除加密的密码。  

        ```console
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -remove all
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -remove "source_1,target_1"
        ```
  
    -   **列出其密码已加密的服务器 Id**  
  
        在命令行中执行 SSMAforAccessConsole.exe， `-securepassword` 并 `-list` 在命令行中列出其密码已加密的所有服务器 id。  

        ```console
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -list
        ```
  
    > [!NOTE]  
    > 1.  脚本或服务器连接文件中提及的明文密码优先于安全文件中的加密密码。  
    > 2.  如果服务器连接文件或脚本文件的服务器部分中不存在密码，或者本地计算机上未保护密码，控制台将提示你输入密码。  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>导出或导入加密的密码  
SSMA 控制台应用程序允许将本地计算机上的文件中存在的加密数据库密码导出到受保护的文件，反之亦然。 它有助于独立加密密码计算机。 导出功能从本地受保护的存储读取服务器 id 和密码，并将信息保存在加密文件中。 系统将提示用户输入安全文件的密码。 请确保输入的密码长度为8个字符或更长。 此安全文件可在不同的计算机之间移植。 导入功能从安全文件中读取服务器 id 和密码信息。 系统将提示用户输入安全文件的密码，并将该信息附加到本地受保护的存储中。  

### <a name="export-password"></a>导出密码

1. 输入用于保护导出的文件的密码

2. `C:\SSMA\SSMAforAccessConsole.EXE -securepassword -export all "machine1passwords.file"`

3. 输入用于保护导出文件的密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

4. 请确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

5. `C:\SSMA\SSMAforAccessConsole.EXE -p -e "AccessDB_1_1,Sql_1" "machine2passwords.file"`

6. 输入用于保护导出文件的密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

7. 请确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  

### <a name="import-an-encrypted-password"></a>导入加密密码

1. 输入用于保护导入文件的密码

2. `C:\SSMA\SSMAforAccessConsole.EXE -securepassword -import all "machine1passwords.file"`

3. 输入密码以从加密文件导入服务器： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

4. 请确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

5. `C:\SSMA\SSMAforAccessConsole.EXE -p -i "AccessDB_1,Sql_1" "machine2passwords.file"`

6. 输入密码以从加密文件导入服务器： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

7. 请确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  

## <a name="see-also"></a>另请参阅  
[执行 SSMA 控制台 (访问) ](./executing-the-ssma-console-accesstosql.md)  
