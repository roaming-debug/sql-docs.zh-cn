---
description: 合并冲突（用于 Excel 的 MDS 外接程序）
title: 合并冲突
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cf95978f-a2c5-4325-8606-dbd4e88741b8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3cb483ab4a9e83289bdc250fc442657a50cb44e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343601"
---
# <a name="merge-conflicts-mds-add-in-for-excel"></a>合并冲突（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在用于 Excel 的 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 外接程序中，如果数据已被另一个用户在服务器上更改，则发布将失败并显示冲突错误。 若要解决此错误，可以执行合并冲突，然后重新发布更改。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-    您必须有权访问“资源管理器”功能区域。  
  
-   对于你要更新的实体的叶模型对象，你必须至少具有更新权限。  
  
-   活动工作表必须具有 MDS 管理的数据。  
  
-   在你尝试发布更改之后，活动工作表必须具有冲突错误。  
  
### <a name="to-merge-conflicts"></a>合并冲突  
  
1.  在工作表中，选择具有冲突错误的行或单元格。  
  
2.  在“发布并验证”  菜单组中，选择“合并冲突”  以打开“合并冲突”  对话框。  
  
3.  在“合并冲突”  对话框中，可以执行以下操作：  
  
    -   选择“最新”  ，然后单击“应用”  以撤消挂起的更改并从服务器重新加载最新版本。  
  
    -   选择“原始”  ，然后单击“应用”  以在工作表中应用原始版本。  
  
    -   选择“你的变更”  ，然后单击“应用  以保留现有的本地更改。  
  
4.  单击“应用” 后，可以进行其他更改，并再次发布。 或者，可以单击“取消”  以取消更新并从服务器重新加载最新的版本。  
  
## <a name="see-also"></a>另请参阅  
 [概述：从 Excel 导入数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
