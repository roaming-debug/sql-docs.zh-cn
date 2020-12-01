---
description: 第 4-4 课：添加平面文件目标
title: 步骤 4：添加平面文件目标 | Microsoft Docs
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f4088de3-16d8-419c-96a1-a2cd005d0a5b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 99266f19a7bb1476f16b5896deb8dfa3602e4032
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449636"
---
# <a name="lesson-4-4-add-a-flat-file-destination"></a>第 4-4 课：添加平面文件目标

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Lookup Currency Key 转换的错误输出将查找失败的所有数据行重定向到脚本转换操作。 为了提供有关发生的错误的详细信息，脚本转换将运行可获取每个错误说明的脚本。  
  
在本任务中，请将有关失败行的所有此类信息保存到带分隔符的文本文件中，以便进行后续处理。 要保存失败的行，请为包含错误数据和平面文件目标的文本文件添加和配置平面文件连接管理器。 通过设置平面文件目标所用平面文件连接管理器的属性，可以指定平面文件目标如何格式化和写入文本文件。 有关详细信息，请参阅[平面文件连接管理器](../integration-services/connection-manager/flat-file-connection-manager.md)和[平面文件目标](../integration-services/data-flow/flat-file-destination.md)。  
  
## <a name="add-and-configure-a-flat-file-destination"></a>添加和配置平面文件目标  
  
1.  单击“数据流”选项卡。  
  
2.  在“SSIS 工具箱”中，展开“其他目标”，然后将“平面文件目标”拖动到数据流设计图面上。 将“平面文件目标”直接放在“获取错误说明”转换的下面。  
  
3.  选择“获取错误说明”转换，然后将蓝色箭头拖动到新的“平面文件目标”上。  
  
4.  在“数据流”设计图面上，在新的“平面文件目标”转换中选择名称“平面文件目标”，然后将该名称更改为“失败的行”。  
  
5.  右键单击“失败的行”转换，然后选择“编辑”，然后在”平面文件目标编辑器”中选择“新建”。  
  
6.  在“平面文件格式”对话框中，确保已选中“带分隔符”，然后选择“确定”。  
  
7.  在“平面文件连接管理器编辑器”的“连接管理器名称”框中，输入“错误数据”。  
  
8.  在“平面文件连接管理器编辑器”对话框中，选择“浏览”，然后找到存储文件的文件夹。  
  
9. 在“打开”对话框中，对于“文件名”，输入“ErrorOutput.txt”，然后选择“打开”。  
  
10. 在“平面文件连接管理器编辑器”对话框中，确保“区域设置”为“英语(美国)”且“代码页”为“1252 (ANSI -Latin I)”。  
  
11. 在“选项”窗格中，选择“列”。  
  
    除源数据文件中的列之外，还存在三个新列：ErrorCode、ErrorColumn 和 ErrorDescription。 这些列是 Lookup Currency Key 转换的错误输出和用于获取错误说明转换中的脚本。 这些列可用于排查失败行的原因。  
  
12. 选择“确定” 。  
  
13. 在“平面文件目标编辑器”中，清除“覆盖文件中的数据”复选框。  
  
    清除该复选框会通过附加每个新运行的错误输出使错误在执行多个包的过程中持续存在。
  
14. 在“平面文件目标编辑器”中，选择“映射”以验证所有列是否正确。 您也可以选择重命名目标中的列。  
  
15. 选择“确定”  。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 5：测试第 4 课教程包](../integration-services/lesson-4-5-testing-the-lesson-4-tutorial-package.md)  
  
  
  
