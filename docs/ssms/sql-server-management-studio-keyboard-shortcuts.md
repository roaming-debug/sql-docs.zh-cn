---
title: SQL Server Management Studio 键盘快捷方式
description: SQL Server Management Studio 键盘快捷方式
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Environment.Keyboard
- VS.ToolsOptionsPages.Environment.Keyboard.Query_Shortcuts
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], shortcuts
- keyboard shortcuts [SQL Server Management Studio]
- menu shortcuts [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], keyboard shortcuts
- hot keys [SQL Server Management Studio]
- shortcuts [SQL Server Management Studio], keyboard shortcuts
- keyboard shortcuts [SQL Server Management Studio], list of shortcuts
- shortcuts [SQL Server Management Studio]
- accelerator keys
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/08/2021
ms.openlocfilehash: 95920a31fc4cd91ccba3f59363e510c4d914ca2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341030"
---
# <a name="sql-server-management-studio-keyboard-shortcuts"></a>SQL Server Management Studio 键盘快捷方式

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Management Studio (SSMS) 提供键盘快捷方式。 默认情况下，它使用 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] 方案，即使用基于 Visual Studio 的键盘快捷方式。 若要更改键盘方案或添加更多键盘快捷方式，请在“工具”菜单中选择“选项” 。 在 **“环境”** 下的 **“键盘”** 页中选择所需的键盘方案。

> [!NOTE]
> 若要仅显示标题，请选择此页顶部的“全部折叠”。

## <a name="menu-activation-keyboard-shortcuts"></a>菜单激活键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
| 移动到 SQL Server Management Studio 菜单栏 | ALT |
| 激活工具组件的菜单|Alt+连字符 |
| 显示上下文菜单|Shift+F10|
| 显示 **“新建文件”** 对话框以创建文件。 | Ctrl+N |
| 显示 **“新建项目”** 对话框以创建新项目|Ctrl+Shift+N|
| 显示 **“打开文件”** 对话框以打开现有文件|Ctrl+O<br /><br /> 或<br /><br /> CTRL+Shift+G|
| 显示 **“打开项目”** 对话框，用于打开现有项目|CTRL+SHIFT+O|
| 显示 **“添加新项”** 对话框，用于向当前项目添加新文件|Ctrl+Shift+A|
| 显示 **“添加现有项”** 对话框，用于向当前项目添加现有文件|SHIFT+ALT+A|
| 显示查询设计器|Ctrl+Shift+Q|
| 关闭菜单或对话框，取消操作|Esc|

## <a name="windows-management-and-toolbar-keyboard-shortcuts"></a>窗口管理和工具栏键盘快捷方式  

