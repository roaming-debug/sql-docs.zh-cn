---
description: 第 1-5 课：添加和配置平面文件源
title: 步骤 5：添加和配置平面文件源 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c95ce51-e0fe-4fc5-95eb-2945929f2b13
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfc6ffa21bf3f6c1205b1c9c667cc6e75868cece
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88390863"
---
# <a name="lesson-1-5-add-and-configure-the-flat-file-source"></a>第 1-5 课：添加和配置平面文件源

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在此任务中，向包中添加一个平面文件源并对其进行配置。 平面文件源是使用由平面文件连接管理器定义的元数据的数据流组件。 此元数据指定转换过程要从平面文件提取的数据的格式和结构。 平面文件源使用平面文件连接管理器中的格式定义从单个平面文件中提取数据。  
  
对于本任务，需将平面文件源配置为使用以前创建的“示例平面文件源数据”连接管理器。  
  
## <a name="add-a-flat-file-source-component"></a>添加平面文件源组件  
  
1.  要打开“数据流”设计器，请双击“Extract Sample Currency Data”数据流任务或选择“数据流”选项卡。  
  
2.  在“SSIS 工具箱”中，展开 **OtherSources**，然后将“平面文件源”拖动到“数据流”选项卡的设计图面上。  
  
3.  在“数据流”设计图面上，右键单击新添加的“平面文件源”，选择“重命名”，然后将该名称改为“Extract Sample Currency Data”。  
  
4.  双击此平面文件源，打开“平面文件源编辑器”对话框。  
  
5.  在“平面文件连接管理器”字段中，选择“Sample Flat File Source Data”。  
  
6.  选择“列”并验证列名是否正确。  
  
7.  选择“确定” 。  
  
8.  右键单击“平面文件源”并选择“属性”。  
  
9. 在“属性”窗口中，验证是否已将 LocaleID 属性设置为“英语(美国)”。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 6：添加和配置查找转换](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
## <a name="see-also"></a>另请参阅  
[平面文件源](../integration-services/data-flow/flat-file-source.md)  
[平面文件连接管理器](../integration-services/connection-manager/flat-file-connection-manager.md)  
  
  
  
