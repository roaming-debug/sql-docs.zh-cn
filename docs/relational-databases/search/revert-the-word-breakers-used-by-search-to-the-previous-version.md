---
description: 将搜索功能所使用的断字符还原到以前的版本（SQL Server 搜索）
title: 将搜索功能所使用的断字符还原到以前的版本
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 29b4488e-4c6a-4bf0-a64d-19e2fdafa7ae
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: f99e5892f697b06bcafcfc35f9672d8ee474bc40
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589283"
---
# <a name="revert-word-breakers-used-by-search-to-previous-version-sql-server-search"></a>将搜索功能所使用的断字符还原到以前的版本（SQL Server 搜索）

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 对于全文搜索支持的所有语言（朝鲜语除外），安装并启用一个版本的断字符和词干分析器。 本文说明如何在这些组件的此版本和以前的版本间切换。  
  
 本文不讨论以下语言：  
  
- **英语**。 若要恢复或还原英语组件，请参阅 [更改用于美国英语和英国英语的断字符](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)。  
  
- **丹麦语、波兰语和土耳其语**。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的先前版本附带的丹麦语、波兰语和土耳其语的第三方断字符已替换为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 组件。  
  
- **捷克语和希腊语**。 捷克语和希腊语已有了新的断字符。 先前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索不包括对这两种语言的支持。  
  
- **朝鲜语**。 在此版本中不升级朝鲜语的断字符和词干分析器。  
  
 有关断字符和词干分析器的一般信息，请参阅 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)。  
  
##  <a name="overview-of-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="overview"></a> 恢复和还原断字符和词干分析器概述  
 还原和恢复断字符和词干分析器的说明取决于语言。 下表总结了还原为以前的组件版本可能需要的三组操作。  
  
|当前文件|以前的文件|受影响语言的数目|针对文件的操作|针对注册表项的操作|  
|------------------|-------------------|----------------------------------|----------------------|---------------------------------|  
|NaturalLanguage6.dll|NaturalLanguage6.dll|34|获取并安装早期版本的 NaturalLanguage6.dll，覆盖当前版本的文件。|无需采取措施。<br /><br /> 对于此版本，注册表项和值未更改。|  
|（其他文件名）|NaturalLanguage6.dll|5|获取并安装早期版本的 NaturalLanguage6.dll，覆盖当前版本的文件。|更改一组注册表项以便指定组件的以前版本。|  
|（其他文件名）|（其他文件名）|6|无需采取措施。<br /><br /> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装程序将组件的当前版本和以前版本都复制到 Binn 文件夹。|更改一组注册表项以便指定组件的以前版本。|  
  
