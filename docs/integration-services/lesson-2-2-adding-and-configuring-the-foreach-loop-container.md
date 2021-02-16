---
description: 第 2-2 课：添加和配置 Foreach 循环容器
title: 步骤 2：添加和配置 Foreach 循环容器 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c63288019fa6938f4481bbded2e5b29c29198d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354315"
---
# <a name="lesson-2-2-add-and-configure-the-foreach-loop-container"></a>第 2-2 课：添加和配置 Foreach 循环容器

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本任务中，将添加循环访问平面文件的文件夹的功能，并将第 1 课中的数据流转换应用于其中的每个平面文件。 实现方法是将 Foreach 循环容器添加到控制流中并进行配置。  
  
所添加的 Foreach 循环容器必须能够连接到该文件夹中的每个平面文件。 由于该文件夹中的所有文件都具有相同的格式，因此，Foreach 循环容器可以使用同一平面文件连接管理器来连接其中的每个文件。 容器使用的平面文件连接管理器是第 1 课中创建的管理器。  
  
目前，第 1 课中的平面文件连接管理器只连接一个特定的平面文件。 若要循环地连接该文件夹中的每个平面文件，必须同时对 Foreach 循环容器和平面文件连接管理器进行如下配置：  
  
-   **Foreach 循环容器：** 将该容器的枚举值映射为用户定义的包变量。 然后，该容器使用此变量来动态修改平面文件连接管理器的 ConnectionString 属性，并循环连接该文件夹中的每个平面文件  。  
  
-   **平面文件连接管理器：** 使用用户定义的变量填充在第 1 课中创建的连接管理器的 ConnectionString 属性，以修改该连接管理器  。  
  
本任务中的过程介绍了如何创建和修改 Foreach 循环容器以使用用户定义的包变量，以及如何将数据流任务添加到该循环中。 你将了解改平面文件连接管理器，以便在下一任务中使用用户定义的变量。  
  
在对该包进行这些修改后，当该包运行时，Foreach 循环容器将循环访问示例数据文件夹中的所有文件。 每次找到一个与条件相匹配的文件时，Foreach 循环容器都会用文件名填充新变量，将该变量映射到 Sample Currency Data 平面文件连接管理器的 ConnectionString 属性，然后对该文件运行数据流  。 这样一来，在 Foreach 循环的每次迭代中，数据流任务都会使用一个不同的平面文件。  
  
> [!NOTE]  
> 由于 [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 区分控制流和数据流，因此添加到控制流的任何循环都不需要对数据流进行修改。 因此，无需更改第 1 课数据流。  
  
## <a name="add-a-foreach-loop-container"></a>添加 Foreach 循环容器  
  
1.  在“SQL Server Data Tools”中，选择“控制流”选项卡   。  
  
2.  在“SSIS 工具箱”  中，展开“容器”  ，然后将“Foreach 循环容器”  拖到“控制流”  选项卡的设计图面上。  
  
3.  右键单击新的“Foreach 循环容器”，然后选择“编辑”   。  
  
4.  在“Foreach 循环编辑器”对话框的“常规”页中，对于“名称”，请输入 Foreach File in Folder     。 选择“确定”  。  
  
5.  右键单击“Foreach 循环容器”，然后选择“属性”，在“属性”窗口中，确保已将 LocaleID 属性设置为“英语(美国)”     。  
  
## <a name="configure-the-enumerator-for-the-foreach-loop-container"></a>为 Foreach 循环容器配置枚举器  
  
1.  双击“Foreach File in Folder”以重新打开“Foreach 循环编辑器”   。  
  
2.  选择“集合”。  
  
3.  在“集合”  页上，选择“Foreach 文件枚举器”  。  
  
4.  在“枚举器配置”组中，选择“浏览”   。  
  
5.  在“浏览文件夹”对话框中，找到计算机上包含 Currency_*.txt 文件（随附示例数据）的文件夹  。

6.  在“文件”框中，输入 Currency_\*.txt   。  
  
## <a name="map-the-enumerator-to-a-user-defined-variable"></a>将枚举器映射为用户定义的变量  
  
1.  选择“变量映射”  。  
  
2.  在“变量映射”页的“变量”列中，选择空单元格，然后选择“\<New Variable...>”  。  
  
3.  在“添加变量”对话框中，对于“名称”，请键入 varFileName    。  
  
    > [!NOTE]  
    > 变量名称区分大小写。  
  
4.  选择“确定”  。  
  
5.  再次选择“确定”，退出“Foreach 循环编辑器”对话框   。  
  
## <a name="add-the-data-flow-task-to-the-loop"></a>将数据流任务添加到循环中  
  
-   将“Extract Sample Currency Data”数据流任务拖到 Foreach File in Folder Foreach 循环容器中   。  
  
## <a name="go-to-next-task"></a>转到下一个任务  
[步骤 3：修改平面文件连接管理器](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a>另请参阅  
[配置 Foreach 循环容器](./control-flow/foreach-loop-container.md)  
[在包中使用变量](./integration-services-ssis-variables.md)  
  
  
