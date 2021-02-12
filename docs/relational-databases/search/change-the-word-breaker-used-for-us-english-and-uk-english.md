---
description: Change the Word Breaker Used for US English and UK English
title: 更改用于美国英语和英国英语的断字符 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 6b5d2177-db98-47f5-b32e-4b80a2f74ffe
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de2501c3af1fcc24aed29dc66da455d7e2ff1f2b
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589280"
---
# <a name="change-the-word-breaker-used-for-us-english-and-uk-english"></a>Change the Word Breaker Used for US English and UK English

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 起，安装程序为英语语言安装新版本的断词器和词干分析器，替换这些组件的以前版本。 有关新组件的更改的行为的信息，请参阅 [全文搜索的行为更改](./full-text-search.md)。 本主题说明如何在这些组件的新版本和以前的版本间切换。 对于群集安装，应对所有主节点和被动节点进行这些更改。  
  
 部分旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用了不同的断字符，由用于美国英语 (LCID 1033) 和英国英语 (LCID 2057) 的不同 CLSID 表示。 在此发行版中，这两个 LCID 使用具有相同 CLSID 的相同组件，如下表中所示：  
  
|LCID|以前版本安装的断字符<br /><br /> 版本 12.0.6828.0|以前版本安装的词干分析器|此版本安装的断字符<br /><br /> 版本 14.0.4999.1038|此版本安装的词干分析器|  
|----------|-------------------------------------------------------------------------|--------------------------------------------|-----------------------------------------------------------------------|---------------------------------------|  
|2052<br />（美国英语）|188D6CC5-CB03-4C01-912E-47D21295D77E|EEED4C20-7F1B-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
|2057<br />（英国英语）|173C97E2-AEBE-437C-9445-01B237ABF2F6|D99F7670-7F1A-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
  
 本主题中所述的组件是在 `MSSQL\Binn` 实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件夹中安装的 DLL 文件。 完整路径通常是 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`。  
  
 有关断字符和词干分析器的详细信息，请参阅 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)。  
  
## <a name="switching-from-the-current-english-word-breaker-to-the-previous-english-word-breakers"></a>从当前的英语断字符切换到以前的英语断字符  
  
#### <a name="to-switch-from-the-current-version-of-the-us-english-word-breaker-to-the-previous-version"></a>从当前版本的美国英语断字符切换到以前的版本  
  
1.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID。  
  
2.  使用以下步骤添加 COM ClassID 的新键，用于 LCID 1033 的以前美国英语的断字符和词干分析器接口：  
  
    1.  为以前的断字符添加值为 **{188D6CC5-CB03-4C01-912E-47D21295D77E}** 的新键。  
  
    2.  将该键值的（默认）数据更新为 **langwrbk.dll**。  
  
    3.  为以前的词干分析器添加值为 **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}** 的新键。  
  
    4.  将该键值的（默认）数据更新为 **infosoft.dll**。  
  
3.  在注册表中，导航到以下节点：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\enu**。  
  
4.  将 **WBreakerClass** 键值更新为 **{188D6CC5-CB03-4C01-912E-47D21295D77E}**。  
  
5.  将 **StemmerClass** 键值更新为 **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}**。  
  
6.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  

#### <a name="to-switch-from-the-current-version-of-the-uk-english-word-breaker-to-the-previous-version"></a>从当前版本的英国英语断字符切换到以前的版本  
  
1.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID。  
  
2.  使用以下步骤添加 COM ClassID 的新键，用于 LCID 2057 的以前英国英语的断字符和词干分析器接口：  
  
    1.  为以前的断字符添加值为 **{173C97E2-AEBE-437C-9445-01B237ABF2F6}** 的新键。  
  
    2.  将该键值的（默认）数据更新为 **langwrbk.dll**。  
  
    3.  为以前的词干分析器添加值为 **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}** 的新键。  
  
    4.  将该键值的（默认）数据更新为 **infosoft.dll**。  
  
3.  在注册表中，导航到以下节点：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**。  
  
4.  将 **WBreakerClass** 键值更新为 **{173C97E2-AEBE-437C-9445-01B237ABF2F6}**。  
  
5.  将 **StemmerClass** 键值更新 **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}**。  
  
6.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
## <a name="switching-back-from-the-previous-english-word-breakers-to-the-current-english-word-breaker"></a>从以前的英语断字符切换回当前的英语断字符  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-us-english-word-breaker-to-the-current-version"></a>从以前版本的美国英语断字符切换回当前版本  
  
1.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID。  
  
2.  如果以下键不存在，则使用以下步骤添加 COM ClassID 的新键，用于 LCID 1033 的当前美国英语的断字符和词干分析器接口：  
  
    1.  添加值为 **{9faed859-0b30-4434-ae65-412e14a16fb8}** 的新键用于当前断字符。  
  
    2.  将该键值的（默认）数据更新为 **MsWb7.dll**。  
  
    3.  添加值为 **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** 的新键用于当前词干分析器。  
  
    4.  将该键值的（默认）数据更新为 **MsWb7.dll**。  
  
3.  在注册表中，导航到以下节点：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**。  
  
4.  将 **WBreakerClass** 键值更新为 **{9faed859-0b30-4434-ae65-412e14a16fb8}**。  
  
5.  将 **StemmerClass** 键值更新为 **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**。  
  
6.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-uk-english-word-breaker-to-the-current-version"></a>从以前版本的英国英语断字符切换回当前版本  
  
1.  在注册表中，导航到以下节点：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID。  
  
2.  如果以下键不存在，则使用以下步骤添加 COM ClassID 的新键，用于 LCID 2057 的当前英国英语的断字符和词干分析器接口：  
  
    1.  添加值为 **{9faed859-0b30-4434-ae65-412e14a16fb8}** 的新键用于当前断字符。  
  
    2.  将该键值的（默认）数据更新为 **MsWb7.dll**。  
  
    3.  添加值为 **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** 的新键用于当前词干分析器。  
  
    4.  将该键值的（默认）数据更新为 **MsWb7.dll**。  
  
3.  在注册表中，导航到以下节点：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**。  
  
4.  将 **WBreakerClass** 键值更新为 **{9faed859-0b30-4434-ae65-412e14a16fb8}**。  
  
5.  将 **StemmerClass** 键值更新为 **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**。  
  
6.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
## <a name="see-also"></a>另请参阅  
 [将搜索功能所使用的断字符还原到以前的版本](../../relational-databases/search/revert-the-word-breakers-used-by-search-to-the-previous-version.md)   
 [对全文搜索的行为更改](./full-text-search.md)  
  
