---
title: Reporting Services 2017 及更高版本的发行说明 | Microsoft Docs
description: 详细了解 SQL Server Reporting Services (SSRS) 版本 2017 及更高版本的更改。
ms.date: 10/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.reviewer: maggies
author: casualoak
ms.author: rhys
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 07e8b01d94d1ab9c3b9d19ab28d4ed4b3d595b2b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425102"
---
# <a name="release-notes-for-sql-server-reporting-services-ssrs-2017-and-later"></a>SQL Server Reporting Services (SSRS) 2017 及更高版本的发行说明

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../includes/ssrs-appliesto-2017-and-later.md)]

本文介绍版本 2017 及更高版本的 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] (SSRS) 的更改。

有关报表查看器控件的发行说明，请参阅[有关 SSRS 的 WebForms 和 WinForms 的报表查看器控件的发行说明](application-integration/release-notes-ssrs-application-integration.md)。

<!--
We are "standardizing" all our 'Release Notes' style articles:

  - File names:   'release-notes-[TechArea-Name].md'


  - Content format:   2-column tables.  No longer using bullet lists.

    - Ideally, the 'Details' column should contain a link to another SSRS documentation article wherein the particular issue fix is discussed in any way.  Or if there is more to say beyond one sentence, the other sentences of elaboration would go into the 'Details' column.


  - H2 header names are kept short, for better display.


  - Try to keep all Release Notes in one .md file.  Avoid having multiple R.N. files whose names differ only by version or date.

    - Seriously consider erasing info about ancient releases that are so old that nobody cares about them anymore.  If you really want to retain ancient info, consider doing so in an HTML comment at the end of the .md file, just in case a Microsoft employee needs to re-examine ancient info.  In any case, this file cannot get ever longer, and some deletion or hiding of oldest info must eventually occur.


  - Do use '::: moniker range=' zones when version 2017 is no longer the only version represented inside this .md file.

    - Use the '=' operator on the moniker, not the '>=' operator.
    - In contrast, in our 'Whats New' articles we do use the '>=', rather than '='.

GeneMi, DevOps = 1467988 (MsEng > TechnicalContent) , 2019/03/19
-->
## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

## <a name="15075454810-20200831"></a>15.0.7545.4810，2020/08/31 
（产品版本：15.0.1102.861）

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 安全更新  | &nbsp; |
| 受约束的注释附件支持不再允许 PDF 文档  | &nbsp; |
| 修复了当导出在名称中包含句点的报表时的文件名截断问题  | &nbsp; |
| 修复了与订阅相关的问题，以及导致无效日期格式错误的 zh-TW 区域性问题  | &nbsp; |
| 修复了某些报表中访问参数选项导致不定旋转的问题  | &nbsp; |
| 修复了与报表名称中的单引号相关的问题  | &nbsp; |
| 修复了 URL 访问中导致 FindString 找不到匹配项的问题  | &nbsp; |
| 修复了用于 PDF 导出的替换文本不能正确编码多字节字符的问题  | &nbsp; |
| 修复了线性元素下空图像的外观不满足需求的问题  | &nbsp; |
| 修复了 Web Edition 中自定义身份验证的错误不支持错误  | &nbsp; |
| 修复了屏幕阅读器读取 Tablix 中的额外行和列的问题  | &nbsp; |
| 修正了在缩放到整个页面时为匹配大小而导致图像截断的问题  | &nbsp; |
| 命令行升级不再需要 EULA 标志  | &nbsp; |

## <a name="150724337714-20191101"></a>15.0.7243.37714, 2019/11/01
（产品版本：15.0.1102.675）

初始版本。


## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

## <a name="1406001669-20200831"></a>14.0.600.1669，2020/08/31 

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 安全更新  | &nbsp; |
| 受约束的注释附件支持不再允许 PDF 文档  | &nbsp; |
| 修复了当导出在名称中包含句点的报表时的文件名截断问题  | &nbsp; |
| 修复了与订阅相关的问题，以及导致无效日期格式错误的 zh-TW 区域性问题  | &nbsp; |

## <a name="1406001572-20200406"></a>14.0.600.1572（2020 年 4 月 6 日） 

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| JQuery UI 已更新为 1.12  | &nbsp; |
| 修复了 URL 区分大小写的问题  | &nbsp; |
| 修复了在有多个参数时的参数位置问题  | &nbsp; |
| 修复了 FindString 在 HTML5 呈现中不起作用的问题  | &nbsp; |
| 为了解决 TLS 1.0/1.1 弃用问题，Analysis Services 客户端库已更新 | &nbsp; |

## <a name="1406001451-20191113"></a>14.0.600.1451（2019 年 11 月 13 日） 

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 安全更新 | &nbsp; |
| 启用快照时，分页报表不能正常使用筛选器参数  | &nbsp; |
| 具有浏览者角色和默认设置的用户无权下载 Excel 文件  | &nbsp; |
| 升级时从 SQL Server 2016 Reporting Services 升级到 Power BI 报表服务器失败 | &nbsp; |
| 从 SQL Server 2012 Reporting Services 升级后，订阅失败，并显示“邮件头中发现无效字符: ','”消息 | &nbsp; |
| 配置工具：在“数据库”部分中取消模式窗口会重新启动 Reporting Services 服务 | &nbsp; |
| Textbox 控件的 BorderStyle 属性表达式未呈现为 Excel 格式  | &nbsp; |
| 分页问题，可能会导致某些报表一直呈现相同页面而无法到达报表的最后一页 | &nbsp; |