| 操作 | 快捷键 |
|--------|----------|
|关闭当前 MDI 子窗口|Ctrl+F4|
|关闭菜单或对话框，取消正在进行中的操作，或者将焦点放置于当前文档窗口|ESC|
|打印|Ctrl+P|
|退出|Alt + F4|
|切换全屏模式|Shift+Alt+Enter|
|关闭当前工具窗口|Shift+Esc|
|循环显示下一个 MDI 子窗口|Ctrl+F6|
|显示 IDE 导航器，并选中第一个文档窗口|Ctrl+Tab|
|循环显示上一个 MDI 子窗口|Ctrl+Shift+Tab|
|在编辑器处于代码视图或服务器代码视图中时将插入点移到位于代码编辑器顶部的下拉条|Ctrl+F2|
|移到当前工具窗口工具栏|Shift+Alt|
|显示 IDE 导航器，并选中第一个工具窗口|Alt+F7|
|移到下一个工具窗口|Alt+F6<br /><br /> 或<br /><br /> F6（在数据库引擎查询编辑器中）|
|移到上一个工具窗口|Shift+Alt+F7|
|移到单个文档的拆分窗格视图的下一个窗格|F6|
|移到上一个选定窗口|Shift+Alt+F6<br /><br /> 或<br /><br /> SHIFT+F6（在数据库引擎查询编辑器中）|
|移到单个文档的拆分窗格视图的上一个窗格|Shift+F6|
|显示停靠菜单|Alt+减号 (-)|
|显示一个弹出窗口，其中列出所有打开的窗口|Ctrl+Alt+向下键|
|打开一个新的查询编辑器窗口|Ctrl+O|
|显示对象资源管理器|F8|
|显示已注册的服务器|CTRL+ALT+G|
|显示模板资源管理器|Ctrl+Alt+T|
|显示解决方案资源管理器|Ctrl+Alt+L|
|显示摘要窗口|F7|
|显示属性窗口|F4|
|显示 **“输出”** 窗口|CTRL+ALT+O|
|显示 **“任务列表”** 窗口|CTRL+\\，T<br /><br /> 或<br /><br /> CTRL+\\，CTRL+T|
|在对象资源管理器的详细信息列表视图和对象资源管理器的详细信息属性窗格之间切换。|F6|
|控制拆分栏，拆分栏用于分隔对象资源管理器的详细信息列表视图和对象资源管理器的详细信息属性窗格，以便调整显示窗格的大小。|TAB，然后是上箭头或下箭头|
|显示工具箱|Ctrl+Alt+X|
|显示书签窗口|Ctrl+K、Ctrl+W|
|显示浏览器窗口|Ctrl+Alt+R|
|显示用于 HTML 设计器中 Web 服务器控件的公共命令的智能标记菜单|Shift+ALT+F10|
|显示“错误列表”窗口（仅限[!INCLUDE[tsql](../includes/tsql-md.md)] 编辑器）|CRTL+\\，CTRL+E<br /><br /> 或<br /><br /> CTRL+\\，E|
|移到“错误列表”窗口中的下一项（仅限[!INCLUDE[tsql](../includes/tsql-md.md)] 编辑器）|Ctrl+Shift+F12|
|显示查看历史记录中的上一页。 仅在 Web 浏览器窗口中可用|Alt+向左键|
|显示查看历史记录中的下一页。 仅在 Web 浏览器窗口中可用|Alt+向右键|

## <a name="cursor-movement-keyboard-shortcuts"></a>光标移动键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|左移光标|向左键|
|右移光标|向右键|
|向上移动光标|向上键|
|下移光标|向下键|
|将光标移到行首|Home|
|将光标移到行尾|End|
|将光标移到文档开头|Ctrl+Home|
|将光标移到文档末尾|Ctrl+End|
|将光标上移一个屏幕|Page Up|
|将光标下移一个屏幕|Page Down|
|将光标右移一个字|Ctrl+向右键|
|将光标左移一个字|Ctrl+向左键|
|将光标返回到最后一项。|Shift+F8|
|将光标移到文档顶部|Ctrl+PAGE UP|
|移到文档中的上一个选项卡|Ctrl+PAGE UP|
|将光标移到文档底部|Ctrl+PAGE DOWN|
|移到文档中的下一个选项卡|Ctrl+PAGE DOWN|

