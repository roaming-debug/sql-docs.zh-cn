---
title: 指定部署前或部署后脚本
description: 了解如何使用预先部署和后期部署脚本在主部署脚本运行前后执行 Transact-SQL 语句。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 7f78f517-f13d-4f4b-84b9-e804cb490b2c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d4565e9b6ab4efc133aef84ff9d4d6dbbd6c501a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018107"
---
# <a name="how-to-specify-predeployment-or-postdeployment-scripts"></a>如何：指定预先部署或后期部署脚本

预先部署和后期部署脚本在从数据库项目生成的主部署脚本之前和之后执行 Transact\-SQL 语句。 在 Visual Studio 中，从架构比较结果更新目标时，将不执行预部署脚本。 一个项目只能有一个预先部署脚本和一个后期部署脚本。 这些脚本可用于许多用途。 例如：  
  
-   预先部署脚本可以从要更改的表中将数据复制到一个临时表，然后重新设置数据格式并在后期部署脚本中将数据应用于已更改表。  
  
-   您可以将必须在某个表中存在的引用数据插入后期部署脚本中。 在插入数据前，您可以测试此表是否已包含数据。 如果此表不为空，则必须清除现有数据或指定希望始终在部署数据库之前重新创建此数据库。 您可以将如下语句添加到后期部署脚本中：  
  
```  
IF (EXISTS(SELECT * FROM [dbo].[MyReferenceTable]))  
BEGIN  
    DELETE FROM [dbo].[MyReferenceTable]  
END  
```  

## <a name="to-add-and-modify-a-pre--or-post-deployment-script"></a>添加和修改预先部署或后期部署脚本  
  
1.  在“解决方案资源管理器”中，展开数据库项目以便显示“脚本”文件夹。  
  
2.  右键单击“脚本”文件夹并选择“添加”。  
  
3.  从上下文菜单中选择“脚本”。  
  
4.  选择“预先部署脚本”或“后期部署脚本”。 可以选择指定一个非默认名称。 单击“添加”以便完成。  
  
5.  在“脚本”文件夹中双击该文件。  
  
    Transact\-SQL 编辑器将打开，并且显示该文件的内容。  
  
您可以在脚本中使用 SQLCMD 语法和变量，并且在数据库项目属性中设置它们。 例如：  
  
-   您可以使用 SQLCMD 语法在预先部署或后期部署脚本中包含某一文件的内容。 文件将被包含并且按照您定义的顺序运行：`:r .\myfile.sql`  
  
-   您可以使用 SQLCMD 语法在后期部署脚本中引用某一变量。 您在项目属性或发布配置文件中设置 SQLCMD 变量：  
  
    ```  
    :setvar TableName MyTable  
    SELECT * FROM [$(TableName)]  
    ```  
  
有关如何在脚本中使用 SQLCMD 的详细信息，请参阅[数据库项目设置](../ssdt/database-project-settings.md)。  
  
## <a name="see-also"></a>另请参阅  
[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)  
  
