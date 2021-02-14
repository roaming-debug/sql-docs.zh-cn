---
title: 扩展性 API
description: 了解 Azure Data Studio 扩展性 API，扩展可使用它们来与 Azure Data Studio 的其他部分（例如对象资源管理器）进行交互。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: d795d96d47fa7f07744c492d8639cdecdc2e23fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052153"
---
# <a name="azure-data-studio-extensibility-apis"></a>Azure Data Studio 扩展性 API

Azure Data Studio 提供了一个 API，扩展可以使用该 API 与 Azure Data Studio 的其他部分（例如对象资源管理器）进行交互。 这些 API 可从 [`src/sql/azdata.d.ts`](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.d.ts) 文件中获得，如下所述。

## <a name="connection-management"></a>连接管理
`azdata.connection`

### <a name="top-level-functions"></a>顶级函数

- `getCurrentConnection(): Thenable<azdata.connection.Connection>` 根据活动编辑器或对象资源管理器选择获取当前连接。

- `getActiveConnections(): Thenable<azdata.connection.Connection[]>` 获取用户的所有活动连接的列表。 如果没有此类连接，则返回一个空列表。

- `getCredentials(connectionId: string): Thenable<{ [name: string]: string }>` 获取一个字典，其中包含与连接关联的凭据。 如果不以字典的形式获取凭据，这些凭据将作为选项字典的一部分在 `azdata.connection.Connection` 对象下返回，但会从该对象中去除。 

### `Connection`
- `options: { [name: string]: string }` 连接选项字典
- `providerName: string` 连接提供程序的名称（例如“MSSQL”）
- `connectionId: string` 连接的唯一标识符

### <a name="example-code"></a>示例代码
```
> let connection = azdata.connection.getCurrentConnection();
connection: {
    providerName: 'MSSQL',
    connectionId: 'd97bb63a-466e-4ef0-ab6f-00cd44721dcc',
    options: {
        server: 'mairvine-sql-server',
        user: 'sa',
        authenticationType: 'sqlLogin',
        ...
    },
    ...
}
> let credentials = azdata.connection.getCredentials(connection.connectionId);
credentials: {
    password: 'abc123'
}

```

## <a name="object-explorer"></a>“对象资源管理器”

`azdata.objectexplorer`


### <a name="top-level-functions"></a>顶级函数
- `getNode(connectionId: string, nodePath?: string): Thenable<azdata.objectexplorer.ObjectExplorerNode>` 获取与给定连接和路径对应的对象资源管理器节点。 如果未给定路径，则返回给定连接的顶级节点。 如果给定路径上没有节点，则返回 `undefined`。 注意：对象的 `nodePath` 由 SQL 工具服务后端生成，很难手动构造。 未来的 API 改进将允许根据你提供的有关节点的元数据来获取节点，例如名称、类型和架构。

- `getActiveConnectionNodes(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` 获取所有活动的对象资源管理器连接节点。