## <a name="text-selection-keyboard-shortcuts"></a>文本选择键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|选择从光标所在位置到文档开头处的文本|Ctrl+Shift+HOME|
|选择从光标所在位置到文档末尾的文本|Ctrl+Shift+End|
|选择从光标到当前行首的文本|Shift+Home|
|将光标到当前行的开头并扩展列选择范围|Shift+Alt+HOME|
|选择从光标到当前行尾的文本|Shift+End|
|将光标到行的末尾并扩展列选择范围。|Shift+Alt+END|
|从光标开始向下逐行选择文本|Shift+向下键|
|将光标下移一行并扩展列选择范围|Shift+Ctrl+Shift+Del|
|将光标左移一个字符，并且扩展选择范围|SHIFT + 向左键|
|将光标左移一个字符，并且扩展列选择范围|Shift+Alt+向左键|
|将光标右移一个字符，并且扩展选择范围|SHIFT + 向右键|
|将光标右移一个字符，扩展列选择范围|Shift+Alt+向右键|
|从光标开始向上逐行选择文本|Shift+向上键|
|将光标上移一行，扩展选择范围|Shift+Alt+向上键|
|将选定范围向上扩展一页|Shift+Page Up|
|将选择范围向下扩展一页|Shift+Page Down|
|选择整个当前文档|CTRL + A|
|选择包含光标的字或最近的字|Ctrl+W|
|选择编辑器中的当前位置，返回编辑器中的上一位置|Ctrl+=|
|将选择范围扩展到当前窗口的顶部|Ctrl+Shift+Page Up|
|将光标移到视图中的最后一行，扩展选择范围|Ctrl+Shift+Page Down|
|将选定范围向右扩展一个字|Ctrl+Shift+向右键| 
|将所选内容向左扩展一个单词|Ctrl+Shift+向左键|
|将光标右移一个字，扩展选择范围|Ctrl+Shift+Alt+向右键|
|将光标左移一个字，扩展选择范围|Ctrl+Shift+Alt+向左键|
|将光标移至下一个大括号，扩展选择范围|Ctrl+Shift+]|
|选择从光标的当前位置到向后导航（Ctrl+减号 (-)）位置的文本|Ctrl+等号 (=)|
|返回到导航历史记录中的上一个文档或窗口|Ctrl+减号 (-)|
|前进到导航历史记录中的下一个文档或窗口|Ctrl+Shift+减号 (-)|
|交换当前选择范围的定位点和端点|Ctrl+K、Ctrl+A|
|将光标移到视图中的第一行，扩展选择范围|Ctrl+Shift+Page Up|
|将光标移到视图中的最后一行，扩展选择范围|Ctrl+Shift+Page Down|

## <a name="bookmark-keyboard-shortcuts"></a>书签键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|在当前行设置或删除书签|Ctrl+K、Ctrl+K|
|下一个书签|Ctrl+K、Ctrl+N|
|如果当前书签位于某一文件夹中，则移到该文件夹中的下一个书签。 将跳过该文件夹之外的书签。<br /><br /> 如果书签不在某一文件夹中，则移到同一级别的下一个书签。<br /><br /> 如果“书签”窗口包含文件夹，则跳过文件夹中的书签。|Ctrl+Shift+K、Ctrl+Shift+N|
|上一个书签|Ctrl+K、Ctrl+P|
|如果当前书签位于某一文件夹中，则移到该文件夹中的下一个书签。 将跳过该文件夹之外的书签。<br /><br /> 如果书签不在某一文件夹中，则移到同一级别的下一个书签。<br /><br /> 如果“书签”窗口包含文件夹，则跳过文件夹中的书签。|Ctrl+Shift+K、Ctrl+Shift+P|
|清除书签|Ctrl+K、Ctrl+L|

## <a name="tree-control-keyboard-shortcuts"></a>树控件键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|折叠树节点|-（位于数字键盘）|
|展开所有树节点|*（位于数字键盘）|
|在窗口中向上滚动树控件|CTRL + 向上箭头|
|在窗口中向下滚动树控件|Ctrl+向下键|

## <a name="code-editor-keyboard-shortcuts"></a>代码编辑器键盘快捷方式

并非所有类型的代码编辑器都实现了所有快捷方式。
 
