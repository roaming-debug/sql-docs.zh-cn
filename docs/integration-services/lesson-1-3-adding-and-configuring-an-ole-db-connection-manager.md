---
description: 第 1-3 课：添加和配置 OLE DB 连接管理器
title: 步骤 3：添加和配置 OLE DB 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e7b74cba-a0e5-4478-af12-3f81b9484e9e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 33eeb6e462c096505fc4fbd1088e7f5b36b37618
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449659"
---
# <a name="lesson-1-3-add-and-configure-an-ole-db-connection-manager"></a>第 1-3 课：添加和配置 OLE DB 连接管理器

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



添加了用于连接到数据源的平面文件连接管理器后，请添加用于连接到数据目标的 OLE DB 连接管理器。 通过 OLE DB 连接管理器，包可以在任何 OLE DB 兼容的数据源中提取数据或加载数据。 使用 OLE DB 连接管理器，可以为连接指定服务器、身份验证方法和默认数据库。  
  
在本课中，将创建使用 Windows 身份验证连接到 AdventureWorksDW2012 的本地实例的 OLE DB 连接管理器。 本教程以后要创建的其他组件（如查找转换和 OLE DB 目标）也将引用此 OLE DB 连接管理器。  
  
## <a name="add-and-configure-an-ole-db-connection-manager"></a>添加和配置 OLE DB 连接管理器

1. 在“解决方案资源管理器”窗格中，右键单击“连接管理器”，然后选择“新建连接管理器”。

1. 在“添加 SSIS 连接管理器”对话框中，选择“OLEDB”，然后选择“添加”。
    
2. 在“配置 OLE DB 连接管理器”对话框中，选择“新建”。  
  
3. 在“服务器名称”中，输入 **localhost**。  
  
    将 localhost 指定为服务器名称时，连接管理器将连接到本地计算机上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的默认实例。 若要使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的远程实例，请将 localhost 替换为要连接到的服务器的名称。  
  
4. 在“登录到服务器”组中，确认选择了“使用 Windows 身份验证”。  
  
5. 在“连接到数据库”组的“选择或输入数据库名称”框中，输入或选择“AdventureWorksDW2012”。  
  
6. 单击“测试连接”，验证指定的连接设置是否有效。  
  
7. 选择“确定”  。  
  
8. 选择“确定”。  
  
9. 在“连接管理器”窗格中，确保已选中“localhost.AdventureWorksDW2012”。  
  

## <a name="go-to-next-task"></a>转到下一个任务
[步骤 4：向包添加数据流任务](../integration-services/lesson-1-4-adding-a-data-flow-task-to-the-package.md)  
  
## <a name="see-also"></a>另请参阅  
[“无缓存”](../integration-services/connection-manager/ole-db-connection-manager.md)  
  