- `findNodes(connectionId: string, type: string, schema: string, name: string, database: string, parentObjectNames: string[]): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` 查找与给定元数据匹配的所有对象资源管理器节点。 `schema`、`database` 和 `parentObjectNames` 参数在不适用时应为 `undefined`。 `parentObjectNames` 是对象资源管理器中位于所需对象之上的非数据库父对象（从最高级别到最低级别）的列表。 例如，当使用连接 ID `connectionId` 搜索属于表“schema1.table1”和数据库“database1”的列“column1”时，请调用 `findNodes(connectionId, 'Column', undefined, 'column1', 'database1', ['schema1.table1'])`。 另请参阅 [Azure Data Studio 针对此 API 调用默认支持的类型的列表](https://github.com/Microsoft/azuredatastudio/wiki/Object-Explorer-types-supported-by-FindNodes-API)。

### <a name="objectexplorernode"></a>ObjectExplorerNode
- `connectionId: string` 节点所在的连接的 ID

- `nodePath: string` 节点的路径，用于调用 `getNode` 函数。

- `nodeType: string` 表示节点类型的字符串

- `nodeSubType: string` 表示节点子类型的字符串

- `nodeStatus: string` 表示节点状态的字符串

- `label: string` 对象资源管理器中显示的节点标签

- `isLeaf: boolean` 节点是否为叶节点，因此没有任何子级

- `metadata: azdata.ObjectMetadata` 描述此节点表示的对象的元数据

- `errorMessage: string` 节点处于错误状态时显示的消息

- `isExpanded(): Thenable<boolean>` 节点当前是否在对象资源管理器中展开

- `setExpandedState(expandedState: vscode.TreeItemCollapsibleState): Thenable<void>` 设置节点是展开还是折叠。 如果状态设置为“无”，则不会更改节点。

- `setSelected(selected: boolean, clearOtherSelections?: boolean): Thenable<void>` 设置是否选择了节点。 如果 `clearOtherSelections` 为 true，则在进行新选择时清除所有其他选择。 如果为 false，则保留所有现有选择。 如果 `selected` 为 true，则 `clearOtherSelections` 默认为 true；如果 `selected` 为 false，则默认为 false。

- `getChildren(): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` 获取此节点的所有子节点。 如果没有子节点，则返回一个空列表。

- `getParent(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` 获取此节点的父节点。 如果没有父节点，则返回 undefined。

### <a name="example-code"></a>示例代码

```cs
private async interactWithOENode(selectedNode: azdata.objectexplorer.ObjectExplorerNode): Promise<void> {
    let choices = ['Expand', 'Collapse', 'Select', 'Select (multi)', 'Deselect', 'Deselect (multi)'];
    if (selectedNode.isLeaf) {
        choices[0] += ' (is leaf)';
        choices[1] += ' (is leaf)';
    } else {
        let expanded = await selectedNode.isExpanded();
        if (expanded) {
            choices[0] += ' (is expanded)';
        } else {
            choices[1] += ' (is collapsed)';
        }
    }
    let parent = await selectedNode.getParent();
    if (parent) {
        choices.push('Get Parent');
    }
    let children = await selectedNode.getChildren();
    children.forEach(child => choices.push(child.label));
    let choice = await vscode.window.showQuickPick(choices);
    let nextNode: azdata.objectexplorer.ObjectExplorerNode = undefined;
    if (choice === choices[0]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Expanded);
    } else if (choice === choices[1]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Collapsed);
    } else if (choice === choices[2]) {
        selectedNode.setSelected(true);
    } else if (choice === choices[3]) {
        selectedNode.setSelected(true, false);
    } else if (choice === choices[4]) {
        selectedNode.setSelected(false);
    } else if (choice === choices[5]) {
        selectedNode.setSelected(false, true);
    } else if (choice === 'Get Parent') {
        nextNode = parent;
    } else {
        let childNode = children.find(child => child.label === choice);
        nextNode = childNode;
    }
    if (nextNode) {
        let updatedNode = await azdata.objectexplorer.getNode(nextNode.connectionId, nextNode.nodePath);
        this.interactWithOENode(updatedNode);
    }
}

vscode.commands.registerCommand('mssql.objectexplorer.interact', () => {
    azdata.objectexplorer.getActiveConnectionNodes().then(activeConnections => {
        vscode.window.showQuickPick(activeConnections.map(connection => connection.label + ' ' + connection.connectionId)).then(selection => {
            let selectedNode = activeConnections.find(connection => connection.label + ' ' + connection.connectionId === selection);
            this.interactWithOENode(selectedNode);
        });
    });
});
```

## <a name="proposed-apis"></a>建议的 API

我们添加了建议的 API，以允许扩展在对话框、向导和文档选项卡中显示自定义 UI 以及其他功能。 有关更多文档，请参阅[建议的 API 类型文件](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.proposed.d.ts)，但请注意，这些 API 随时可能发生更改。 有关如何使用其中一些 API 的示例，请参阅[“sqlservices”示例扩展](https://github.com/Microsoft/azuredatastudio/tree/main/samples/sqlservices)。