| 操作 | 快捷键 |
|--------|----------|
|切换全屏显示|Shift+Alt+Enter|
|向上滚动一行文本|CTRL + 向上箭头|
|向下滚动一行文本|CTRL + 向下箭头|
|撤消上一个编辑操作|CTRL+Z<br /><br /> 或<br /><br /> Alt+退格键| 
|恢复上一个撤消的操作|Ctrl+Shift+Z<br /><br /> 或<br /><br /> Ctrl+Y<br /><br /> 或<br /><br /> Alt+Shift+Backspace|
|保存选定项|Ctrl+S|
|全部保存|Ctrl+Shift+S|
|关闭|Ctrl+F4|
|打印|Ctrl+P|
|退出|Alt + F4|
|在浏览器中打开当前文件|Ctrl+Shift+W|
|删除当前文件中的所有文本|Ctrl+Shift+Del|
|显示 **“转到行”** 对话框|Ctrl+G|
|显示 **“导航到”** 对话框。|Ctrl+加号 (+)|
|增加行缩进|Tab|
|减少行缩进|Shift+Tab|
|将选定文本设为大写|Ctrl+Shift+U|
|将选定文本设为小写|CTRL+U|
|将选定文本设为注释|Ctrl+K、Ctrl+C|
|取消注释所选文本|Ctrl+K、Ctrl+U|
|用当前连接打开一个新查询|Ctrl+N|
|在对象资源管理器中打开数据库|Alt+F8|
|指定模板参数的值|Ctrl+Shift+M|
|运行查询编辑器的选定部分；如果未选择任何内容，则分析整个查询编辑器|F5<br /><br /> 或<br /><br /> Ctrl+Shift+E|
|分析查询编辑器的选定部分；如果未选择任何内容，则分析整个查询编辑器|Ctrl+F5|
|显示估计的执行计划|Ctrl+Shift+Alt+L|
|取消正在执行的查询|Alt+Break|
|在查询输出中包括实际执行计划|Ctrl+Shift+Alt+M|
|以网格方式输出结果|Ctrl+Shift+D|
|以文本格式输出结果|CTRL+T|
|将结果输出到文件|Ctrl+Shift+T|
|显示或隐藏查询结果窗格|Ctrl+R|
|显示查询结果窗格|Ctrl+Shift+Alt+R|
|在查询和结果窗格之间切换|F6|
|将结果网格和标题复制到剪贴板|Ctrl+Shift+C|
|移到 SSMS 中的下一个活动窗口|Alt+F6|
|打开 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|Ctrl+Alt+P|
|从查询编辑器窗口显示“查询设计器”对话框|Ctrl+Shift+Q|
|运行 **sp_help** 系统存储过程|Alt+F1|
|运行 **sp_who** 系统存储过程|CTRL+1|
|运行 **sp_lock** 系统存储过程|Ctrl+2|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|CTRL+3|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+4|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+5|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+6|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+7|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+7|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+8|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+9|
|运行在 **“工具”**、 **“选项”**、 **“键盘”**、 **“查询快捷方式”** 对话框中为此快捷方式配置的存储过程|Ctrl+0|

## <a name="text-manipulation-in-code-editor-keyboard-shortcuts"></a>代码编辑器中的文本操作键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|插入新行|Enter 或 Shift+Enter|
|交换光标两侧的字符（不适用于 SQL 编辑器。）|CTRL+T|
|删除光标右侧一个字符|DELETE|
|删除光标左侧一个字符|Backspace<br /><br /> 或<br /><br /> Shift+<br /><br /> Backspace|
|删除选定内容中的空白，或者删除光标旁的空白（如果不存在选定内容）|Ctrl+K、C|
|插入编辑器配置的空格数|Tab|
|在光标上方插入一个空行|Ctrl+Enter|
|在光标下方插入一个空行|Ctrl+Shift+Enter|
|将选定文本转换为小写|CTRL+SHIFT+L|
|将选定文本转换为大写|Ctrl+Shift+U|
|在插入模式和改写模式间切换|INSERT|
|将选定行左移一个制表位|Shift+Tab|
|删除光标右侧的字|Ctrl+Delete|
|删除光标左侧的字|Ctrl+Backspace|
|转置光标两侧的字（不适用于 SQL 编辑器。）|
|将包含光标的行向下移到下一行|Shift+Alt+T|
|为在 **“选项”** 对话框的 **“文本编辑器”** 部分中语言的 **“格式”** 窗格上指定的语言应用缩进和空间格式设置。 仅在编辑器中可用。|Ctrl+K、Ctrl+D|
|基于代码周围的行，正确缩进所选代码行|Ctrl+K、Ctrl+F|
|在当前行设置或删除快捷方式|Ctrl+K、Ctrl+H|
|从当前行中删除注释语法|Ctrl+K、Ctrl+U|
|显示或隐藏空格和制表符|Ctrl+R、Ctrl+W|
|启用或禁用编辑器中的自动换行|Alt+F、Ctrl+W|
|折叠所有大纲区域，只显示层次结构中最外面的组|Ctrl+M、Ctrl+A|
|折叠当前选定的大纲区域|Ctrl+M、Ctrl+S|
|展开页上的所有大纲区域|Ctrl+M、Ctrl+X|
|展开当前选定的大纲区域|Ctrl+M、Ctrl+E|
|折叠现有的大纲区域|Ctrl+M、Ctrl+O|
|隐藏选定的文本。 信号图标标记隐藏的文本的位置|Ctrl+M、Ctrl+H|
|在隐藏状态和显示状态之间切换以前标记为隐藏的所有文本选择。|Ctrl+M、Ctrl+L|
|在隐藏状态和显示状态之间切换当前选择的隐藏的文本选择|Ctrl+M、Ctrl+M|
|删除文档中的所有大纲信息|Ctrl+M、Ctrl+P|
|删除当前选定的区域的大纲信息|Ctrl+M、Ctrl+U|

