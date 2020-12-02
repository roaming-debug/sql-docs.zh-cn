---
description: 为全文搜索配置和管理同义词库文件
title: 为全文搜索配置和管理同义词库文件
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], thesaurus files
- thesaurus [full-text search], configuring
- thesaurus [full-text search]
ms.assetid: 3ef96a63-8a52-45be-9a1f-265bff400e54
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: d713b4eb49a527f2cbbbf871cce9d01d4449443d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130945"
---
# <a name="configure-and-manage-thesaurus-files-for-full-text-search"></a>为全文搜索配置和管理同义词库文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]全文搜索查询可以使用全文搜索同义词库来搜索用户指定字词的同义词。 每个同义词库为特定语言定义一组同义词。 通过开发针对全文数据定制的同义词库，您可以有效地扩大对这些数据的全文查询的范围。

仅对所有 [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) 和 [FREETEXTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 查询以及指定 `FORMSOF THESAURUS` 子句的任意 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 查询执行同义词库匹配操作。
  
全文搜索同义词库是一个 XML 文本文件。
  
##  <a name="whats-in-a-thesaurus"></a><a name="tasks"></a>同义词库包含哪些内容  
 必须先为给定语言定义同义词库映射（即同义词），才能使全文搜索查询可以查找该语言的同义词。 必须手动配置每个同义词库以定义下面各项：  
  
-   扩展集  
  
     扩展集包含一组同义词（如“writer”、“author”和“journalist”），全文查询可将这些同义词互相替换。 如果查询包含扩展集中任意同义词的匹配项，这些查询将进行扩展以包含扩展集中的每个其他同义词。  
  
     有关详细信息，请参阅本主题稍后的[扩展集的 XML 结构](#expansion)。  
  
-   替换集  
  
     替换集包含将由替换集替换的文本模式。 有关示例，请参阅本主题稍后的[替换集的 XML 结构](#replacement)部分。 

-   标注字符设置  
  
     对于给定同义词库，所有搜索模式要么区分标注字符，要么不区分标注字符，如波形符 (~)、锐音符 (&acute;) 或元音变音符 (&uml;)（即要么区分重音，要么不区分重音）。 例如，假设你在全文查询中指定要将“caf&eacute;”模式替换为其他模式。 如果同义词库不区分重音，全文搜索会替换“caf&eacute;”和“cafe”模式。 如果同义词库区分重音，全文搜索仅替换“caf&eacute;”模式。 默认情况下，同义词库不区分重音。  
  
##  <a name="default-thesaurus-files"></a><a name="initial_thesaurus_files"></a>默认同义词库文件
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了一组 XML 同义词库文件，分别对应于每种支持的语言。 这些文件实际上是空的。 它们仅包含所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 同义词库通用的顶级 XML 结构以及注释掉的示例同义词库。  
  
##  <a name="location-of-thesaurus-files"></a><a name="location"></a>同义词库文件的位置  
 同义词库文件的默认位置为：  
  
`<SQL_Server_data_files_path>\MSSQL13.MSSQLSERVER\MSSQL\FTDATA\`
  
该默认位置包含以下文件：  
  
-   **特定于语言的** 同义词库文件  

    安装程序将在上述位置安装空同义词库文件。 对于每种支持的语言，将提供一个单独的文件。 系统管理员可以自定义这些文件。  
  
    同义词库文件的默认文件名采用以下格式：  
  
    `'ts' + <three-letter language-abbreviation> + '.xml'`
  
    对于给定的语言，同义词库文件位置是在注册表的以下值中指定的：
     
    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<instance-name>\MSSearch\<language-abbrev>`
  
-   **全局** 同义词库文件  
  
    空的全局同义词库文件 tsGlobal.xml。  

### <a name="change-the-location-of-a-thesaurus-file"></a>更改同义词库文件的位置 
可通过更改同义词库文件的注册表项来更改其位置和名称。 对于每种语言，同义词库文件位置是在注册表的以下值中指定的：  
  
`HKLM\SOFTWARE\Microsoft\Microsoft SQL Server\<instance name>\MSSearch\Language\<language-abbreviation>\TsaurusFile`
  
 全局同义词库文件对应于 LCID 为 0 的非特定语言。 此值只能由管理员更改。  

##  <a name="how-full-text-queries-use-the-thesaurus"></a><a name="how_queries_use_tf"></a>全文查询如何使用同义词库  
同义词库查询同时使用特定于语言的同义词库和全局同义词库。
1.  首先，查询查找特定于语言的文件，并加载该文件以进行处理（除非已加载了该文件）。 该查询将进行扩展以包含同义词库文件中的扩展集和替换集规则所指定的特定于语言的同义词。 
2.  然后，对全局同义词库重复执行这些步骤。 但是，如果字词已经是特定于语言的同义词库文件中的匹配项的一部分，则不会在全局同义词库中对该字词再次进行匹配。  

##  <a name="structure-of-a-thesaurus-file"></a><a name="structure"></a>同义词库文件的结构  
 每个同义词库文件都定义了一个 ID 为 `Microsoft Search Thesaurus` 的 XML 容器，以及一个包含示例同义词库的注释 `<!--`...`-->`。 同义词库是在 `<thesaurus>` 元素中定义的，其中包含定义标注字符设置、扩展集和替换集的子元素的示例。

典型的空同义词库文件包含以下 XML 文本：  
  
```xml  
<XML ID="Microsoft Search Thesaurus">  
  
<!--  Commented out  
  
    <thesaurus xmlns="x-schema:tsSchema.xml">  
<diacritics_sensitive>0</diacritics_sensitive>  
        <expansion>  
            <sub>Internet Explorer</sub>  
            <sub>IE</sub>  
            <sub>IE5</sub>  
        </expansion>  
        <replacement>  
            <pat>NT5</pat>  
            <pat>W2K</pat>  
            <sub>Windows 2012</sub>  
        </replacement>  
        <expansion>  
            <sub>run</sub>  
            <sub>jog</sub>  
        </expansion>  
    </thesaurus>  
-->  
</XML>  
```

### <a name="xml-structure-of-an-expansion-set"></a><a name="expansion"></a> XML structure of an expansion set  
  
 每个扩展集包含在 `<expansion>` 元素中。 在此元素中，可以在 `<sub>` 元素中指定一个或多个替换项。 在扩展集中，可以指定一组互为同义词的替换项。  
  
 例如，可以编辑扩展部分，以将替换项“writer”、“author”和“journalist”视为同义词。 在一个替换项中包含匹配项的全文搜索查询将展开以包括扩展集中指定的所有其他替换项。 因此，在前面的示例中，如果对单词“author”发出 FORMS OF THESAURUS 或 FREETEXT 查询，全文搜索还会返回包含单词“writer”和“journalist”的搜索结果。  
  
对于上述示例，您将看到如下所示的扩展集部分：  
  
```xml  
<expansion>  
        <sub>writer</sub>  
        <sub>author</sub>  
        <sub>journalist</sub>  
</expansion>  
```  
  
### <a name="xml-structure-of-a-replacement-set"></a><a name="replacement"></a> XML structure of a replacement set  
  
每个替换集包含在 `<replacement>` 元素中。 在此元素中，可以在 `<pat>` 元素中指定一个或多个模式，还可以在 `<sub>` 元素中指定零个或多个替换项，每个替换项对应于一个同义词。 可以指定要由替换集替换的模式。 模式和替换项可以包含一个或一组单词。 如果没有为某个模式指定任何替换项，则会导致从用户查询中删除该模式。  
  
例如，假设您希望运行用替换项“Windows Server 2012”或“Windows 8.0”替换“Win8”模式的查询。 如果对“Win8”运行全文查询，全文搜索仅返回包含“Windows Server 2012”或“Windows 8.0”的搜索结果。 它不会返回包含“Win8”的结果。 这是因为模式“Win8”已经被模式“Windows Server 2012”和“Windows 8.0”替换。  
  
对于上述示例，您将看到如下所示的替换集部分：  
  
```xml  
<replacement>  
        <pat>Win8</pat>  
        <sub>Windows Server 2012</sub>  
        <sub>Windows 8.0</sub>  
</replacement>  
```  
  
如果有两个具有相似模式的替换集可匹配，则优先选用两者中的更长者。 例如，如果对“Internet Explorer online community”运行 FORMS OF THESAURUS 查询，并且有以下替换集，则“Internet Explorer”替换集的优先级别高于“Internet”的优先级别。 因此，查询将作为“IE online community”或“IE 9 online community”进行处理。  
  
```xml  
<replacement>  
         <pat>Internet</pat>  
         <sub>intranet</sub>  
</replacement>  
```  
  
以及  
  
```xml  
<replacement>  
         <pat>Internet Explorer</pat>  
         <sub>IE</sub>  
         <sub>IE 9</sub>  
</replacement>  
```

### <a name="xml-structure-of-the-diacritics-setting"></a>标注字符设置的 XML 结构  
  
同义词库的标注字符设置是在单个 `<diacritics_sensitive>` 元素中指定的。 此元素包含一个控制重音区分设置的整数值，如下所示：  
  
|标注字符设置|值|XML|  
|------------------------|-----------|---------|  
|不区分重音|0|`<diacritics_sensitive>0</diacritics_sensitive>`|  
|区分重音|1|`<diacritics_sensitive>1</diacritics_sensitive>`|  
  
> [!NOTE]  
>  只能在文件中应用一次此设置，它适用于文件中的所有搜索模式。 不能为各个模式单独指定此设置。  

##  <a name="edit-a-thesaurus-file"></a><a name="editing"></a>编辑同义词库文件  
可通过编辑给定语言的同义词库文件（XML 文件）来配置该语言的同义词库。 在安装过程中，将安装空的同义词库文件，它们仅包含 `<xml>` 容器和注释掉的示例 `<thesaurus`> 元素。 为了使查找同义词的全文搜索查询正常工作，必须创建一个定义一组同义词的实际 `<thesaurus`> 元素。 可以定义两种形式的同义词，即扩展集和替换集。  

### <a name="edit-a-thesaurus-file"></a>编辑同义词库文件  
  
1.  使用记事本或其他文本编辑器打开同义词库文件。  
  
2.  如果第一次编辑同义词库文件，请分别删除位于文件开头和末尾的以下注释行：  
  
    ```xml  
    <!--Commented out  
    -->  
    ```  
  
3.  添加、修改或者删除替换集或扩展集。  
  
4.  保存文件并关闭记事本。  
  
5.  使用 [sp_fulltext_load_thesaurus_file](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md) 将同义词库文件内容加载到 tempdb 中，并指定对应于同义词库文件语言的本地标识符 (LCID)。 例如，对于英语同义词库文件 tsenu.xml，对应的 LCID 为 1033。  
  
    ```sql  
    USE AdventureWorks;  
    EXEC sys.sp_fulltext_load_thesaurus_file 1033;  
    GO
    ```

### <a name="recommendations-for-editing-thesaurus-files"></a>有关编辑同义词库文件的建议  
  
 建议同义词库文件中的项不要包含任何特殊字符。 这是因为，断字符在遇到特殊字符时会出现不确定的行为。 如果同义词库项包含任何特殊字符，在全文查询中与该项一起使用的断字符会出现不确定的行为。  
  
 建议不要在 `<sub>` 项中包含任何非索引字，因为全文检索中将省略非索引字。 查询将进行扩展以包含同义词库文件中的 `<sub>` 项，如果 `<sub>` 项包含非索引字，则会不必要地增加查询大小。  

### <a name="restrictions-for-editing-thesaurus-files"></a>编辑同义词库文件时的限制  
  
 下面的限制适用于编辑同义词库文件：  
  
-   只有系统管理员能够更新、修改或删除同义词库文件。  
  
-   使用文本编辑器工具编辑同义词库文件时，必须以 Unicode 格式保存这些文件，并且必须指定字节顺序标记。  
  
-   同义词库项不能为空或断字为空字符串。  
  
-   同义词库文件中的短语长度不得超过 512 个字符。  
  
-   同义词库不能在扩展集的 `<sub>` 项和替换集的 `<pat>` 元素中包含任何重复项。  

## <a name="see-also"></a>另请参阅  
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)     
 [sp_fulltext_load_thesaurus_file (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 [sys.dm_fts_parser (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)
 
