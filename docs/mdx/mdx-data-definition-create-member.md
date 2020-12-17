---
description: MDX 数据定义 - CREATE MEMBER
title: " (MDX) 创建成员语句 |Microsoft Docs"
ms.date: 07/22/2020
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 878d189aba259e5b69f5c27dbbc8b80b3f7f880b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642974"
---
# <a name="mdx-data-definition---create-member"></a>MDX 数据定义 - CREATE MEMBER


  创建计算成员。  
  
## <a name="syntax"></a>语法  
  
```  
  
CREATE [ SESSION ] [HIDDDEN] [ CALCULATED ] MEMBER CURRENTCUBE | Cube_Name.Member_Name   
   AS MDX_Expression  
      [,Property_Name = Property_Value, ...n]  
......[,SCOPE_ISOLATION = CUBE]  
```  
  
## <a name="arguments"></a>自变量  
 *Cube_Name*  
 一个有效的字符串表达式，用于提供要创建成员的多维数据集的名称。  
  
 *Member_Name*  
 提供成员名称的有效字符串表达式。 指定完全限定名称以在 Measures 维度之外的维度中创建成员。 如果不提供完全限定成员名称，将在 Measures 维度中创建成员。  
  
 *MDX_Expression*  
 有效的多维表达式 (MDX)。  
  
 *Property_Name*  
 提供计算成员属性名称的有效字符串。  
  
 *Property_Value*  
 定义计算成员属性值的有效标量表达式。  
  
