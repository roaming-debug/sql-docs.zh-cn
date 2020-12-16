---
description: 查找搜索属性的属性集 GUID 和属性整数 ID
title: 查找搜索属性的属性集 GUID 和属性整数 ID
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], configuring
ms.assetid: 7db79165-8bcc-4be6-8d40-12d44deda79f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 518d5479b1b07c13ab0613aaf2dc68f1431cb16b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460050"
---
# <a name="find-property-set-guids-and-property-integer-ids-for-search-properties"></a>查找搜索属性的属性集 GUID 和属性整数 ID
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题讨论在将属性添加到搜索属性列表且使其可由全文搜索进行搜索之前，如何获取所需的值。 这些值包括文档属性的属性集 GUID 和属性整数标识符。  
  
 可对由 IFilter 从二进制数据 - 即从存储在 varbinary、varbinary(max)（包括 FILESTREAM）或 image 数据类型列中的数据中 - 提取的文档属性进行全文搜索   。 若要使提取的属性可供搜索，必须手动将该属性添加到搜索属性列表。 搜索属性列表还必须与一个或多个全文索引相关联。 有关详细信息，请参阅 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。  
  
 在向属性列表添加可用属性之前，必须找到有关该属性的 2 段信息：  
  
-   该属性的属性集 GUID。  
  
-   该属性的整数标识符。  
  
 （当将属性添加到属性列表时，还必须提供名称和说明。 但是，不一定使用属性的规范名称和说明。）  
  
 本主题介绍可找到有关可用属性的信息的常用方法，尤其是有关 Microsoft 定义的属性。 有关由第三方定义的属性的信息，请参阅第三方文档或与该供应商联系。  
  
##  <a name="finding-information-about-widely-used-well-known-microsoft-properties"></a><a name="wellknown"></a> 查找广泛使用的、众所周知的 Microsoft 属性的信息  
 Microsoft 定义了数百个在多种上下文中使用的文档属性，但这只是每种文件格式使用的可用属性的一小部分。 在常用的 Windows 属性中包括一小部分泛型属性。 下表显示了众所周知的泛型属性的一些示例。 下表显示了已知名称、Windows 规范名称（来自 Microsoft 发布的属性说明）、属性集 GUID、属性整数标识符和简短说明。  
  
|已知名称|Windows 规范名称|属性集 GUID|整数标识符|说明|  
|----------------------|----------------------------|-----------------------|----------------|-----------------|  
|Authors|**System.Author**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|4|给定项的一个或多个创作者。|  
|标记|**System.Keywords**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|5|分配给该项的一组关键字（也称为标记）。|  
|类型|**System.PerceivedType**|28636AA6-953D-11D2-B5D6-00C04FD918D0|9|假设的文件类型，基于其规范类型。|  
|Title|**System.Title**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|2|项的标题。 例如，文档的标题、邮件的主题、照片的题注或音乐曲目的名称。|  
  
 为了提倡在文件格式之间保持一致性，Microsoft 为几类文档确定了部分常用的高优先级文档属性。 其中包括通信、联系人、文档、音乐文件、图片和视频。 有关每个类别排名靠前的属性的详细信息，请参阅 Windows 搜索文档中的 [system-defined properties for custom file formats](/windows/win32/search/-shell-systemdefinedpropertiesforfileformats) （自定义文件格式的系统定义属性）。  
  
 特定的文件格式可能实现三种类型的属性：  
  
-   由 [!INCLUDE[msCoName](../../includes/msconame-md.md)]定义的泛型属性。  
  
-   由 [!INCLUDE[msCoName](../../includes/msconame-md.md)]定义的特定于类别的属性。  
  
-   由软件供应商定义的特定于应用程序的自定义属性。  
  
##  <a name="finding-information-about-available-properties-by-using-filtdumpexe"></a><a name="filtdump"></a> 通过使用 FILTDUMP.EXE 查找有关可用属性的信息  
 若要了解哪些属性是由安装的 IFilter 发现和提取的，可安装并运行属于 **Windows SDK 的** filtdump.exe [!INCLUDE[msCoName](../../includes/msconame-md.md)] 实用工具。  
  
 从命令提示符运行 **filtdump.exe** 并提供一个参数。 此参数是具有特定文件类型的单独文件的名称，该文件类型是安装 IFilter 所针对的目标文件类型。 该实用工具显示文档中由 IFilter 发现的所有属性的列表及其属性集 GUID、整数标识符以及其他信息。  
  
 有关安装此软件的信息，请参阅 [Microsoft Windows SDK for Windows 7 and .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=8279)（用于 Windows 7 和 .NET Framework 4 的 Microsoft Windows SDK）。 在下载并安装该 SDK 后，请在下列文件夹中查找 filtdump.exe 实用工具。  
  
-   对于 64 位版本，请查看 `C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin\x64`。  
  
-   对于 32 位版本，请查看 `C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin`。  
  
##  <a name="finding-values-for-a-search-property-from-a-windows-property-description"></a><a name="propdesc"></a> 从 Windows 属性说明查找搜索属性的值  
 对于众所周知的 Windows 搜索属性，可以从属性说明 ( **propertyDescription** ) 的 **formatID** 和 **propID** 属性中获取你需要的信息。  
  
 下面的示例显示了典型的 Microsoft 属性说明的相关部分，在此示例中为 `System.Author` 属性的说明。 `formatID` 特性指定属性集 GUID `F29F85E0-4FF9-1068-AB91-08002B27B3D9`， `propID` 特性指定属性整数 ID `4.` 。请注意， `name` 特性指定 Windows 规范属性名称 `System.Author`。 （此示例中省略了不相关的属性描述部分。）  
  
```  
.  
propertyDescription  
name = System.Author  
...  
formatID = F29F85E0-4FF9-1068-AB91-08002B27B3D9  
propID = 4  
...  
```  
  
 有关该属性的完整说明，请参阅 Windows 搜索文档中的 [System.Author](/windows/win32/properties/props-system-author) 。  
  
 有关 Windows 属性的完整列表，请参阅也在 Windows 搜索文档中的 [Windows 属性](/windows/win32/properties/props)。  
  
##  <a name="adding-a-property-to-a-search-property-list"></a><a name="examples"></a> 将属性添加到搜索属性列表  
 下面的示例说明如何将属性添加到搜索属性列表中。 该示例使用 [ALTER SEARCH PROPERTY LIST](../../t-sql/statements/alter-search-property-list-transact-sql.md) 语句将 `System.Author` 属性添加到名为 `PropertyList1`的搜索属性列表，并为属性 `Author`提供用户友好名称。  
  
```  
ALTER SEARCH PROPERTY LIST PropertyList1   
  ADD 'Author'  
    WITH (  
          PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',  
          PROPERTY_INT_ID = 4,   
          PROPERTY_DESCRIPTION = 'System.Author - the author or authors of the item'   
         )  
GO  
```  
  
 有关创建搜索属性列表并将其与全文索引相关联的详细信息，请参阅 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)  
  
