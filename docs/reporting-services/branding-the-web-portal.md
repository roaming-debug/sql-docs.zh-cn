---
title: 设置 Web 门户的品牌 | Microsoft Docs
ms.date: 07/01/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
description: 了解如何通过品牌包为业务打造网站门户的品牌外观。 品牌包的设计使你无需深入了解 CSS 知识便可打造专业品牌外观。
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2079a44a4b9f3977ae09f83685efa122e9467a96
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907195"
---
# <a name="branding-the-web-portal"></a>设置 Web 门户的品牌

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

你可以根据业务设置 Web 门户品牌，从而改变 Web 门户的外观。 使用品牌包可以实现此目的。 品牌包已经设计好，所以无需深入了解级联样式表即可进行创建。

<iframe width="560" height="315" src="https://www.youtube.com/embed/m08kLuofwFA" frameborder="0" allowfullscreen></iframe>

## <a name="creating-the-brand-package"></a>创建品牌包
  
Reporting Services 的品牌包由三项组成，被打包为一个 zip 文件。   
  
- colors.json  
- metadata.xml  
- logo.png（可选）  
  
文件必须采用上述名称。 zip 文件可以根据你的喜好进行命名。  
  
### <a name="metadataxml"></a>metadata.xml
  
metadata.xml 文件可以设置品牌包的名称，并且含有 colors.json 和 logo.png 文件的引用项。  
  
若要更改品牌包的名称，请更改 **SystemResourcePackage** 元素的 **name** 属性。  

```xml
    name="Multicolored example brand"  
```
  
你还可以选择在品牌包中包含一张徽标图片。 Contents 元素中会列出此选项。  
  
不使用徽标文件的示例。  

```xml
<Contents>  
    <Item key="colors" path="colors.json" />  
</Contents>  
```

使用徽标文件的示例。  
  
```xml
<Contents>  
    <Item key="colors" path="colors.json" />  
    <Item key="logo" path="logo.png" />  
</Contents>  
```

### <a name="colorsjson"></a>Colors.json
  
上传品牌包时，服务器会从 colors.json 文件中提取相应的名称/值对，并使用主 LESS 样式表 brand.less 将其合并。 然后对此 LESS 文件进行处理，并且将生成的 CSS 文件提供给客户端。 样式表中的所有颜色都遵循六个字符的颜色十六进制表示形式。  
  
LESS 样式表中包含了引用预定义 LESS 变量的块，如下所示。  

```css
/* primary buttons */   
.btn-primary {   
    color:@primaryButtonColor;   
    background-color:@primaryButtonBg;   
}  
```
  
与 CSS 语法类似，带有 @symbol 符号前缀的颜色值对 LESS 而言是唯一的。 这些颜色值是变量，变量值由 json 文件设置。  
  
例如，如果 colors.json 文件具有以下值。  

```json
"primary":"#009900",   
"primaryContrast":"#ffffff"   
```

经过处理的输出会查看“\@primaryButtonBg变量，并确保它映射到了名为“primary”的 json 属性（在此示例中是 #009900）。 这样才能输出正确的 CSS。  

```css
    .btn-primary {   
        color:#ffffff;   
        background-color:#009900;   
    }  
```

所有主要按钮将呈深绿色，而文本呈白色。  
  
Reporting Services 的 colors.json 文件有两种主要类别，项按这两种类别分组。  
  
- **接口** ：包括特定于 Reporting Services Web 门户的项。  
- **主题** ：包括特定于所创建的移动报表中的项。  
  
接口部细分为下列分组。  
  
|部分|说明|  
|---|---|  
|主|按钮和悬停颜色。|  
|辅助副本|标题栏、搜索栏、左侧菜单（如果显示）以及这些项的文本颜色|  
|中性第一级|主区域和报表区域背景。|  
|中性第二级|文本框和文件夹选项背景，以及设置菜单。|  
|中性第三级|站点设置背景。|  
|危险/警告/成功消息|这些消息的颜色。|  
|KPI|对好 (1)，中 (0)，中 (-1) 和无的颜色进行控制。|  
|||

第一次使用移动报表发布服务器连接到部署了品牌包的服务器时，主题会添加到应用右上角菜单中的可用主题中。  
  
![“选择调色板”对话框的屏幕截图。](../reporting-services/media/ssrsbrandingmobilereportpublisher.png)  
  
然后你就可以对所创建的任何移动报表使用该主题，即使报表不适用于已在其上部署主题的同一服务器。   
  
### <a name="using-a-logo"></a>使用徽标
  
如果包含了带品牌包的徽标，徽标将显示在 Web 门户中，并替代你在“站点设置”菜单中为 Web 门户设置的名称。  
  
包含徽标的文件必须使用 PNG 文件格式。 上传到服务器后文件大小会增大。 应该增大到大约 290px x 60px。  
   
