<p align="center">
  <img alt="vscode logo" src="images/logo-stable.png" width="100px" />
  <h1 align="center">Visual Studio Code Documentation</h1>
</p>

您已经找到了Visual Studio Code文档GitHub仓库, 其中包含了[Visual Studio Code文档](https://code.visualstudio.com/docs)的内容。

此处提交的主题将发布到[Visual Studio Code](https://code.visualstudio.com)门户。

如果你正在寻找VS Code产品的GitHub仓库，你可以在[here](https://github.com/Microsoft/vscode)找到它。

## Index

1. [关于 Visual Studio Code](#visual-studio-code)
2. [Feedback](#feedback)
3. [Documentation Issues](#documentation-issues)
4. [Contributing to the documentation](#contributing)
5. [Publishing](#publishing)

## Visual Studio Code

[VS Code](https://code.visualstudio.com/) 是一个轻量级的源代码编辑器和强大的开发环境，用于构建和调试现代web和云应用。 它是免费的，可以在你喜欢的平台上使用——Linux、macOS和Windows。

如果你寻找关于VS代码的其他信息，请前往 [our website](https://code.visualstudio.com) 获取更多信息。

## 反馈

如果您希望提供文档反馈，请使用位于每个文档页面底部的反馈控件。

## Documentation Issues

要输入文档错误，请创建一个[new GitHub issue](https://github.com/Microsoft/vscode-docs/issues)。

如果您认为问题出在 VS Code 产品本身，请在 VS Code 产品仓库中输入问题 [这里](https://github.com/Microsoft/vscode/issues) 。

## 贡献

若要贡献新的 主题/信息 或对现有文档进行更改，请阅读[Contributing Guideline](./CONTRIBUTING.md#contributing)

### 流程

建议的两个工作流程是:

- 对于小的更改，可以在每个页面上使用 "Edit" 按钮直接在GitHub上编辑Markdown文件。
- 如果您想在VS Code中进行重大更改或预览Markdown文件，[克隆](#cloning) 仓库到 VS Code 中直接 [编辑与预览](https://code.visualstudio.com/docs/languages/markdown) 文件。

![Markdown Preview Button](images/MDPreviewButton.png)

### 克隆

1. 安装 [Git LFS](https://git-lfs.github.com/).
2. 运行 `git lfs install` 指令安装全局的 git hooks. 每台机器只需要运行一次。
3. 执行 `git clone git@github.com:Microsoft/vscode-docs.git`。
4. 现在你可以 `git add` 二进制文件以及提交。 他们将在LFS中被追踪。

#### 不包含二进制文件的克隆

您可能希望不使用1.6GB映像的情况下克隆。按如下操作:

1. 安装 [Git LFS](https://git-lfs.github.com/).
2. 运行 `git lfs install` 指令安装全局的 git hooks. 每台机器只需要运行一次。
3. 克隆仓库而不包含二进制文件。
    - macOS / Linux: `GIT_LFS_SKIP_SMUDGE=1 git clone git@github.com:Microsoft/vscode-docs.git`.
    - Windows: `GIT_LFS_SKIP_SMUDGE="1"; git clone git@github.com:Microsoft/vscode-docs.git`.
4. 现在您可以有选择地检出一些要使用的二进制文件。例如:
    - `git lfs pull -I "docs/nodejs"`
    - `git lfs pull -I "release-notes/images/1_3*/*"`
    - 你可以执行 `git lfs pull -I <PATTERN>` 指令 , 只要 `<PATTERN>` 是 comma-separated glob strings. 更多的模式详见 [Git LFS: Include and Exclude](https://github.com/git-lfs/git-lfs/blob/master/docs/man/git-lfs-fetch.1.ronn#include-and-exclude).

我们采用LFS之前仓库的历史可以在 [microsoft/vscode-docs-archive](https://github.com/Microsoft/vscode-docs-archive) 这里找到。

## Publishing

如何发布文档更改的方法可以在 VS Code网站的（私有）仓库中找到。
