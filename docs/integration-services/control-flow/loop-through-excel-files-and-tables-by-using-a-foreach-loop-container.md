---
description: 使用 Foreach 循环容器循环遍历 Excel 文件和表
title: 使用 Foreach 循环容器循环遍历 Excel 文件和表 | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- connections [Integration Services], Excel
- Excel [Integration Services]
- connection managers [Integration Services], Excel
ms.assetid: a5393c1a-cc37-491a-a260-7aad84dbff68
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c6e986f032f755f73db249f7ddeff539fca4a8c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197167"
---
# <a name="loop-through-excel-files-and-tables-with-a-foreach-loop-container"></a>使用 Foreach 循环容器循环遍历 Excel 文件和表

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本主题中的过程介绍如何使用具有相应枚举器的 Foreach 循环容器循环访问文件夹中的 Excel 工作簿或 Excel 工作簿中的表。  

> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)。
 
## <a name="to-loop-through-excel-files-by-using-the-foreach-file-enumerator"></a>使用 Foreach 文件枚举器循环遍历 Excel 文件  
  
1.  创建一个将在每次循环迭代中接收当前 Excel 路径和文件名的字符串变量。 若要避免验证问题，请分配有效的 Excel 路径和文件名作为该变量的初始值。 （本过程后面显示的示例表达式将使用变量名 `ExcelFile`。）  
  
2.  （可选）创建另一个字符串变量，用于存放 Excel 连接字符串的扩展属性参数的值。 此参数包含一系列值，这些值指定 Excel 版本并确定第一行是否包含列名称，以及是否使用导入模式。 （此过程随后显示的示例表达式将使用变量名 `ExtProperties`，其初始值为“`Excel 12.0;HDR=Yes`”。）  
  
     如果您未使用扩展属性参数的变量，必须手动将它添加到包含连接字符串的表达式。  
  
3.  将 Foreach 循环容器添加到 **“控制流”** 选项卡。有关如何配置 Foreach 循环容器的信息，请参阅 [配置 Foreach 循环容器](./foreach-loop-container.md)。  
  
4.  在“Foreach 循环编辑器”的“集合”页上，选择“Foreach 文件”枚举器，并指定 Excel 工作簿所在的文件夹，然后指定文件筛选器（通常是 *.xlsx）。  
  
5.  在“变量映射”页中，将索引 0 映射到用户定义字符串变量，该变量将在每个循环迭代中接收当前 Excel 路径和文件名。 （本过程后面显示的示例表达式将使用变量名 `ExcelFile`。）  
  
6.  关闭 **“Foreach 循环编辑器”**。  
  
7.  按照 [在包中添加、删除或共享连接管理器](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))中所述，将 Excel 连接管理器添加到包。 为连接选择一个现有 Excel 工作簿文件以避免出现验证错误。  
  
    > [!IMPORTANT]  
    >  若要避免在对使用此 Excel 连接管理器的任务和数据流组件进行配置时出现验证错误，请在 **“Excel 连接管理器编辑器”** 中选择一个现有的 Excel 工作簿。 在您按照下列步骤为 **ConnectionString** 属性配置表达式以后，连接管理器在运行时将不使用此工作簿。 在创建并配置包后，可在“属性”窗口中清除 **ConnectionString** 属性的值。 但是，如果清除此值，要等到 Foreach 循环运行时 Excel 连接管理器的连接字符串属性才会有效。 因此，在使用了连接管理器的任务中，必须将 **DelayValidation** 属性设置为 **True** 以避免出现验证错误。  
    >   
    >  还必须使用 Excel 连接管理器的 **False** 属性的默认值 **RetainSameConnection** 。 如果将此值更改为 **True**，该循环的每次迭代将继续打开第一个 Excel 工作簿。  
  
8.  选择新的 Excel 连接管理器，并在“属性”窗口中单击 **“表达式”** 属性，然后单击省略号。  
  
9. 在 **“属性表达式编辑器”** 中，选择 **ConnectionString** 属性，并单击省略号。  
  
10. 在表达式生成器中，输入以下表达式：  
  
    ```  
    "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=" +  @[User::ExcelFile] + ";Extended Properties=\"" + @[User::ExtProperties] + "\""  
    ```  
  
     注意使用转义符“\\”来转义扩展属性参数的值前后所需的内部引号。  
  
     扩展属性参数不是可选的。 如果未使用变量来包含其值，则必须手动将它添加到表达式中，如以下示例所示：  
  
    ```  
    "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=" +  @[User::ExcelFile] + ";Extended Properties=Excel 12.0"  
    ```  
  
11. 在 Foreach 循环容器中创建任务，这些任务使用 Excel 连接管理器来在每个与指定的文件位置和模式匹配的 Excel 工作簿上执行相同的操作。  
  
## <a name="to-loop-through-excel-tables-by-using-the-foreach-adonet-schema-rowset-enumerator"></a>使用 Foreach ADO.NET 架构行集枚举器循环遍历 Excel 表  
  
1.  创建使用 Microsoft ACE OLE DB 访问接口连接 Excel 工作簿的 ADO.NET 连接管理器。 在“连接管理器”对话框的“所有”页上，确保输入 Excel 版本，在本例中，Excel 12.0 作为“Extended Properties”属性的值。 有关详细信息，请参阅 [在包中添加、删除或共享连接管理器](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))。  
  
2.  创建一个字符串变量，用于在每次循环迭代中接收当前表的名称。  
  
3.  将 Foreach 循环容器添加到 **“控制流”** 选项卡。有关如何配置 Foreach 循环容器的信息，请参阅 [配置 Foreach 循环容器](./foreach-loop-container.md)。  
  
4.  在 **“Foreach 循环编辑器”** 的 **“集合”** 页上，选择 Foreach ADO.NET 架构行级枚举器。  
  
5.  对于 **“连接”** 的值，请选择前面创建的 ADO.NET 连接管理器。  
  
6.  对于 **“架构”** 的值，选择“表”。  
  
    > [!NOTE]  
    >  Excel 工作簿中的表列表同时包括工作表（具有 $ 后缀）和指定范围。 如果要从列表中只筛选出工作表或指定范围，则必须在脚本任务中编写自定义代码来实现这一点。 有关详细信息，请参阅 [使用脚本任务使用的 Excel 文件](../../integration-services/extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)。  
  
7.  在 **“变量映射”** 页上，将索引 2 映射到以前创建的字符串变量，以存放当前表的名称。  
  
8.  关闭 **“Foreach 循环编辑器”**。  
  
9. 在 Foreach 循环容器中创建任务，这些任务使用 Excel 连接管理器对指定工作簿中的每个 Excel 表执行相同的操作。 如果你使用脚本任务来检查枚举表名或处理每个表，请记住将字符串变量添加到脚本任务的 ReadOnlyVariables 属性。  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)  
 [配置 Foreach 循环容器](./foreach-loop-container.md)   
 [添加或更改属性表达式](../../integration-services/expressions/add-or-change-a-property-expression.md)   
 [Excel 连接管理器](../../integration-services/connection-manager/excel-connection-manager.md)   
 [Excel 源](../../integration-services/data-flow/excel-source.md)   
 [Excel 目标](../../integration-services/data-flow/excel-destination.md)   
 [使用脚本任务处理 Excel 文件](../../integration-services/extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)  
  
