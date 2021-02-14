---
description: 快捷查询文件（用于 Excel 的 MDS 外接程序）
title: 快捷查询文件
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 1ba0219a-6c40-41fa-aff9-8c8f41ef3220
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e791b65ad9ce17fd3c0120d774f72f7fa5934b69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345597"
---
# <a name="shortcut-query-files-mds-add-in-for-excel"></a>快捷查询文件（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，使用快捷查询文件可快速连接并加载常用数据。 当您要与他人共享 MDS 数据时，也可以使用这些文件。 不需要保存工作表再通过电子邮件发送，而应保存一个快捷查询文件然后通过电子邮件发送该文件。 这将确保您同时连接到 MDS 存储库，以获取最新数据。  
  
 快捷查询文件是包含以下相关信息的 XML 文件：  
  
-   MDS 存储库连接。  
  
-   模型、版本和实体。  
  
-   创建快捷方式时应用的任何筛选器。  
  
-   创建快捷方式时从左到右的列顺序。  
  
 可以将这些文件保存在一个列表中，然后在打开该外接程序时从中进行选择。 您可以将其导出到您的计算机或某个共享位置，也可以将其发送给其他人。  
  
 若要打开快捷查询文件，可以采用以下两种方法：你可以导入这些文件，也可以双击这些文件，从而让 QueryOpener 应用程序自动打开它们。  
  
## <a name="queryopener-application"></a>QueryOpener 应用程序  
 安装了 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] 的所有用户都装有名为 QueryOpener 的应用程序。 此应用程序用于在 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中打开快捷查询文件。 如果双击快捷查询文件，将自动使用此应用程序在该外接程序中打开该文件。  
  
 使用此应用程序打开快捷查询文件时，系统会提示将该连接视为“安全”连接，这意味着你信任此位置的内容。  (通过在提示窗口中选择 " **始终允许连接到此地址** "，可以确保连接是安全的。 ) 每次将连接标记为安全时，它都会添加到列表中。 如果要清空该列表，请打开 **“设置”** 对话框，然后在 **“添加到安全列表的服务器”** 部分中，单击 **“全部清除”**。  
  
 该应用程序的默认位置为 *drive*:\Program Files\Microsoft SQL Server\130\Master Data Services\Excel Add-In\Microsoft.MasterDataServices.QueryOpener.exe。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|将活动工作表的内容另存为一个快捷查询文件。|[保存快捷查询文件（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/save-a-shortcut-query-file-mds-add-in-for-excel.md)|  
|通过电子邮件发送表示活动工作表内容的快捷查询文件。|[以电子邮件形式发送快捷查询文件（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/email-a-shortcut-query-file-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [连接（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [用于 Microsoft Excel 的 Master Data Services 外接程序](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
-   [安全性 (Master Data Services)](../../master-data-services/security-master-data-services.md)  
  
  
