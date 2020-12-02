---
description: 解压缩 DAC 包
title: 解压缩 DAC 包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- wizard [DAC], unpack
- data-tier application [SQL Server], unpack
- How to [DAC], unpack
- unpack DAC
ms.assetid: 697b69b3-f157-4e22-ac4e-f65c5fc2d0ad
author: stevestein
ms.author: sstein
ms.openlocfilehash: dee530c223890a51ab255d319db9a7772e0ce686
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88499765"
---
# <a name="unpack-a-dac-package"></a>解压缩 DAC 包
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用“解压缩数据层应用程序”对话框可以从数据层应用程序 (DAC) 包解压缩脚本和文件。 这些脚本和文件放置在一个文件夹中，您可以在使用该 DAC 包将 DAC 部署到生产系统中之前查看该文件夹。 一个 DAC 的内容也可与解压缩到其他文件夹中的其他包的内容进行比较。  
  
1.  **开始之前：** [安全性](#Security)  
  
2.  **若要解压缩 DAC，请使用：**  [“解压缩数据层应用程序”对话框](#UnpackDACDial)、[检查 DAC 包的内容](#ExamDACPack)  

##  <a name="security"></a><a name="Security"></a> Security  
 建议您不要从未知或不可信源部署 DAC 包。 此类 DAC 可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构导致错误。 在使用来自未知或不可信源的 DAC 之前，请将其部署到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的一个独立的测试实例中，解压缩该 DAC 并检查代码，例如存储过程或者其他用户定义的代码。  
  
##  <a name="unpack-data-tier-application-dialog"></a><a name="UnpackDACDial"></a> “解压缩数据层应用程序”对话框  
 **解压缩 DAC 包文件**  
  
-   在 **Windows 资源管理器** 中，导航到 DAC 包 (.dacpac) 文件的位置。  
  
-   使用以下两种方法之一可打开“解压缩数据层应用程序”对话框：  
  
    1.  右键单击该 DAC 包 (.dacpac) 文件，然后选择“解压缩”。  
  
    2.  双击该 DAC 包文件。  
  
-   完成对话框：  
  
    -   [解压缩 Microsoft SQL Server DAC 包文件](#Unpack)  
  
    -   [查找文件夹](#Browse)  
  
###  <a name="unpack-microsoft-sql-server-dac-package-file"></a><a name="Unpack"></a> 解压缩 Microsoft SQL Server DAC 包文件  
 使用此页可指定用来放置解压缩文件的目标文件夹，然后运行解压缩操作。  
  
 **文件将解压缩到此文件夹：** - 指定解压缩文件所在文件夹的完整路径。 如果该文件夹存在并且您知道完整路径，请在框中键入该路径。 如果该文件夹不存在或者您不知道完整路径，请单击 **“浏览”** 以导航到某一文件夹或者创建一个新文件夹。  
  
 **浏览** - 打开“浏览文件夹”页，从中可以通过导航文件层次结构选择一个文件夹，或者创建一个新文件夹。  
  
 **解压缩** - 启动解压缩操作。  
  
 **取消** - 终止该对话框且不解压缩 DAC 包。  
  
###  <a name="browse-for-folder"></a><a name="Browse"></a> 查找文件夹  
 使用此页可选择解压缩操作的目标文件夹。 或者，也可以创建一个新文件夹。  
  
 **文件夹列表** - 显示计算机的文件层次结构。 展开节点以导航到要将 DAC 包解压缩到的文件夹。 单击该文件夹，然后单击 **“确定”**。  
  
 **新建文件夹** - 打开一个对话框，从中可以指定要在文件夹层次结构中当前选择的文件夹中创建的新文件夹的名称。  
  
 **确定** - 放置你在“解压缩 DAC 包文件”页的“文件将解包到此文件夹”框中选择的文件夹的路径，并且返回到该页。  
  
 **取消** - 终止该对话框且未选择文件夹。  
  
##  <a name="examine-the-contents-of-a-dac-package"></a><a name="ExamDACPack"></a> 检查 DAC 包的内容  
 对包解压缩后，可以检查由“解压缩数据层应用程序”对话框生成的文件。 该对话框在所选目标文件夹中生成以下文件：  
  
1.  一个 Transact-SQL 脚本，包含用于创建在该 DAC 中定义的对象的语句。 该文件名是 *DACName*.sql，其中， *DACName* 是 DAC 的名称。  
  
2.  包中的所有 XML 文件。  
  
3.  来自 DAC 的“附加文件”部分的所有文件，例如 DAC 预部署文件或部署后文件。  
  
 有关详细信息，请参阅 [Validate a DAC Package](../../relational-databases/data-tier-applications/validate-a-dac-package.md)。  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [部署数据层应用程序](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)   
 [升级数据层应用程序](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)  
  
  
