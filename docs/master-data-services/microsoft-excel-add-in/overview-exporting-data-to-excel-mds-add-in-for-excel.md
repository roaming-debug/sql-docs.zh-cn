---
description: 概述：将数据导出到 Excel（适用于 Excel 的 MDS 加载项）
title: 将数据导出到 Excel
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b628548b-982b-4e45-abf4-c8e83e3ab1c2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ab060952626699683379dbbbca848f3af36c68e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348870"
---
# <a name="overview-exporting-data-to-excel-mds-add-in-for-excel"></a>概述：将数据导出到 Excel（适用于 Excel 的 MDS 加载项）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，你必须先将数据从 MDS 存储库导出到 Excel 活动工作表中，然后才能处理这些数据。 完成数据处理后，将其导入到 MDS 存储库以便其他用户可以共享这些数据。  
  
 可以导出的数据仅限于你具有访问权限的数据。 访问数据的权限是在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序中或以编程方式设置的。  
  
 导出大量数据时，可以设置在数据可能需要较长时间加载时显示警告。 若要执行此操作，请在 **“选项”** 组中单击 **“设置”**。 在 **“数据”** 选项卡上，选择 **“显示针对大型数据集的筛选警告”**。  
  
> [!WARNING]  
>  只能使用用于 Excel 的 MDS 外接程序在 Excel 中打开和更新启用 MDS 的工作薄。 不支持在未安装 MDS Excel 外接程序的计算机上的 Excel 中打开启用 MDS 的工作簿，并且可能导致工作簿文件损坏。 若要与他人共享数据，应该通过电子邮件向他人发送快捷查询文件，而不是保存该工作表并通过电子邮件发送。 有关查询的详细信息，请参阅[以电子邮件形式发送快捷查询文件（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/email-a-shortcut-query-file-mds-add-in-for-excel.md)。  
  
## <a name="filtering-data"></a>筛选数据  
 可以在导出数据前先筛选数据，以限制要下载的数据量。 这包括选择要加载的属性（列）、属性的显示顺序，以及要处理的成员（数据行）。 有关详细信息，请参阅 [导出前筛选数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)。  
  
## <a name="connect-automatically-and-load-frequently-used-data"></a>自动连接和加载常用数据  
 如果总要连接同一服务器并导出相同的一组数据，可以创建快捷查询文件，其中包含连接和筛选器信息。 有关查询文件的详细信息，请参阅 [快捷查询文件（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)。  
  
## <a name="refreshing-data"></a>刷新数据  
 在导出 MDS 存储库中的数据后，其他用户可能更新这些数据。 可以检索此数据，而不会丢失对非 MDS 数据所做的更改。 有关详细信息，请参阅[刷新数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/refreshing-data-mds-add-in-for-excel.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|将 MDS 数据加载到 Excel 中之前先对其进行筛选。|[导出前筛选数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)|  
|将 MDS 数据加载到 Excel 中。|[将数据从 Master Data Services 导出到 Excel](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
|下载数据前更改列的顺序。|[对列重新排序（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/reorder-columns-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [连接（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [快捷查询文件（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
-   [用于 Microsoft Excel 的 Master Data Services 外接程序](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
-   [安全性 (Master Data Services)](../../master-data-services/security-master-data-services.md)  
  
  
