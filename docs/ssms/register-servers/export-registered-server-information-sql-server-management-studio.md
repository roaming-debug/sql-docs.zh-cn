---
description: 导出已注册服务器信息 (SQL Server Management Studio)
title: 导出已注册的服务器信息
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.exportregisteredservers.f1
helpviewer_keywords:
- Registered Servers [SQL Server], exporting
- exporting registered server information
- transferring registered server information
ms.assetid: b65e168f-b6bf-489c-b8ad-3b8644acf0b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a71bebde2525a18e062832aba064a4b9aecf7f3a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250447"
---
# <a name="export-registered-server-information-sql-server-management-studio"></a>导出已注册服务器信息 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何保存和导出 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中的已注册服务器信息并将其分发到其他雇员或服务器。 您可以使用此导出功能在多台计算机上显示一致的用户界面。  
  
 通过导出再导入已注册服务器的相关文件，可以便捷地为多台计算机配置与“已注册的服务器”中相同服务器之间的连接。 当从不同位置的计算机管理大量服务器，或希望为不熟练的用户配置基本的连接设置时，这非常有用。  
  
> [!NOTE]  
>  使用 SQL Server 身份验证的服务器注册按用户来存储密码。 在导出并导入服务器注册信息后，用户在首次连接时必须输入每台服务器的密码，以便将密码存储在该用户的已注册服务器列表中。 对于使用 Windows 身份验证进行注册的服务器，则不需要如此操作。  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-export-registered-server-information"></a>导出已注册服务器信息  
  
1.  在已注册的服务器上，右键单击服务器组，再单击“导出”。  
  
    > [!NOTE]  
    >  您可以导出单个服务器、所有已注册服务器树或已注册服务器树的子集。  
  
2.  在 **“导出已注册的服务器”** 对话框中，进行以下选择：  
  
     **服务器组**  
     指定要导出的服务器组。 将所有已注册的服务器、特定服务器组中已注册的服务器或单个已注册的服务器导出到导出文件中。 导出功能是递归的；例如，如果服务器组 A 包含服务器组 B，而服务器组 B 包含服务器组 C 和 D，则导出服务器组 A 会导出 A、B、C 和 D 中的所有条目。  
  
     服务器组仅显示当前已注册服务器树的服务器组。  
  
     **导出文件**  
     在文本框中键入导出文件的名称，或使用“浏览”按钮 (**...**) 定位到客户端计算机上的导出文件。 如果选择现有文件，则已注册服务器的信息将追加到文件末尾。 并使用 .regsrvr 扩展名。 如果希望其他用户或另一台计算机也可以使用已注册服务器信息，可以将文件保存在网络中。 其他用户就可以访问该文件并导入部分或全部已注册服务器信息。 如果选择现有文件作为导出文件，该文件的内容将被服务器注册信息覆盖。  
  
     **不在导出文件中包括用户名和密码**  
     在导出文件时不包括用户名。  
  
    > [!IMPORTANT]  
    >  尽管导出文件已加密，但是，如果该文件中包括用户名和 SQL Server 身份验证密码，则应当仔细控制对该文件的访问。 因此，默认情况下，不在导出文件中包括用户名和密码。  
  
## <a name="see-also"></a>另请参阅  
 [导入已注册的服务器信息 (SQL Server Management Studio)](./import-registered-server-information-sql-server-management-studio.md)   
 [创建新的已注册的服务器 (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)  
  