## <a name="microsoft-intellisense-keyboard-shortcuts"></a>Microsoft IntelliSense 键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|列出成员|Ctrl+J|
|完成单词|Ctrl+空格键<br /><br /> 或<br /><br /> Alt+向右键|
|显示快速信息|Ctrl+K、Ctrl+I|
|显示参数信息|Ctrl+Shift+空格键|
|复制参数提示|Ctrl+Shift+Alt+C|
|粘贴参数提示|Ctrl+Shift+Alt+P|
|在语法对之间跳转|CTRL+]|
|启动代码段选择器|Ctrl+K、Ctrl+X|
|刷新本地缓存|Ctrl+Shift+R|
|启动外侧代码段选择器|Ctrl+K、Ctrl+S|
|显示代码段管理器|Ctrl+K、Ctrl+B|
|将 IntelliSense 筛选级别从 **“公共”** 选项卡更改为 **“所有”** 选项卡。|Alt+加号 (+)|
|将 IntelliSense 筛选级别从 **“所有”** 选项卡更改为 **“公共”** 选项卡。|Alt+句点 (.)|

## <a name="document-window-and-browser-keyboard-shortcuts"></a>文档窗口和浏览器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|切换全屏显示模式|Shift+Alt+Enter|
|移到文档拆分窗格视图的下一个窗格|F6|
|移到编辑器或设计器中的上一个文档|Ctrl+Shift+F6<br /><br /> Ctrl+Shift+Tab|
|移到拆分窗格视图中的上一个文档窗格|Shift+F6|
|后退，显示查看历史记录中的上一页|Alt+向左键|
|前进，显示查看历史记录中的下一页|Alt+向右键|
|关闭菜单或对话框，取消正在进行中的操作，或者将焦点放置于当前窗口中|ESC|

## <a name="solution-explorer-keyboard-shortcuts"></a>解决方案资源管理器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|显示解决方案资源管理器|Ctrl+Alt+L|
|显示 **“新建文件”** 对话框，用于创建新文件|Ctrl+N|
|显示 **“新建项目”** 对话框以创建新项目|Ctrl+Shift+N|
|显示 **“打开文件”** 对话框以打开现有文件|Ctrl+O|
|更改所选对象的名称|F2|

## <a name="help-and-books-online-keyboard-shortcuts"></a>帮助和联机丛书键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|帮助|F1<br /><br /> 或<br /><br /> Shift+F1|
|显示 SQL Server 联机丛书|Ctrl+F1|
|打开帮助库管理器|Ctrl+Alt+F1|
|显示 SQL Server 资源中心网页|Ctrl+Alt+F2|
|显示当前编辑器窗口的帮助|Shift+F1|

