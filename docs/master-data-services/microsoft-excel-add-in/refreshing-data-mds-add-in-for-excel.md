---
description: 刷新数据（用于 Excel 的 MDS 外接程序）
title: 刷新数据
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 58dbe99a-288d-4f1c-9cd5-704d6836c945
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e0661048822933b8b49274d7addc40cec130e289
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343191"
---
# <a name="refreshing-data-mds-add-in-for-excel"></a>刷新数据（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，当你要从 MDS 存储库中获取最新信息但不想打开新工作表时，可刷新数据。 您可以刷新所有单元，也可以刷新所选单元。 如果您插入的列中包含自定义公式或其他不由 MDS 管理但您想要保留的数据，这样做会很有帮助。  
  
## <a name="when-you-can-refresh-mds-managed-data"></a>何时可以刷新 MDS 管理的数据  
 如果活动工作表已包含 MDS 管理的数据，则您可以刷新该活动工作表中 MDS 管理的数据。 如果更改了工作表中的属性值或向工作表添加了成员，则必须在刷新之前先发布所做更改。  
  
## <a name="refreshing-a-selection"></a>刷新选定内容  
 您可以选择刷新所有单元还是只刷新所选单元。 所选单元必须是连续的。 如果选择了一组连续单元，则该组中所有的 MDS 托管单元都将更新，以显示服务器上当前存储的值。 并非由 MDS 管理的未更改的行和列不会受任何影响。  
  
## <a name="what-happens-when-you-refresh-mds-managed-data"></a>刷新 MDS 管理的数据时执行的操作  
 在 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中刷新数据时，对工作表中 MDS 管理的数据执行的操作取决于自上次加载数据以来 MDS 存储库中所发生的更改。  
  
-   如果向存储库添加了新成员，这些成员将添加到工作表的末尾并以绿色突出显示。  
  
-   如果从存储库中删除了成员，这些成员会从工作表中删除。  
  
-   如果属性值在 MDS 存储库中发生了更改，工作表中的值将随 MDS 存储库中的值更新。 单元颜色不会更改。  
  
> [!WARNING]
>  -   在活动工作表中，如果在 MDS 管理的数据之下的行中存在非 MDS 管理的数据，则可能覆盖这些非 MDS 管理的数据。 发生这种情况是由于您刷新了工作表，添加了与非 MDS 管理的数据重叠的 MDS 管理的新数据行。  
> -   刷新时，将删除 MDS 管理的单元上的注释。  
  
## <a name="how-to-refresh-mds-managed-data"></a>如何刷新 MDS 管理的数据  
 在功能区上的 **“连接并加载”** 组中， **“刷新”** 按钮提供两个选项： **“全部刷新”** 和 **“刷新所选内容”**。 功能区按钮的默认操作是 **“全部刷新”**。 若要使用服务器的值刷新整个工作表，请单击 **“刷新”** 按钮或选择 **“全部刷新”** 选项。 若要仅刷新工作表中的部分单元格，请选择这些单元格（必须是一个连续选择的单元格区域），然后选择“刷新所选内容”选项。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建与 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库的连接。|[连接到 MDS 存储库（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)|  
|将 MDS 数据加载到 Excel 中。|[将数据从 Master Data Services 导出到 Excel](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [连接（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [概述：将数据导出到 Excel（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
-   [用于 Microsoft Excel 的 Master Data Services 外接程序](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