## <a name="applying-the-brand-package-to-the-web-portal"></a>将品牌包应用于 Web 门户
  
若要添加、下载或删除品牌包，可以执行以下操作。  
  
1.  选择右上角的“齿轮”  。  
  
2.  选择“站点设置”。   
  
    ![“设置”下拉列表的屏幕截图，其中突出显示了“网站设置”选项。](../reporting-services/media/ssrsgearmenu.png)  
  
3.  选择“品牌”  。  
  
    ![“网站设置”页面的屏幕截图，其中选择了“品牌打造”选项。](../reporting-services/media/ssrsbranding.png) 
  
“当前已安装的品牌包”  会显示已上传的包的名称，或者显示“无”。  
  
“上传品牌包”  会将包应用到 Web 门户。 你将看到操作立即生效。  
  
你还可以 **下载** 或 **删除** 包。 删除包后，Web 门户会立即重置为默认品牌。  
  
## <a name="metadataxml-example"></a>metadata.xml 示例

```xml
<?xml version="1.0" encoding="utf-8"?>  
<SystemResourcePackage xmlns="http://schemas.microsoft.com/sqlserver/reporting/2016/01/systemresourcepackagemetadata"  
    type="UniversalBrand"  
    version="2.0.2"  
    name="Multicolored example brand"  
    >  
    <Contents>  
        <Item key="colors" path="colors.json" />  
        <Item key="logo" path="logo.png" />  
    </Contents>  
</SystemResourcePackage>  
```

## <a name="colorsjson-example"></a>colors.json 示例

```json
{  
    "name":"Multicolored example brand",  
    "version":"1.0",  
    "interface":{  
        "primary":"#b31e1e",  
        "primaryAlt":"#ca0806",  
        "primaryAlt2":"#621013",  
        "primaryAlt3":"#e40000",  
        "primaryAlt4":"#e14e50",  
        "primaryContrast":"#fff",  

        "secondary":"#042200",  
        "secondaryAlt":"#0f4400",  
        "secondaryAlt2":"#155500",  
        "secondaryAlt3":"#217700",  
        "secondaryContrast":"#49e63c",  

        "neutralPrimary":"#d8edff",  
        "neutralPrimaryAlt":"#c9e6ff",  
        "neutralPrimaryAlt2":"#aedaff",  
        "neutralPrimaryAlt3":"#88c8ff",  
        "neutralPrimaryContrast":"#0a2b4c",  

        "neutralSecondary":"#e9d8eb",  
        "neutralSecondaryAlt":"#d9badc",  
        "neutralSecondaryAlt2":"#b06cb5",  
        "neutralSecondaryAlt3":"#a75bac",  
        "neutralSecondaryContrast":"#250a26",  

        "neutralTertiary":"#f79220",  
        "neutralTertiaryAlt":"#f8a54b",  
        "neutralTertiaryAlt2":"#facc9b",  
        "neutralTertiaryAlt3":"#fce3c7",  
        "neutralTertiaryContrast":"#391d00",  

        "danger":"#ff0000",  
        "success":"#00ff00",  
        "warning":"#ff8800",  
        "info":"#00ff",  
        "dangerContrast":"#fff",  
        "successContrast":"#fff",  
        "warningContrast":"#fff",  
        "infoContrast":"#fff",  

        "kpiGood":"#4fb443",  
        "kpiBad":"#de061a",  
        "kpiNeutral":"#d9b42c",  
        "kpiNone":"#333",  
        "kpiGoodContrast":"#fff",  
        "kpiBadContrast":"#fff",  
        "kpiNeutralContrast":"#fff",  
        "kpiNoneContrast":"#fff"  
        },  
        "theme":{  
        "dataPoints":[  
            "#0072c6",  
            "#f68c1f",  
            "#269657",  
            "#dd5900",  
            "#5b3573",  
            "#22bdef",  
            "#b4009e",  
            "#008274",  
            "#fdc336",  
            "#ea3c00",  
            "#00188f",  
            "#9f9f9f"  
        ],  

        "good":"#85ba00",  
        "bad":"#e90000",  
        "neutral":"#edb327",  
        "none":"#333",  

        "background":"#fff",  
        "foreground":"#222",  
        "mapBase":"#00aeef",  
        "panelBackground":"#f6f6f6",  
        "panelForeground":"#222",  
        "panelAccent":"#00aeef",  
        "tableAccent":"#00aeef",  

        "altBackground":"#f6f6f6",  
        "altForeground":"#000",  
        "altMapBase":"#f68c1f",  
        "altPanelBackground":"#235378",  
        "altPanelForeground":"#fff",  
        "altPanelAccent":"#fdc336",  
        "altTableAccent":"#fdc336"  
    }  
}  
```

## <a name="next-steps"></a>后续步骤

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