## <a name="remarks"></a>备注  
 CREATE MEMBER 语句定义的计算成员在整个会话期间均可用，因此可在会话期间用于多个查询。 有关详细信息，请参阅 [&#40;MDX&#41;创建 Session-Scoped 计算成员 ](/analysis-services/multidimensional-models/mdx/mdx-calculated-members-session-scoped-calculated-members)。  
  
 还可以定义供一个查询使用的计算成员。 若要定义只供一个查询使用的计算成员，请在 SELECT 语句中使用 WITH 子句。 有关详细信息，请参阅 [&#40;MDX&#41;创建 Query-Scoped 计算成员 ](/analysis-services/multidimensional-models/mdx/mdx-calculated-members-query-scoped-calculated-members)。  
  
 *Property_Name* 可以引用标准或可选的计算成员属性。 标准成员属性在本主题后面列出。 使用 CREATE MEMBER 而不使用 **session** 值创建的计算成员具有会话作用域。 另外，计算成员定义中的字符串用双引号分隔。 这不同于 OLE DB 定义的方法，后者指定字符串应由单引号分隔。  
  
 指定当前连接的多维数据集以外的多维数据集将导致错误。 因此，应使用 CURRENTCUBE 来代替多维数据集名称，以表示当前的多维数据集。  
  
 有关 OLE DB 定义的成员属性的详细信息，请参阅 OLE DB 文档。  
  
## <a name="scope"></a>范围  
 计算成员可出现在下表列出的一个作用域中。  
  
 查询范围  
 计算成员的可见性和生存期限制在查询中。 计算成员在单独的查询中定义。 查询作用域将覆盖会话作用域。 有关详细信息，请参阅 [&#40;MDX&#41;创建 Query-Scoped 计算成员 ](/analysis-services/multidimensional-models/mdx/mdx-calculated-members-query-scoped-calculated-members)。  
  
 会话范围  
 计算成员的可见性和生存期限制在创建计算成员时所在的会话中。 如果在计算成员上发出 DROP MEMBER 语句， (生存期小于会话持续时间。 ) CREATE MEMBER 语句将创建具有会话作用域的计算成员。  
  
### <a name="scope-isolation"></a>作用域隔离  
 当多维数据集多维表达式 (MDX) 脚本包含计算成员时，默认情况下会在解析任何会话范围的计算和解析任何查询定义的计算之前对计算成员进行解析。  
  
> [!NOTE]  
>  在某些情况下， [聚合 (mdx) ](../mdx/aggregate-mdx.md) 函数和 [VisualTotals (mdx) ](../mdx/visualtotals-mdx.md) 函数不会表现出这种行为。  
  
 这种行为允许一般客户端应用程序处理包含复杂计算的多维数据集，而不必考虑这些计算的特定实现。 但是，在某些情况下，您可能希望在多维数据集中的某些计算之前执行会话或查询范围的计算成员，并且 **聚合** 函数和 **VisualTotals** 函数均不适用。 若要完成此操作，应使用 SCOPE_ISOLATION 计算属性。  
  
#### <a name="example"></a>示例  
 在下面脚本示例所示的情况下，需要 SCOPE_ISOLATION 计算属性来生成正确的结果。  
  
 **多维数据集的 MDX 脚本：**  
  
```  
CREATE MEMBER CURRENTCUBE.Measures.ProfitRatio AS 'Measures.[Store Sales]/Measures.[Store Cost]', SOLVE_ORDER = 10  
```  
  
 **MDX 查询：**  
  
```  
WITH MEMBER [Customer].[Customers].[USA]. USAWithoutWA AS  
[Customer].[Customers].[Country].&[USA] - [Customer].[Customers].[State Province.&[WA], SOLVE_ORDER=5  
SELECT {USAWithoutWA} ON 0 FROM SALES  
WHERE ProfitRatio  
```  
  
 上一个查询的所需结果为 USA（不包括 WA）销售额与 USA（不包括 WA）商店成本的比率。 上一个查询未返回所需结果；返回的是 USA 比率减去 WA 比率，这是毫无意义的结果。 若要获得所需的结果，可以使用 SCOPE_ISOLATION 计算属性。  
  
 **使用 SCOPE_ISOLATION 计算属性的 MDX 查询：**  
  
```  
WITH MEMBER [Customer].[Customers].[USA]. USAWithoutWA AS  
[Customer].[Customers].[Country].&[USA] - [Customer].[Customers].[State Province.&[WA], SOLVE_ORDER=5  
,SCOPE_ISOLATION=CUBE  
SELECT {USAWithoutWA} ON 0 FROM SALES  
WHERE ProfitRatio  
```  
  
## <a name="standard-properties"></a>标准属性  
 每个计算成员都有一个默认属性集。 当客户端应用程序连接到时 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ，默认属性是受支持的，或者可在管理员选择的情况下受支持。  
  
 其他成员属性是否可用取决于多维数据集定义。 下列属性表示与多维数据集中的维度级别有关的信息。  
  
|属性标识符|含义|  
|-------------------------|-------------|  
|SOLVE_ORDER|在一个计算成员引用另一个计算成员（即计算成员相交）的情况下，计算成员的求解次序。|  
|FORMAT_STRING|一种在显示单元值时客户端应用程序可以使用的 Office 样式格式字符串。|  
|VISIBLE|指示计算成员在架构行集中是否可见的值。 可以使用 [AddCalculatedMembers](../mdx/addcalculatedmembers-mdx.md) 函数将可见计算成员添加到集。 非零值表示计算成员可见。 此属性的默认值为 *Visible*。<br /><br /> 不可见（此值设置为零时）的计算成员通常用作更复杂的计算成员中的中间步骤。 这些计算成员也可以由其他类型的成员（如度量值）引用。|  
|NON_EMPTY_BEHAVIOR|解析空单元时，用来确定计算成员的行为的度量值或集。<br /><br /> 警告此属性已弃用。 **\* \* \* \*** 避免将其设置。 有关详细信息，请参阅 [SQL Server 2014 中不推荐使用的 Analysis Services 功能](/previous-versions/sql/2014/analysis-services/deprecated-analysis-services-features-in-sql-server-2014?view=sql-server-2014&preserve-view=true) 。|  
|CAPTION|客户端应用程序用作成员标题的字符串。|  
|DISPLAY_FOLDER|标识客户端应用程序用来显示成员的显示文件夹路径的字符串。 文件夹级别的分隔符由客户端应用程序定义。 对于提供的工具和客户端 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ，) 的反斜杠 (\\ 是级别分隔符。 若要为已定义的成员提供多个显示文件夹，请使用分号 (; ) 来分隔文件夹。|  
|ASSOCIATED_MEASURE_GROUP|与此成员关联的度量值组的名称。|  
  
## <a name="see-also"></a>另请参阅  
 [DROP MEMBER 语句 &#40;MDX&#41;](../mdx/mdx-data-definition-drop-member.md)   
 [&#40;MDX&#41;更新成员语句 ](../mdx/mdx-data-definition-update-member.md)   
 [Mdx 数据定义语句 &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