## <a name="search-keyboard-shortcuts"></a>搜索键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|显示 **“查找”** 对话框|Ctrl+F|
|显示所选符号的定义。|F12|
|显示所选符号的引用列表。|Shift+F12|
|显示 **“替换”** 对话框|Ctrl+H|
|启动渐进式搜索。 键入要搜索的字符，或者按 Ctrl+I 搜索上次搜索的字符|CTRL+I|
|查找上一个搜索文本的下一个匹配项|F3|
|查找搜索文本的上一个匹配项|SHIFT+F3|
|查找当前选定文本的下一个匹配项|Ctrl+F3|
|查找当前选定文本的上一个匹配项|Ctrl+Shift+F3|
|显示 **“在文件中替换”** 对话框|Ctrl+Shift+H|
|反向渐进式搜索，从文件末尾搜索到开头|Ctrl+Shift+I|
|选中或清除 **“查找和替换”** 中的 **“向上搜索”** 选项|Alt+F3、B|
|停止 **“在文件中查找”** 搜索|Alt+F3、S|
|选中或清除 **“查找和替换”** 中的 **“全字匹配”** 选项|Alt+F3、W|
|选择或清除 **“查找和替换”** 中的 **“通配符”** 选项|Alt+F3、P|
|将插入号放置于标准工具栏的“查找/命令”框|CTRL+/|

## <a name="cut-and-paste-keyboard-shortcuts"></a>剪切和粘贴键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|剪切（删除当前选定项并将其放入剪贴板）|Ctrl+X|
|剪切所有所选行，或者剪切当前行（如果没有选定任何内容）。|Ctrl+L<br /><br /> 或<br /><br /> CTRL+SHIFT+L|
|复制到剪贴板|Ctrl+C<br /><br /> 或<br /><br /> Ctrl+Insert|
|在插入点从剪贴板粘贴|Ctrl+V<br /><br /> 或<br /><br /> Shift+Insert|
|从剪贴板环在插入点处粘贴某一项并且自动选择粘贴的项|Ctrl+Shift+V|

## <a name="activity-monitor-keyboard-shortcuts"></a>活动监视器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|启动活动监视器|Ctrl+Alt+A|
|关闭活动监视器|Ctrl+F4|
|刷新|F5|
|筛选监视器显示|Ctrl+Shift+F|
|循环切换面板|F6|
|展开或折叠选定的窗格|Ctrl 和 + 或 -|
|展开或折叠所有窗格|+ 或 -|
|复制网格中全部选定的行|Ctrl+C|
|复制单元|Ctrl+Shift+C|
|下拉以便在网格中筛选|Alt+向下键|
|在活动监视器中上下滚动|Ctrl+Alt+向上键/向下键|

## <a name="replication-monitor-keyboard-shortcuts"></a>复制监视器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|刷新|F5|
|从网格打开详细信息窗口|Enter|

## <a name="replication-conflict-viewer-keyboard-shortcuts"></a>复制冲突查看器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|定义筛选器|F6|
|应用筛选器|F7|
|显示所有列|F8|

## <a name="query-designer-keyboard-shortcuts"></a>查询设计器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|取消或停止当前正在运行的查询|CTRL+T|
|显示 **“查询设计器”** 的关系图窗格|CTRL+1|
|显示 **“查询设计器”** 的条件窗格|Ctrl+2|
|显示 **“查询设计器”** 的 SQL 窗格|CTRL+3|
|显示 **“查询设计器”** 的结果窗格|Ctrl+4|
|运行 **“查询设计器”** 中指定的查询|Ctrl+R|
|在处于结果窗格中时，将焦点移到停靠在设计器底部的工具条上|Ctrl+G|
|在 **“查询设计器”** 中启用 JOIN 模式|Ctrl+Shift+J|

## <a name="designer-keyboard-shortcuts"></a>设计器键盘快捷方式

| 操作 | 快捷键 |
|--------|----------|
|在设计图面上以 8 为增量向下移动所选控件|向下键|
|在设计图面上以 8 为增量向左移动所选控件|向左键|
|在设计图面上以 8 为增量向右移动所选控件|向右键|
|在设计图面上以 8 为增量向上移动所选控件|向上键|
|以 8 为增量增加所选控件的高度|SHIFT + 向下键|
|以 8 为增量减少所选控件的宽度|SHIFT + 向左键|
|以 8 为增量增加所选控件的宽度|SHIFT + 向右键|
|以 8 为增量减少所选控件的高度|SHIFT + 向上键|
|移到页面上的下一个控件|Tab|
|移到页面上的上一个控件|Shift+Tab|
|在设计图面上显示网格|Enter|

## <a name="see-also"></a>另请参阅

- [自定义菜单和快捷键](customize-menus-and-shortcut-keys.md)