> [!WARNING]  
>  如果您使用其他版本替换文件 NaturalLanguage6.dll 的当前版本，则使用此文件的所有语言的行为都将受到影响。  
  
 本文中所述的文件是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 `MSSQL\Binn` 文件夹中安装的 DLL 文件。 完整路径通常是以下路径：  
  
 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`  
  
##  <a name="languages-for-which-the-file-name-of-both-the-current-and-previous-word-breaker-is-naturallanguage6dll"></a><a name="nl6nl6"></a> 当前和以前的断字符的文件名均为 NaturalLanguage6.dll 的语言  
 对于下表中的语言，当前和以前的断字符的文件名均为 NaturalLanguage6.dll。 若要还原或恢复这些组件，您必须使用同一文件的不同版本覆盖 NaturalLanguage6.dll。 您不必更改任何注册表项，因为对于此版本，未更改这些注册表项。  
  
> [!WARNING]  
>  如果您使用其他版本替换文件 NaturalLanguage6.dll 的当前版本，则使用此文件的所有语言的行为都将受到影响。  
  
 **受影响的语言的列表**  
  
|语言|缩写<br />用于<br />注册表|LCID|  
|--------------|---------------------------------------|----------|  
|孟加拉语|`ben`|1093|  
|保加利亚语|`bgr`|1026|  
|加泰罗尼亚语|`cat`|1027|  
|西班牙语|`esn`|3082|  
|法语|`fra`|1036|  
|古吉拉特语|`guj`|1095|  
|希伯来语|`heb`|1037|  
|Hindi|`hin`|1081|  
|克罗地亚语|`hrv`|1050|  
|印度尼西亚语|`ind`|1057|  
|冰岛语|`isl`|1039|  
|意大利语|`ita`|1040|  
|卡纳达语|`kan`|1099|  
|立陶宛语|`lth`|1063|  
|拉脱维亚语|`lvi`|1062|  
|马拉雅拉姆语|`mal`|1100|  
|马拉地语|`mar`|1102|  
|马来语|`msl`|1086|  
|中立|`Neutral`|0000|  
|挪威语（博克马尔）|`nor`|1044|  
|旁遮普语|`pan`|1094|  
|葡萄牙语（巴西）|`ptb`|1046|  
|葡萄牙语|`ptg`|2070|  
|罗马尼亚语|`rom`|1048|  
|斯洛伐克语|`sky`|1051|  
|斯洛文尼亚语|`slv`|1060|  
|塞尔维亚语 - 西里尔|`srb`|3098|  
|塞尔维亚语 - 拉丁|`srl`|2074|  
|瑞典语|`sve`|1053|  
|泰米尔语|`tam`|1097|  
|泰卢固语|`tel`|1098|  
|乌克兰语|`ukr`|1058|  
|乌尔都语|`urd`|1056|  
|越南语|`vit`|1066|  
  
 前面的表在缩写列上按字母顺序排序。  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="nl6nl6revert"></a> 恢复到以前的组件  
  
1.  导航到上述 Binn 文件夹。  
  
2.  将 NaturalLanguage6.dll 的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 版本备份到其他位置。  
  
3.  将以前版本的 NaturalLanguage6.dll 从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 实例的 Binn 文件夹复制到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例的 Binn 文件夹中。  
  
    > [!WARNING]  
    >  此更改影响在当前版本和以前版本中均使用 NaturalLanguage6.dll 的所有语言。  
  
4.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  

###  <a name="to-restore-the-current-components"></a><a name="nl6nl6restore"></a> 还原当前组件  
  
1.  导航到备份了 NaturalLanguage6.dll 的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 版本的位置。  
  
2.  将 NaturalLanguage6.dll 的当前版本从该备份位置复制到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例的 Binn 文件夹中。  
  
    > [!WARNING]  
    >  此更改影响在当前版本和以前版本中均使用 NaturalLanguage6.dll 的所有语言。  
  
3.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
##  <a name="languages-for-which-the-file-name-of-the-previous-word-breaker-only-is-naturallanguage6dll"></a><a name="newnl6"></a> 仅以前的断字符的文件名为 NaturalLanguage6.dll 的语言  
 对于下表中的语言，以前的断字符的文件名不同于新版本的文件名。 以前的文件名为 NaturalLanguage6.dll。 若要还原为以前的版本，您必须使用同一文件的以前版本覆盖 NaturalLanguage6.dll 的当前版本。 您还必须更改一组注册表项以便指定组件的以前或当前版本。  
  
> [!WARNING]  
>  如果您使用其他版本替换文件 NaturalLanguage6.dll 的当前版本，则使用此文件的所有语言的行为都将受到影响。  
  
 **受影响的语言的列表**  
  
|语言|缩写<br />用于<br />注册表|LCID|  
|--------------|---------------------------------------|----------|  
|阿拉伯语|ara|1025|  
|德语|deu|1031|  
|日语|jpn|1041|  
|荷兰语|nld|1043|  
|俄语|rus|1049|  
  
 前面的表在缩写列上按字母顺序排序。  
  
 将以下说明与 [用于恢复和还原断字符和词干分析器的文件名和注册表值](#newnl6values)部分中的值列表一起使用。  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnl6revert"></a> 恢复到以前的组件  
  
1.  导航到上述 Binn 文件夹。  
  
2.  不要从 Binn 文件夹中删除组件的当前版本的文件。  
  
3.  将 NaturalLanguage6.dll 的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 版本备份到其他位置。  
  
4.  将旧版 NaturalLanguage6.dll 从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 实例的 Binn 文件夹复制到新的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例的 Binn 文件夹中。  
  
    > [!WARNING]  
    >  此更改影响在当前版本和以前版本中均使用 NaturalLanguage6.dll 的所有语言。  
  
5.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID。  
  
6.  使用以下步骤添加 COM ClassID 的新键，用于所选语言的以前的断字符和词干分析器接口：  
  
    1.  添加值来自以前的断字符的表的新键。  
  
    2.  将该键值的（默认）数据更新为来自该表的以前的断字符的文件名。  
  
    3.  如果所选语言使用某一词干分析器，则添加具有来自以前的词干分析器的表值的新键。  
  
    4.  如果所选语言使用某一词干分析器，则将该键值的（默认）数据更新为来自该表的以前的词干分析器的文件名。  
  
7.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>。 *<language_key>* 表示用于注册表中的语言的缩写；例如，“fra”表示法语，“esn”表示西班牙语。  
  
8.  将 **WBreakerClass** 键值更新为来自当前断字符的表的值。  
  
9. 如果所选语言使用某一词干分析器，则将 **StemmerClass** 键值更新为来自当前词干分析器的表的值。  
  
10. 重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
###  <a name="to-restore-the-current-components"></a><a name="newnl6restore"></a> 还原当前组件  
  
1.  导航到备份了 NaturalLanguage6.dll 的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 版本的位置。  
  
2.  将 NaturalLanguage6.dll 的当前版本从该备份位置复制到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例的 Binn 文件夹中。  
  
    > [!WARNING]  
    >  此更改影响在当前版本和以前版本中均使用 NaturalLanguage6.dll 的所有语言。  
  
3.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID。  
  
4.  如果以下键不存在，则使用以下步骤为所选语言的当前断字符和词干分析器接口的 COM ClassID 添加新键：  
  
    1.  添加值来自当前断字符的表的新键。  
  
    2.  将该键值的（默认）数据更新为来自该表的当前断字符的文件名。  
  
    3.  如果所选语言使用某一词干分析器，则添加具有来自当前词干分析器的表值的新键。  
  
    4.  如果所选语言使用某一词干分析器，则将该键值的（默认）数据更新为来自该表的当前词干分析器的文件名。  
  
5.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>。 *<language_key>* 表示用于注册表中的语言的缩写；例如，“fra”表示法语，“esn”表示西班牙语。  
  
6.  将 **WBreakerClass** 键值更新为来自以前断字符的表的值。  
  
7.  如果所选语言使用某一词干分析器，则将 **StemmerClass** 键值更新为来自以前词干分析器的表的值。  
  
8.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnl6values"></a> 用于恢复和还原断字符和词干分析器的文件名和注册表值  
 将文件名和注册表项的以下列表与前一节中的说明一起使用。 使用以前的值恢复到以前的版本，或者使用当前值还原组件的当前版本。  
  
 下表是用于各语言的缩写的按字母顺序排序的列表。  
  
 **阿拉伯语 (ara)，LCID 1025**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|7EFD3C7E-9E4B-4a93-9503-DECD74C0AC6D|483B0283-25DB-4c92-9C15-A65925CB95CE|  
|以前的文件名|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|当前的 CLSID|04b37e30-c9a9-4a7d-8f20-792fc87ddf71|无|  
|当前文件名|MSWB7.dll|无|  
  
 **德语 (deu)，LCID 1031**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|45EACA36-DBE9-4e4a-A26D-5C201902346D|65170AE4-0AD2-4fa5-B3BA-7CD73E2DA825|  
|以前的文件名|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|当前的 CLSID|dfa00c33-bf19-482e-a791-3c785b0149b4|8a474d89-6e2f-419c-8dd5-9b50edc8c787|  
|当前文件名|MSWB7.dll|MSWB7.dll|  
  
 **日语 (jpn)，LCID 1041**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|E1E8F15E-8BEC-45df-83BF-50FF84D0CAB5|3D5DF14F-649F-4cbc-853D-F18FEDE9CF5D|  
|以前的文件名|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|当前的 CLSID|04096682-6ece-4e9e-90c1-52d81f0422ed|无|  
|当前文件名|MsWb70011.dll|无|  
  
 **荷兰语 (nld)，LCID 1043**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|2C9F6BEB-C5B0-42b6-A5EE-84C24DC0D8EF|F7A465EE-13FB-409a-B878-195B420433AF|  
|以前的文件名|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|当前的 CLSID|69483c30-a9af-4552-8f84-a0796ad5285b|CF923CB5-1187-43ab-B053-3E44BED65FFA|  
|当前文件名|MSWB7.dll|MSWB7.dll|  
  
 **俄语 (rus)，LCID 1049**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|2CB6CDA4-1C14-4392-A8EC-81EEF1F2E079|E06A0DDD-E81A-4e93-8A8D-F386C3A1B670|  
|以前的文件名|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|当前的 CLSID|aaa3d3bd-6de7-4317-91a0-d25e7d3babc3|d42c8b70-adeb-4b81-a52f-c09f24f77dfa|  
|当前文件名|MSWB7.dll|MSWB7.dll|  
  
##  <a name="languages-for-which-neither-the-previous-nor-the-current-file-name-is-naturallanguage6dll"></a><a name="newnew"></a> 当前和以前的文件名均不是 NaturalLanguage6.dll 的语言  
 对于下表中的语言，以前的断字符和词干分析器的文件名不同于新版本的文件名。 当前和以前的文件名均不是 NaturalLanguage6.dll。 您不必替换任何文件，因为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装程序将组件的当前版本和以前版本都复制到 Binn 文件夹。 但是，您必须更改一组注册表项以便指定组件的以前或当前版本。  
  
 **受影响的语言的列表**  
  
|语言|缩写<br />用于<br />注册表|LCID|  
|--------------|---------------------------------------|----------|  
|简体中文|chs|2052|  
|繁体中文|cht|1028|  
|泰语|tha|1054|  
|中文(繁体)|zh-hk|3076|  
|中文(繁体)|zh-mo|5124|  
|简体中文|zh-sg|4100|  
  
 前面的表在缩写列上按字母顺序排序。  
  
 将以下说明与 [用于恢复和还原断字符和词干分析器的文件名和注册表值](#newnewvalues)部分中的值列表一起使用。  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnewrevert"></a> 恢复到以前的组件  
  
1.  不要从 Binn 文件夹中删除组件的当前版本的文件。  
  
2.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID。  
  
3.  使用以下步骤添加 COM ClassID 的新键，用于所选语言的以前的断字符和词干分析器接口：  
  
    1.  添加值来自以前的断字符的表的新键。  
  
    2.  将该键值的（默认）数据更新为来自该表的以前的断字符的文件名。  
  
    3.  如果所选语言使用某一词干分析器，则添加具有来自以前的词干分析器的表值的新键。  
  
    4.  如果所选语言使用某一词干分析器，则将该键值的（默认）数据更新为来自该表的以前的词干分析器的文件名。  
  
4.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>。 *<language_key>* 表示用于注册表中的语言的缩写；例如，“fra”表示法语，“esn”表示西班牙语。  
  
5.  将 **WBreakerClass** 键值更新为来自当前断字符的表的值。  
  
6.  如果所选语言使用某一词干分析器，则将 **StemmerClass** 键值更新为来自当前词干分析器的表的值。  
  
7.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
###  <a name="to-restore-the-previous-components"></a><a name="newnewrestore"></a> 还原以前的组件  
  
1.  不要从 Binn 文件夹中删除组件的以前版本的文件。  
  
2.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID。  
  
3.  如果以下键不存在，则使用以下步骤为所选语言的当前断字符和词干分析器接口的 COM ClassID 添加新键：  
  
    1.  添加值来自当前断字符的表的新键。  
  
    2.  将该键值的（默认）数据更新为来自该表的当前断字符的文件名。  
  
    3.  如果所选语言使用某一词干分析器，则添加具有来自当前词干分析器的表值的新键。  
  
    4.  如果所选语言使用某一词干分析器，则将该键值的（默认）数据更新为来自该表的当前词干分析器的文件名。  
  
4.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>。 *<language_key>* 表示用于注册表中的语言的缩写；例如，“fra”表示法语，“esn”表示西班牙语。  
  
5.  将 **WBreakerClass** 键值更新为来自以前断字符的表的值。  
  
6.  如果所选语言使用某一词干分析器，则将 **StemmerClass** 键值更新为来自以前词干分析器的表的值。  
  
7.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnewvalues"></a> 用于恢复和还原断字符和词干分析器的文件名和注册表值  
 将文件名和注册表项的以下列表与前一节中的说明一起使用。 使用以前的值恢复到以前的版本，或者使用当前值还原组件的当前版本。  
  
 下表是用于各语言的缩写的按字母顺序排序的列表。  
  
 **简体中文 (chs)，LCID 2052**  
  
|组件|断字符|  
|---------------|------------------|  
|以前的 CLSID|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|以前的文件名|chsbrkr.dll|  
|当前的 CLSID|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|当前文件名|MsWb70804.dll|  
  
 **繁体中文 (cht)，LCID 1028**  
  
|组件|断字符|  
|---------------|------------------|  
|以前的 CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|以前的文件名|chtbrkr.dll|  
|当前的 CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|当前文件名|MsWb70404.dll|  
  
 **泰国语 (tha)，LCID 1054**  
  
|组件|断字符|词干分析器|  
|---------------|------------------|-------------|  
|以前的 CLSID|CCA22CF4-59FE-11D1-BBFF-00C04FB97FDA|CEDC01C7-59FE-11D1-BBFF-00C04FB97FDA|  
|以前的文件名|Thawbrkr.dll|Thawbrkr.dll|  
|当前的 CLSID|F70C0935-6E9F-4ef1-9F06-7876536DB900|无|  
|当前文件名|MsWb7001e.dll|无|  
  
 **繁体中文 (zh-hk)，LCID 3076**  
  
|组件|断字符|  
|---------------|------------------|  
|以前的 CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|以前的文件名|chtbrkr.dll|  
|当前的 CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|当前文件名|MsWb70404.dll|  
  
 **繁体中文 (zh-mo)，LCID 5124**  
  
|组件|断字符|  
|---------------|------------------|  
|以前的 CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|以前的文件名|chtbrkr.dll|  
|当前的 CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|当前文件名|MsWb70404.dll|  
  
 **简体中文 (zh-sg)，LCID 4100**  
  
|组件|断字符|  
|---------------|------------------|  
|以前的 CLSID|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|以前的文件名|chsbrkr.dll|  
|当前的 CLSID|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|当前文件名|MsWb70804.dll|  
  
## <a name="see-also"></a>另请参阅  
 [更改用于美国英语和英国英语的断字符](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)   
 [对全文搜索的行为更改](./full-text-search.md)
