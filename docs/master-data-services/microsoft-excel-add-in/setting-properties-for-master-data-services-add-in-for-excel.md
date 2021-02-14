---
description: 设置用于 Excel 的 Master Data Services 外接程序的属性
title: 设置属性
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cab1c662-5d40-4c16-9f5c-36ff9608810b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: af9b942b8fb2cf6b1d0838327d5e32d278068146
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352975"
---
# <a name="setting-properties-for-master-data-services-add-in-for-excel"></a>设置用于 Excel 的 Master Data Services 外接程序的属性

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  用于 Excel 的 Master Data Services 外接程序设置确定将 MDS 中的数据加载到 Excel 外接程序的方式和将 Excel 外接程序中的数据发布到 MDS 的方式。  
  
 若要设置 Excel 外接程序，请打开“Excel”，单击“主数据”菜单，然后单击“设置”。 具有对 Excel 的访问权限的任何人都可以更改这些设置。 这些设置适用于打开了 Excel 的计算机。  
  
## <a name="excel-add-in-settings"></a>Excel 外接程序设置  
  
|选项卡和部分|设置|描述|  
|-|-|-|  
|设置：发布|发布时显示 **“发布并添加批注”** 对话框|如果在单击 **“发布”** 之后选择显示 **“发布并添加批注”** 对话框，您便可以为所有更改输入单个批注或为每个更改输入一个批注。<br /><br /> 取消选择可指定启动发布过程时不显示 **“发布并添加批注”** 对话框。 您将没有机会输入批注。|  
|设置：版本|版本选择|选择将加载到 Excel 外接程序中的主数据的版本。 可以是：<br /><br /> **“无”** 以便不将任何版本设置为默认版本；<br /><br /> **“最早”** 以将默认版本设置为最早版本； **“最新”** 以将默认版本设置为最新版本。|  
|设置：日志记录|打开详细日志记录|对将 MDS 中的主数据加载到 Excel 外接程序的过程启用日志记录，以便记录服务中的每条命令的结果。|  
|设置：遥测|启用遥测数据收集|启用遥测有助于提升 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] Excel 外接程序的质量、可靠性和性能。|  
|设置：批大小|要加载的单元格数|选择一个数目，指示从 MDS 服务器加载到 Excel 的一个批次中将加载几千个单元格。 默认值为 50,000 个单元格。|  
|设置：批大小|要发布的单元格数|选择一个数目，指示从 Excel 返回到服务器的一个批次中将发布几千个单元格。 默认值为 50,000 个单元格。|  
|设置：添加到安全列表的服务器|全部清除|单击可清除在打开关联的快捷查询文件时被指定为安全的连接列表。|  
|数据：筛选器|显示针对大型数据集的筛选警告|如果要从 MDS 中加载到 Excel 的数据集超过最大行数或列数，则单击此选项会显示警告。|  
|数据：筛选器|最大行数|选择要加载的行数的阈值，超过该值将发布一个筛选器警告。|  
|数据：筛选器|最大列数|选择要加载的列数的阈值，超过该值将发布一个筛选器警告。|  
|数据：单元格格式|属性值更改时更改颜色|单击此选项可以指定：当您使用 MDS 库中的新数据刷新 Excel 外接程序表时，如果某个单元格中的属性值发生更改，则更改该单元格的颜色。|  
|数据：单元格格式|添加成员时更改颜色|单击此选项可以指定：当使用 MDS 存储库中的新数据刷新 Excel 外接程序表时，如果在某个行中添加了新成员，则更改该行的单元格的颜色。|  
|数据：单元格格式|显示格式|选择首选格式，以便显示基于域的属性的值。 选项包括：Code {Name}、Code 和 Name {Code}。|  
  
  
