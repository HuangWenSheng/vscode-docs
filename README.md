<p align="center">
  <img alt="vscode logo" src="images/logo-stable.png" width="100px" />
  <h1 align="center">Visual Studio Code Documentation</h1>
</p>

You've found the Visual Studio Code documentation GitHub repository, which contains the content for the [Visual Studio Code documentation](https://code.visualstudio.com/docs).

Topics submitted here will be published to the [Visual Studio Code](https://code.visualstudio.com) portal.

If you are looking for the VS Code product GitHub repository, you can find it [here](https://github.com/Microsoft/vscode).

## Index

1. [About Visual Studio Code](#visual-studio-code)
2. [Feedback](#feedback)
3. [Documentation Issues](#documentation-issues)
4. [Contributing to the documentation](#contributing)
5. [Publishing](#publishing)

## Visual Studio Code

[VS Code](https://code.visualstudio.com/) is a lightweight source code editor and powerful development environment for building and debugging modern web and cloud applications. It is free and available on your favorite platform - Linux, macOS, and Windows.

If you landed here looking for other information about VS Code, head over to [our website](https://code.visualstudio.com) for additional information.

## Feedback

If you want to give documentation feedback, please use the feedback control located at the bottom of each documentation page.

## Documentation Issues

To enter documentation bugs, please create a [new GitHub issue](https://github.com/Microsoft/vscode-docs/issues). Please check if there is an existing issue first.

If you think the issue is with the VS Code product itself, please enter issues in the VS Code product repo [here](https://github.com/Microsoft/vscode/issues).

## Contributing

To contribute new topics/information or make changes to existing documentation, please read the [Contributing Guideline](./CONTRIBUTING.md#contributing).

### Workflow

The two suggested workflows are:

- For small changes, use the "Edit" button on each page to edit the Markdown file directly on GitHub.
- If you plan to make significant changes or preview the Markdown files in VS Code, [clone](#cloning) the repo to [edit and preview](https://code.visualstudio.com/docs/languages/markdown) the files directly in VS Code.

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

如何发布文档更改的方法可以在 [here](https://github.com/Microsoft/vscode-website#publishing-a-documentation-change) （ VS Code网站的（私有）仓库中）找到。