## <a name="1406001274-20190701"></a>14.0.600.1274，2019/07/01

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 安全更新 | &nbsp; |
| 创建每周共享计划时无法选择工作日 | &nbsp; |
| 报表在 Word 格式中没有正确显示回车符 | &nbsp; |
| System Center Operations Manager (SCOM) 2019 不再适用于最新的 SSRS 2017 升级 | &nbsp; |
| 为共享数据集调用授权扩展插件时出错 | &nbsp; |
| 在 SSRS 2017 和 PBIRS 中，存储过程 GetAllProperties 的逻辑发生了变化，这导致 Web service 终结点 ReportingService2010.GetProperties 方法无法获取链接报表的任何数据 | &nbsp; |
| 当单击网格中的项时，移动报表中的简单网格行标头将消失 | &nbsp; |
| 无法在数据驱动订阅参数中使用日期字段 | &nbsp; |
| 嵌套 tablix 在 SSRS 2016 和更高版本中显示小型字体或部分字体 | &nbsp; |
| 在具有不同区域设置的用户编辑订阅后，使用 DateTime 参数进行订阅时出错 | &nbsp; |
| 创建具有空传递扩展插件的数据驱动订阅失败，显示“出现传递错误” | &nbsp; |
| 以 Excel\Word 格式设置值时，URL 编码不正确 | &nbsp; |

## <a name="1406001109-20190212"></a>14.0.600.1109，2019/02/12

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 缓存报表快照计划在修改订阅后更改为“报表专用计划”。 | &nbsp; |
| rc:Toolbar=false 在 Express Edition 中不起作用。 | &nbsp; |
| 将分页报表导出为 PDF 时，一些泰语字符的呈现不正确。 | &nbsp; |
| 已完成的数据驱动订阅的通知期间出现死锁。 | &nbsp; |
| 使用 rc:Toolbar=False 参数时，嵌入图像在一些情况下不显示。 | &nbsp; |
| 无法为使用级联参数的报表创建数据驱动订阅。 | &nbsp; |
| 无法编辑配置间隔无效的订阅。 | &nbsp; |
| 安全更新。 | &nbsp; |
| 链接的报表 UI 不显示。 | &nbsp; |
| 一些嵌套 Tablix 控件的分页报表的字体不正确。 | &nbsp; |
| 向一些包含 tablix 数据区域的分页报表错误添加空格。 | &nbsp; |
| 扩展移动报表的简单数据网格时，标题行消失。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600906-20180912"></a>14.0.600.906，2018/09/12

已修复以下问题：

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 自定义身份验证未返回正确的 cookie 信息。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600892-20180831"></a>14.0.600.892，2018/08/31

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 当 rc:Toolbar=False 且它具有长文本时，矩形内的文本框导致矩形不沿垂直方向增长。 | &nbsp; |
| 如果 pageHeight 小于 0.5 英寸，则文本大小不会缩放。 | &nbsp; |
| 当 SSRS 目录数据库与 CRM 一起使用时，出现死锁。 | &nbsp; |
| 在报表中向下滚动时，垂直对齐的列标题不能正确显示。 | &nbsp; |
| 添加到 System Center Operations Manager 报表角色的用户已被阻止访问 SSRS Web 门户。 | &nbsp; |
| 泰语字符未正确导出到 PDF 中。 | &nbsp; |
| 浏览器角色行为更改。 | &nbsp; |
| rc:Toolbar=false 在 Express Edition 中不起作用。 | &nbsp; |
| 在参数提示区域缺少垂直滚动条。 | &nbsp; |
| 更新了移动报表运行时。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600744-20180425"></a>14.0.600.744，2018/04/25

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 创建后，数据驱动订阅页不会显示传递选项。 | &nbsp; |
| 将 SSRS 2012 升级到 SSRS 2017 会导致 RSManagement 每几秒钟就引发异常。 | &nbsp; |
| 无法更改 IE11 中多值参数的默认值。 | &nbsp; |
| 只要执行共享的计划，计划就为空。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600689-20180228"></a>14.0.600.689，2018/02/28

| 修复的问题 | 详细信息 |
| :---------- | :------ |
| 链接报表中的报表参数可见性在用户编辑它的属性后还原。 | &nbsp; |
| URL 参数 rc:Toolbar = false 在 Express edition 中不起作用。 | &nbsp; |
| 如果在将 CanGrow 属性设为 false 的文本框中使用表达式，会导致不显示值。 | &nbsp; |
| 为安装程序中的产品密钥添加了“了解更多”链接。  | &nbsp; |
| 使用自定义表单身份验证的 Web 门户忽略弹性到期 Cookie。 | &nbsp; |
| “导出到 Word”导致行高度不等（如果行内容为空的话）。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600594-20180109"></a>14.0.600.594，2018/01/09

已实现某些安全更新。

### <a name="140600490-20171101"></a>14.0.600.490，2017/11/01

已通过 SKU 升级解决问题。

## <a name="140600451-20170930"></a>14.0.600.451，2017/09/30

初始版本。

## <a name="next-steps"></a>后续步骤

[Reporting Services (SSRS) 中的新增功能](what-s-new-in-sql-server-reporting-services-ssrs.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)。
