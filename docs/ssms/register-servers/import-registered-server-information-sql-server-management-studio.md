---
description: 导入已注册服务器信息 (SQL Server Management Studio)
title: 导入已注册的服务器信息
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.importregisteredservers.f1
helpviewer_keywords:
- transferring registered server information
- Registered Servers [SQL Server], importing
- importing registered server information
ms.assetid: cc497a14-1360-4887-b70c-002f042823b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: acf4033e4f4a7faed1fc771612fd038916da52ec
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250703"
---
# <a name="import-registered-server-information-sql-server-management-studio"></a>导入已注册服务器信息 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何导入 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中保存的已注册服务器信息。 通过导出再导入已注册服务器的相关文件，可以便捷地为多台计算机配置与“已注册的服务器”中相同服务器之间的连接。 当从不同位置的计算机管理大量服务器，或希望为不熟练的用户配置基本的连接设置时，这非常有用。  
  
> [!NOTE]  
>  您不能从早期版本的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中将已注册服务器信息导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-import-registered-server-information"></a>导入已注册的服务器信息  
  
1.  在已注册的服务器中，单击“已注册的服务器”工具栏上的服务器类型。 服务器类型必须与已注册的服务器的导出文件类型一样。 例如，如果您已经导出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已注册服务器信息，则必须在“已注册的服务器”工具栏上单击 **“SQL Server”** 。  
  
2.  右键单击服务器组，再选择“导入”。  
  
3.  在 **“导入已注册的服务器”** 对话框中，选择要导入的已注册的服务器文件，再单击 **“确定”**。  
  
     **导入文件**  
     在文本框中键入导入文件的名称，或单击“浏览”按钮 (**...**) 定位到客户端计算机上的导入文件。 如果选择现有文件，则已注册服务器的信息将追加到文件末尾。 导入文件只能是以前导出的已注册服务器的文件。 已注册服务器的文件的扩展名为 .regsrvr。  
  
     **选择要导入到的服务器组**  
     选择要将文件中的已注册的服务器条目导入到的根节点或特定服务器组。 您可以将所有已注册的服务器、特定服务器组中已注册的服务器或单个已注册的服务器导入到导出文件中。 导入功能是递归的；例如，如果服务器组 A 包含服务器组 B，而服务器组 B 包含服务器组 C 和 D，则导入服务器组 A 会导出 A、B、C 和 D 中的所有条目。  
  
     服务器组仅显示当前已注册服务器树的服务器组。  
  
     如果选择导入某个现有服务器组或服务器，则会显示一条消息，以便您确认是否要覆盖现有服务器或服务器组。  
  
 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的服务器注册按用户来存储密码。 在导入服务器注册信息后，用户在首次连接时必须输入每台服务器的密码，以便将密码存储在该用户的已注册服务器列表中。 对于通过 Windows 身份验证进行注册的服务器，则不需要如此操作。  
  
## <a name="see-also"></a>另请参阅  
 [更改服务器的注册信息 (SQL Server Management Studio)](./change-a-server-s-registration-sql-server-management-studio.md)   
 [导出已注册服务器信息 (SQL Server Management Studio)](./export-registered-server-information-sql-server-management-studio.md)   
 [创建新的已注册的服务器 (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)  
  
