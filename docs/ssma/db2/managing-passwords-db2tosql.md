---
description: " (DB2ToSQL) 管理密码"
title: " (DB2ToSQL) 管理密码 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 07/05/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 56d546e3-8747-4169-aace-693302667e94
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3415515af0c29e0d1cca1ddbfab89859d26c83fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014588"
---
# <a name="managing-passwords-db2tosql"></a> (DB2ToSQL) 管理密码
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
  
        -   在 `SSMAforDB2Console.exe` `-securepassword` 命令行上执行，并在服务器定义部分传递包含 password 节点的服务器连接或脚本文件，并在命令行上执行。  
  
        -   在提示符下，要求用户输入数据库密码并进行确认。  
  
            服务器定义 id 及其相应的加密密码存储在本地计算机上的文件中  
            
            示例 1：
            
            ```console
            Specify password
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml"
            
            Enter password for server_id 'XXX_1': xxxxxxx
            
            Re-enter password for server_id 'XXX_1': xxxxxxx
            ```
            
            示例 2：
            
            ```console
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml" -o
            
            Enter password for server_id 'source_1': xxxxxxx
            
            Re-enter password for server_id 'source_1': xxxxxxx
            
            Enter password for server_id 'target_1': xxxxxxx
            
            Re-enter password for server_id 'target _1': xxxxxxx  
            ```
    
    -   **删除加密的密码**  
  
        在 `SSMAforDB2Console.exe` `-securepassword` `-remove` 命令行传递服务器 id 的情况下执行，并从本地计算机上的受保护存储文件中删除加密的密码。  
  
        例如：  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove all
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove "source_1,target_1"
        ```

    -   **列出其密码已加密的服务器 Id**  
  
        在 `SSMAforDB2Console.exe` `-securepassword` 命令行中使用和开关执行， `-list` 以列出其密码已加密的所有服务器 id。  
  
        例如：  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -list
        ```

    > [!NOTE]  
    > 1.  脚本或服务器连接文件中提及的明文密码优先于安全文件中的加密密码。  
    > 2.  如果服务器连接文件或脚本文件的服务器部分中不存在密码，或者本地计算机上未保护密码，控制台将提示你输入密码。  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>导出或导入加密的密码  
SSMA 控制台应用程序允许将本地计算机上的文件中存在的加密数据库密码导出到受保护的文件，反之亦然。 它有助于独立加密密码计算机。

_导出功能_ 从本地受保护的存储读取服务器 id 和密码。 然后，系统会将 id 和密码保存在加密文件中。 系统将提示用户输入安全文件的密码。 请确保输入的密码长度为8个或更多个字符。 此安全文件可在不同的计算机之间移植。

_导入功能_ 从安全文件中读取服务器 id 和密码信息。 系统将提示用户输入安全文件的密码，并将该信息附加到本地受保护的存储中。  

### <a name="export-example"></a>导出示例

1. 导出密码。

2. 输入用于保护导出的文件的密码。

3. 运行： &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -securepassword -export all "machine1passwords.file"`

4. 输入用于保护导出文件的密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

5. 确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

6. 运行： &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -p -e "DB2DB_1_1,Sql_1" "machine2passwords.file"`

7. 输入用于保护导出文件的密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

8. 确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  

### <a name="import-example"></a>导入示例

1. 导入加密密码。

2. 输入用于保护导入的文件的密码。

3. 运行： &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -securepassword -import all "machine1passwords.file"`

4. 输入密码以从加密文件中导入服务器： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

5. 确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

6. 运行： &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -p -i "DB2DB_1,Sql_1" "machine2passwords.file"`

7. 输入密码以从加密文件中导入服务器： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

8. 确认密码： xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="see-also"></a>另请参阅  
[执行 SSMA 控制台](./executing-the-ssma-console-db2tosql.md)  
