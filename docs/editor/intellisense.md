---
Order: 4
Area: editor
TOCTitle: IntelliSense
ContentId: 80f4fa1e-d4c5-42cf-8b12-4b8e88c41c3e
PageTitle: IntelliSense in Visual Studio Code
DateApproved: 10/9/2019
MetaDescription:  Learn about Visual Studio Code IntelliSense (intelligent code completion).
---
# 智能感知

智能感知是各种代码编辑特性的通用术语，包括:代码完成、参数信息、快速信息和成员列表。智能感知功能还有其他名称，如“代码完成”、“内容辅助”和“代码提示”。

![IntelliSense demo](images/intellisense/intellisense.gif)

## 编程语言的智能感知

Visual Studio Code 提供了对 JavaScript、TypeScript、JSON、HTML、CSS、SCSS 等的智能感知 。VS Code 支持任何编程语言的基于关键字的补全，但是也可以通过安装语言扩展来配置成更丰富的智能感知 。

这是 [市场](https://marketplace.visualstudio.com/vscode) 有最流行的语言扩展。单击扩展块阅读描述和评论，以决定哪个扩展最适合您。

<div class="marketplace-extensions-languages-curated"></div>
## 智能感知功能

VS Code 智能感知功能是由语言服务提供的。语言服务提供基于语言语义和源代码分析的智能代码补全。如果语言服务知道可能的补全，那么在您键入时将弹出智能感知建议。如果继续键入字符，成员列表(变量、方法等)将被筛选为只包含包含键入字符的成员。 按 `kbstyle(Tab)` or `kbstyle(Enter)` 将插入所选成员。

您可以在任何编辑器窗口中通过输入 `kb(editor.action.triggerSuggest)` 或输入一个触发字符 (比如JavaScript中的点字符 (`kbstyle(.)`) 来触发智能感知。

![intellisense in package json](images/intellisense/intellisense_packagejson.gif)

> **提示:** suggestions小部件支持CamelCase过滤，这意味着您可以在方法名称中键入大写字母来限制建议。例如，“cra”将快速打开“createApplication”。

如果你愿意，你可以在编码的时候关闭智能感知。 详见 [定制智能感知](/docs/editor/intellisense.md#customizing-intellisense) 。下面学习如何禁用或自定义VS Code 的智能感知特性。

由语言服务提供，你可以看到 **快速信息** 的每个方法，要么按 `kb(toggleSuggestionDetails)` 或点击信息图标。该方法的相关文档现在将扩展到侧面。扩展的文档将保持不变，并在您浏览列表时更新。你可以通过再次点击 `kb(toggleSuggestionDetails)` 或者点击关闭图标来关闭它。

![quick info](images/intellisense/intellisense_docs.gif)

在选择一个方法后，您将获得 **参数信息**.

![parameter info](images/intellisense/paramater_info.png)

如果适用，语言服务将在 quick info 和 method signatures 中显示底层类型。 在上图中，你可以看到几种 `any` 类型. 因为 JavaScript 是动态的，不需要或强制类型， `any` 表示变量可以是任何类型。

## 类型的完成

下面的JavaScript代码演示了智能感知的完成。智能感知给出了推断的建议和项目的全局标识符。首先显示推断符号，然后是全局标识符(由文档图标显示)。

![intellisense icons](images/intellisense/intellisense_icons.png)

VS Code 智能感知提供了不同类型的补全，包括语言服务器建议、代码片段和简单的基于单词的文本补全。

|       |         |       |
| ----- | ------- | ----- |
| ![method icon](images/intellisense/Method_16x.svg) | Methods and Functions | `method`, `function`  |
| ![variable icon](images/intellisense/Field_16x.svg) | Variables and Fields | `variable`, `field` |
| ![class](images/intellisense/Class_16x.svg) | Classes | `class` |
| ![interface](images/intellisense/Interface_16x.svg) | Interfaces | `interface` |
| ![module](images/intellisense/Namespace_16x.svg) | Modules | `module` |
| ![property](images/intellisense/Property_16x.svg) | Properties and Attributes | `property` |
| ![enumeration icon](images/intellisense/EnumItem_16x.svg) | Values and Enumerations | `value`, `enum` |
| ![reference](images/intellisense/Reference_16x.svg) | References | `reference` |
| ![keyword](images/intellisense/Keyword_16x.svg) | Keywords | `keyword` |
| ![color](images/intellisense/ColorPalette_16x.svg) | Colors | `color` |
| ![unit](images/intellisense/Ruler_16x.svg) | Unit | `unit` |
| ![a square with ellipses forming the bottom show snippet prefix](images/intellisense/Snippet_16x.svg) | Snippet Prefixes | `snippet` |
| ![a square with letters abc word completion](images/intellisense/String_16x.svg) | Words | `text` |

## 定制智能感知

您可以在设置和键绑定中自定义智能感知体验。

### 设置

下面显示的是默认设置。 您可以在 `settings.json` 中更改这些。[用户和工作区设置](/docs/getstarted/settings.md) 。

```javascript
{
    // 输入时控制是否显示快速建议
    "editor.quickSuggestions": {
        "other": true,
        "comments": false,
        "strings": false
    },

    // 控制除了 Tab 键以外， Enter 键是否同样可以接受建议。这能减少“插入新行”和“接受建议”命令之间的歧义。smart表示仅在文本更改时使用Enter接受建议。Controls if suggestions should be accepted on 'Enter' - in addition to 'Tab'. Helps to avoid ambiguity between inserting new lines or accepting suggestions. The value 'smart' means only accept a suggestion with Enter when it makes a textual change
    "editor.acceptSuggestionOnEnter": "on",

    // Controls the delay in ms after which quick suggestions will show up.
    "editor.quickSuggestionsDelay": 10,

    // Controls if suggestions should automatically show up when typing trigger characters
    "editor.suggestOnTriggerCharacters": true,

    // Controls if pressing tab inserts the best suggestion and if tab cycles through other suggestions
    "editor.tabCompletion": "on",

    // Controls whether sorting favours words that appear close to the cursor
    "editor.suggest.localityBonus": true,

    // Controls how suggestions are pre-selected when showing the suggest list
    "editor.suggestSelection": "recentlyUsed",

    // Enable word based suggestions
    "editor.wordBasedSuggestions": true,

    // Enable parameter hints
    "editor.parameterHints.enabled": true,
}
```

### Tab补全

编辑器支持“tab补全”，当按下 `kb(insertBestCompletion)` 键时，它会插入最匹配的补全。无论是否显示建议小部件，这都是有效的。此外，在插入建议之后按下 `kb(insertNextSuggestion)` 将插入下一个最佳建议。

![Tab Completion](images/intellisense/tabCompletion.gif)

默认情况下，Tab补全是禁用的。使用 `editor.tabCompletion` 设置启用。这些值为:

* `off` - (默认) Tab补全是禁用。
* `on` - 所有建议都启用了Tab补全，重复调用会插入次佳建议。
* `onlySnippets` - 只Tab补全插入与当前行前缀匹配的静态代码片段。

### Locality Bonus

提示的排序取决于扩展信息以及它们与当前输入的单词的匹配程度。此外，您还可以使用 `editor.suggest.localityBonus` 设置来要求编辑器增强显示在更靠近光标位置的建议。

![Sorted By Locality](images/intellisense/localitybonus.png)

在上面的图片中，你可以看到 `count`, `context`, 和 `colocated` 是根据它们出现的范围(循环、函数、文件)来排序的。

### Suggestion selection

默认情况下，VS Code 会在提示列表中预先选择以前使用过的提示。这非常有用，因为您可以多次快速地插入相同的补全。如果你想不一样，例如，总是选择提示列表中的第一项，你可以设置 `editor.suggestSelection` 。

可设置的 `editor.suggestSelection` 参数值是:

* `first` - 始终选择顶部列表项。
* `recentlyUsed` - (默认)选择以前使用过的项目， 除非前缀(type to select)选择不同的项。
* `recentlyUsedByPrefix` - 根据以前完成这些提示的前缀选择项。

"Type to select" 表示当前前缀(大致是光标左边的文本)用于筛选和排序建议。当这种情况发生时，当它的结果与 `recentlyUsed` 的结果不同时，它将获得优先级。

当使用最后一个选项 `recentlyUsedByPrefix` 时，VS Code 会记住为特定的前缀(部分文本)选择了哪个项。例如，如果您输入 `co` ，然后选择 `console` ，那么下一次您输入 `co` 时，建议的 `console` 将被预先选择。这使您可以快速地将各种前缀映射到不同的提示，例如  `co` -> `console` and `con` -> `const` 。

### 片段的提示

默认情况下， VS Code 在一个小部件中显示代码片段和补全建议。您可以使用 `editor.snippetSuggestions`  设置。要从小部件中删除代码片段，请将值设置为 `"none"` 。如果您想查看片段，您可以指定相对于建议的顺序 ; 在顶部  (`"top"`) ，在底部  (`"bottom"`) ，或按字母顺序排列的行内  (`"inline"`) 。默认是  `"inline"` 。


### Key bindings

下面显示的键绑定是默认的。您可以在您的 `keybindings.json` 文件中按照 [Key Bindings](/docs/getstarted/keybindings.md) 中描述的那样更改它们。

> **Note:** There are many more key bindings relating to IntelliSense. Open the **Default Keyboard Shortcuts** (**File** > **Preferences** > **Keyboard Shortcuts**) and search for "suggest".

```json
[
    {
        "key": "ctrl+space",
        "command": "editor.action.triggerSuggest",
        "when": "editorHasCompletionItemProvider && editorTextFocus && !editorReadonly"
    },
    {
        "key": "ctrl+space",
        "command": "toggleSuggestionDetails",
        "when": "editorTextFocus && suggestWidgetVisible"
    },
    {
        "key": "ctrl+alt+space",
        "command": "toggleSuggestionFocus",
        "when": "editorTextFocus && suggestWidgetVisible"
    }
]
```

## 故障排除

如果发现智能感知停止工作，语言服务可能无法运行。尝试重新启动 VS Code，这应该可以解决问题。如果在安装语言扩展后仍然缺少智能感知功能，请在语言扩展问题库中展示你的问题。

> **Tip:** 配置和排除JavaScript智能感知 , 详见 [JavaScript documentation](/docs/languages/javascript.md#intellisense) 。

特定的语言扩展可能不支持 VS Code 的所有智能感知特性。查看扩展的 README 文件以了解支持什么。如果您认为存在语言扩展的问题，通常可以通过 [VS Code Marketplace](https://marketplace.visualstudio.com/vscode) 找到扩展的问题仓库。导航到扩展的详细信息页面并单击 `Support` 链接。

## 下一步

智能感知只是 VS Code 的强大功能之一。继续往下读 ，了解更多信息:

* [JavaScript](/docs/languages/javascript.md) - 充分利用JavaScript开发，包括配置智能感知。
* [Node.js](/docs/nodejs/nodejs-tutorial.md) - 在Node.js演练中可以看到一个智能感知的例子。
* [Debugging](/docs/editor/debugging.md) - 了解如何设置应用程序的调试。
* [Creating Language extensions](/api/language-extensions/programmatic-language-features.md) - 学习如何为新的编程语言创建添加智能感知的扩展。

## 常见问题

### 为什么我没有任何提示?

![image of IntelliSense not working](images/intellisense/intellisense_error.png)

这可能是由多种原因造成的。首先，尝试重新启动 VS Code 。如果问题仍然存在，请参阅语言扩展的文档。有关 JavaScript 的具体故障排除，请参阅 [JavaScript language topic](/docs/languages/javascript.md#intellisense).

### 为什么我看不到方法和变量提示?

![image of IntelliSense showing no useful suggestions](images/intellisense/missing_typings.png)

这个问题是由 JavaScript 中缺少类型声明(typings)文件引起的。您可以使用 [TypeSearch](https://microsoft.github.io/TypeSearch) 站点检查类型声明文件包是否可用于特定的库。在 [JavaScript language topic](/docs/languages/javascript.md#intellisense) 中有关于这个问题的更多信息。对于其他语言，请参考扩展的文档。
