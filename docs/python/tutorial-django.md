# 使用 VS Code 开发 Django 项目教程

Django 是一个高水平的，可用来快速、 安全、可伸缩开发 web 网站的 Python 框架。 Django包含对URL路由、页面模板和数据处理的丰富支持。

在这个 Django 教程里, 你创建一个简单的 Django 应用，使用了基于模板的三个页面。为了理解Django在VS Code终端、编辑器和调试器中如何工作，在Visual Studio Code中创建了这个应用。 本教程不探讨Django本身的各种细节, 比如使用数据模型和创建管理接口。有关这些方面的指导，请参阅本教程末尾的Django文档链接。

本 Django 教程中完成的代码可以在GitHub上找到: [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial).

如果有任何问题, 您可以在 [VScode 文档仓库](https://github.com/Microsoft/vscode-docs/issues) 中提交本教程中的问题。

## 要求

要成功地完成本 Django 教程，您必须执行以下操作 ( 与 [general Python教程](/docs/python/python-tutorial.md) 中的步骤相同 ):

1. Install the [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

1. 安装Python 3的一个版本(本教程就是为这个版本编写的)。 包括:
   - (所有操作系统)下载自 [python.org](https://www.python.org/downloads/) ; 通常使用最先出现在页面上的 **Download Python 3.7.0** 按钮(或任何最新版本)。
   - (Linux)内置的Python 3安装工作得很好，但是要安装其他 Python 包，您必须在终端中运行 `sudo apt install python3-pip` 。
   - (macOS)通过 [Homebrew](https://brew.sh/) 在macOS上使用 `brew install python3` 进行安装(不支持在macOS上安装Python系统)。
   - 下载自 [Anaconda](https://www.anaconda.com/download/) (用于数据科学目的)。

1. 在Windows上,确保Python解释器的路径包含在PATH环境变量中。您可以通过在命令提示符中运行“path”来检查路径。如果Python解释器的文件夹不包括在内, 打开Windows控制面板, 搜索"environment", 选择 **Edit environment variables for your account**, 然后编辑 **Path** 变量包含此文件夹。

## 为本Django项目指南创建虚拟环境

这部分, 您将创建一个虚拟环境来安装 Django 框架。使用虚拟环境可以避免将 Django 安装到全局 Python 环境中，使您能够精确地控制应用程序中使用的库。虚拟环境也让 [创建 requirements.txt 文件](#create-a-requirementstxt-file-for-the-environment)  很方便。

1. 创建一个 `hello_django` 项目文件夹 。

1. 此目录下，使用如下指令创建基于当前python版本的 `env` 虚拟环境:

    ```bash
    # macOS/Linux
    sudo apt-get install python3-venv    # 如果有必要的话
    python3 -m venv env

    # Windows
    python -m venv env
    ```

    > **注意**:在运行上述命令时使用一个普通的Python安装。如果您使用来自Anaconda安装的 `python.exe`，您将看到一个错误，因为ensurepip模块不可用，并且环境处于未完成状态。

1. 在此项目目录中执行 `code .` 命令启动 VS Code , 或者运行 VS Code 后，点 **File** 菜单，再选 **Open Folder** 菜单打开项目目录.

1. 在 VS Code 界面 , 打开"命令面板"( 点 **View** 菜单，再选 **Command Palette** 菜单)。 然后选择 **Python: Select Interpreter** 命令:

    ![Django tutorial: opening the Command Palette in VS Code](images/shared/command-palette.png)

1. 选择此命令后，会显示 VS Code 中可用的解释器列表 ( 会有所差异 ; 但如果没有找到所需的解释器, 请参见 [Configuring Python environments](/docs/python/environments.md) )。 从列表中为项目选择一个虚拟环境 ，起始是 `./env` 或 `.\env` :

    ![Django tutorial: Selecting the virtual environment for Python](images/shared/select-virtual-environment.png)

1. 打开"命令面板" 输入 **Terminal: Create New Integrated Terminal** 选择执行后，会打开一个自动激活虚拟环境的终端。

    > **注意** : 在Windows上，如果您的默认终端类型是 PowerShell，您可能会看到一个错误，它不能运行 activate.ps1 ，因为系统上禁用了运行脚本。该错误提供了有关如何允许脚本的信息的链接。否则，使用 **Terminal: Select Default Shell** 将 "Command Prompt" 或 "Git Bash" 设置为默认值。

1. 所选择的虚拟环境名会显示在 VS Code 状态条的左边, 有 "(venv)" 的提示，即表示你正处在虚拟环境下:

    ![Django tutorial: selected environment showing in the VS Code status bar](images/shared/environment-in-status-bar.png)

1. 在 VS Code 界面的终端窗口内，所属的虚拟环境下，执行如下列命令安装 Django 框架：

    ```bash
    python -m pip install django
    ```

    >用阿里的镜像源安装可以执行以下命令

    ```bash
    python -m pip install -i https://mirrors.aliyun.com/pypi/simple django
    ```

现在您已经为编写Django代码准备好了一个独立的环境。 当你点击 `终端` 菜单，再点击 `新建终端` 菜单后 VS Code 打开自动激活虚拟环境的终端窗口。 如果您已经打开了命令提示符或终端，则可以通过运行`source env/bin/activate`(Linux/macOS)或`env\scripts\activate`(Windows)来激活虚拟环境。 当命令提示符的开始处显示 **(env)** 时，表示虚拟环境被激活。

## 创建和运行最小化的 Django 应用

在Django术语中，“Django project”由几个site-level配置文件和一个或多个“app”组成，您可以将这些“apps”部署到web主机上以创建完整的web应用。 Django project 可以包含多个app，每个app通常在项目中具有独立的功能，相同功能的app可以应用在多个Django项目中。 每个app只是符合Django约定的 Python 包。

要创建最小的Django app，首先需要创建Django项目作为app的容器，然后创建app。出于这两个目的, 您可以使用Django管理实用程序 `Django-admin` , 它是在安装Django时已经安装了的。

### 创建 Django 项目

1. 在 VS Code 终端，激活了虚拟环境的窗口中, 执行如下指令:

    ```bash
    django-admin startproject web_project .
    ```

     在当前目录为项目目录下 ( 指令最后有个 `.` ) , 执行指令后会创建如下这些内容:

    - `manage.py`: Django命令行管理实用程序。 使用 `python manage.py <command> [options]` 的格式.

    - 名为 `web_project` 的子目录, 包含了如下这些文件:
        - `__init__.py`: 一个空文件，告诉 Python 这个文件夹为 Python 包。
        - `wsgi.py`: WSGI兼容的web服务器为项目提供入口。 通常不必去修改此文件。
        - `settings.py`: 包含Django项目的设置，您可以在开发web应用程序的过程中对其进行修改。
        - `urls.py`: 包含了Django项目的 `对应表` ，在开发过程中根据需要修改。

1. 要验证Django项目，请确保您的虚拟环境是激活的，然后使用 `python manage.py runserver` 命令启动Django的开发服务器。 服务器在默认端口8000上运行，在终端窗口中可以看到如下输出:

    ```bash
    Performing system checks...

    System check identified no issues (0 silenced).

    September 05, 2018 - 14:33:31
    Django version 2.1.1, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CTRL-BREAK.
    ```

    第一次运行服务器时，它会创建一个默认的SQLite数据库文件 `db.sqlite3`, 它用于开发目的，但也可用于生产中，用于小容量的web应用程序。 此外，Django内置的web服务器仅用于本地开发。 在部署到web主机时，Django使用主机的web服务器。`wsgi.py` 模块在Django项目中，负责连接到生产服务器。

    如果您想使用与默认端口8000不同的端口，请在命令行上指定端口号，例如 `python manage.py runserver 5000` 。

1. 按住`Ctrl`键不放，鼠标点击终端输出窗口中的 `http://127.0.0.1:8000/` URL，在默认浏览器上打开该地址页。 如果正确安装了Django并且项目有效，您将看到如下所示的默认页面。VS Code终端输出窗口也显示了服务器日志。

    ![Django tutorial: default view of empty Django project](images/django-tutorial/django-empty-project-success.png)

1. 完成后，关闭浏览器窗口并在VS Code终端输出窗口中使用`(Ctrl+C)`停止服务器。

### 创建一个 Django 应用

1. 在 VS Code 终端，虚拟环境内，项目路径下执行如下指令:

    ```bash
    python manage.py startapp hello
    ```

    该命令创建一个名为 `hello` 的文件夹，其中包含许多代码文件和一个子文件夹。其中，您经常使用 `views.py` (包含在web应用程序中定义页面的函数) 和 `models.py` (包含定义数据对象的类)。 Django的管理实用程序使用 `migrations` 文件夹来管理数据库版本，本教程后面将对此进行讨论。 还有这些文件 `apps.py` (app配置)， `admin.py` (用于创建管理接口) 和 `tests.py`(用于测试)，这里不讨论。

1. 修改 `hello/views.py` 文件为如下代码, 为此应用的主页创建一个 view :

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

1. 创建一个 `hello/urls.py` 文件, 包含下列代码。 `urls.py`文件是您指定将不同url路由到相应视图的模式的地方。下面的代码包含一个将app的根URL (`""`) 映射到你刚刚添加到`hello/views.py`的`views.home`函数的路径：

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

1. 这个 `web_project` 目录也包含了一个 `urls.py` 文件, 这是实际处理URL路由的地方。 修改 `web_project/urls.py` 文件为如下代码。这段代码用 `django.urls.include` 把app的 `hello/urls.py` 拉入，它将app的路由包含在app中。 当一个项目包含多个app时，这种分离很有帮助。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

1. 保存所有修改的文件可以用快捷键( Ctrl+K S )。

1. 在VS Code终端中，在激活了虚拟环境下，使用 `python manage.py runserver` 运行开发服务器，并打开浏览器到  `http://127.0.0.1:8000/` 查看呈现的"Hello, Django"页面。

    ![Django tutorial: the basic Django app running in a browser](images/django-tutorial/app-in-browser-01.png)

## 创建调试器启动配置文件

您可能已经在想，是否有一种更简单的方法来运行服务器和测试应用程序，而不是每次都输入 `python manage.py runserver` 。幸运的是,有! 您可以在VS Code中创建一个定制的启动配置文件，用于调试和练习。

1. 在VS Code中，切换到**Debug**视图 (使用左侧的活动栏)。在Debug视图的顶部，您可能会看到 "No Configurations" 和齿轮图标上的警告点。这两个指示器都意味着您还没有包含调试配置的`launch.json`文件：

    ![Django tutorial: initial view of the debug panel](images/shared/debug-panel-initial-view.png)

1. 选择齿轮图标，等待几秒钟，等待VS Code创建并打开 `launch.json` 文件。(如果您正在使用较老版本的VS Code，可能会提示您一个调试器目标列表，在这种情况下，请从列表中选择 **Python** )  `launch.json` 文件包含许多调试配置，每个配置在 `configuration` 数组中都是一个单独的JSON对象。

1. 向下滚动查找名为 "Python: Django"的配置:

    ```json
    {
        "name": "Python: Django",
        "type": "python",
        "request": "launch",
        "program": "${workspaceFolder}/manage.py",
        "console": "integratedTerminal",
        "args": [
            "runserver",
            "--noreload"
        ],
        "django": true
    },
    ```

    这个配置告诉VS Code用所选的Python解释器及`args`列表中的参数去运行`"${workspaceFolder}/manage.py"`。使用此配置启动VS Code调试器，与在激活的虚拟环境的VS Code终端中运行  `python manage.py runserver --noreload` 相同。(如果需要，可以将 `"5000"`之类的端口号添加到`args` 中。)`"django": true`条目还告诉VS Code启用Django页面模板的调试，您将在本教程的后面看到这一点。

1. 保存 `launch.json` 。 在debug configuration下拉列表中选择 **Python: Django** 配置:

    ![Django tutorial: selecting the Django debugging configuration](images/django-tutorial/debug-select-configuration.png)

1. 测试配置 ，选择 **Debug** > **Start Debugging** 菜单, 或者选择如下所示的绿色 **Start Debugging** 箭头:

    ![Django tutorial: start debugging/continue arrow on the debug toolbar](images/django-tutorial/debug-continue-arrow.png)

1. `kbstyle(Ctrl+click)`在终端输出窗口中的 `http://127.0.0.1:8000/`  URL打开浏览器，查看应用程序是否正常运行。

1. 完成后，关闭浏览器并停止调试器。要停止调试器，可以使用stop工具栏按钮(红色方块)或 **Debug** > **Stop Debugging** 命令(`kb(workbench.action.debug.stop)`)。

1. 现在您可以随时使用**Debug** > **Start Debugging**来测试app，还可以自动保存所有修改的文件。

## 探索调试器

调试使您有机会在特定的代码行上暂停正在运行的程序。当程序暂停时，您可以检查变量，在调试控制台面板中运行代码，或者利用 [Debugging](/docs/python/debugging.md) 中描述的特性。运行调试器还会在调试会话开始之前自动保存任何修改后的文件。

在开始之前: 确保你已经在最后一节结束时通过在终端中使用 `kbstyle(Ctrl+C)` 来停止了正在运行的应用程序。如果您让应用程序在一个终端上运行，它将继续拥有该端口。因此，当您使用相同的端口在调试器中运行应用程序时，最初运行的应用程序将处理所有请求，您不会看到正在调试的应用程序中的任何活动，程序也不会在断点处停止。换句话说，如果调试器似乎不工作，请确保没有应用程序的其他实例仍在运行。

1. 在 `hello/urls.py` 上, 添加如下路由项到 `urlpatterns` 列表:

    ```python
    path("hello/<name>", views.hello_there, name="hello_there"),
    ```

     `path` 的第一个参数定义了一个路由“hello/”，它接受一个名为*name*的变量字符串。该字符串被传递给在 `path` 的第二个参数中指定的 `views.hello_there` 函数。

    URL路由是区分大小写的。例如，路由`/hello/<name>`不同于`/Hello/<name>`。如果您想让相同的视图函数处理这不同的两个，请为每个定义相同路径。

1. 用以下代码替换 `views.py` 的内容定义 `hello_there` 函数，你可以单步通过调试：

    ```python
    import re
    from datetime import datetime
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")

    def hello_there(request, name):
        now = datetime.now()
        formatted_now = now.strftime("%A, %d %B, %Y at %X")

        # Filter the name argument to letters only using regular expressions. URL arguments
        # can contain arbitrary text, so we restrict to safe characters only.
        match_object = re.match("[a-zA-Z]+", name)

        if match_object:
            clean_name = match_object.group(0)
        else:
            clean_name = "Friend"

        content = "Hello there, " + clean_name + "! It's " + formatted_now
        return HttpResponse(content)
    ```

    URL路由中定义的 `name` 变量作为 `hello_there` 函数的参数。如代码注释所述，始终过滤用户提供的任意信息，以避免对应用程序的各种攻击。在本例中，代码将name参数过滤为只包含字母，从而避免了注入控制字符、HTML等。(在下一节中使用模板时，Django会自动过滤，因此不需要这些代码。)

1. 在 `hello_there` 函数 (`now = datetime.now()`) 的第一行代码设置一个断点，方法如下:
    - 在当前行, 按 F9, 或者,
    - 在当前行, 选择菜单 **Debug** > **Toggle Breakpoint** , 或者,
    - 直接点击行号左边的空白(当鼠标停留在那里时，会出现一个褪色的红点)。

    断点在左侧空白处显示为一个红点:

    ![Django tutorial: a breakpoint set on the first line of the hello_there function](images/django-tutorial/debug-breakpoint-set.png)

1. 通过选择 **Debug** > **Start Debugging** 菜单命令来启动调试器，或者点击如下所示的边上的绿色 **Start Debugging** 箭头：

    ![Django tutorial: start debugging/continue arrow on the debug toolbar](images/django-tutorial/debug-continue-arrow.png)

    可以看到状态栏的颜色发生了变化，表示正在调试：

    ![Django tutorial: appearance of the debugging status bar](images/django-tutorial/debug-status-bar.png)

    在VS Code中还会出现一个调试工具栏 (如下所示)，其中包含以下顺序的命令: Pause ( 或者 Continue), Step Over , Step Into , Step Out , Restart ,  Stop 。 有关每个命令的描述，请参见 [VS Code debugging](/docs/editor/debugging.md) 。

    ![Django tutorial: the VS Code debug toolbar](images/shared/debug-toolbar.png)

1. 输出出现在 "Python Debug Console" 终端中。打开浏览器并导航到 `http://127.0.0.1:8000/hello/VSCode` 。在页面渲染之前，VS代码会在您设置的断点处暂停程序。断点上的黄色小箭头表示这是要运行的下一行代码。

    ![Django tutorial: VS Code paused at a breakpoint](images/django-tutorial/debug-program-paused.png)

1. 使用 Step Over 来运行 `now = datetime.now()` 语句。

1. 在VS Code窗口的左侧，您将看到一个 **Variables** 窗格，其中显示了本地变量及其参数(如 `now` 和 `name` )。下面的窗格的 **Watch** , **Call Stack** , **Breakpoints** (详见[VS Code debugging](/docs/editor/debugging.md) )。在 **Locals** 部分,尝试输入不同的值。你也可以双击来修改它们。然而，像 `now` 这样的变量改变会破坏程序。开发人员通常只在代码一开始没有生成正确的值时才更改值。

    ![Django tutorial: local variables and arguments in VS Code during debugging](images/django-tutorial/debug-local-variables.png)

1. 当程序暂停时， **Debug Console** 面板(与终端面板中的 "Python Debug Console" 不同)允许您使用表达式进行试验，并使用程序的当前状态调试 。例如，一旦你 step over 了 `now = datetime.now()` 这行代码，就可以尝试不同的日期/时间格式 。在编辑器中，选择 `now.strftime("%A, %d %B, %Y at %X")` 代码，然后右击并选择 **Debug: Evaluate** 将代码发送到调试控制台，在那里运行:

    ```bash
    now.strftime("%A, %d %B, %Y at %X")
    'Friday, 07 September, 2018 at 07:46:32'
    ```

    > **提示**: **Debug Console** 也显示来自应用程序内部的异常，这些异常可能不会出现在终端中。例如，如果您在调试视图的 **Call Stack** 区域中看到 "Paused on exception" 消息，请切换到 **Debug Console** 查看异常消息 。

1. 将下列行复制到调试控制台底部的 > 提示符中，并尝试更改格式 :

    ```bash
    now.strftime("%a, %d %B, %Y at %X")
    'Fri, 07 September, 2018 at 07:46:32'
    now.strftime("%a, %d %b, %Y at %X")
    'Fri, 07 Sep, 2018 at 07:46:32'
    now.strftime("%a, %d %b, %y at %X")
    'Fri, 07 Sep, 18 at 07:46:32'
    ```

    > **注意**:如果您看到您想要的更改，您可以在调试会话期间将其复制并粘贴到编辑器中。但是，在重新启动调试器之前不会应用这些更改 。

1. 如果您愿意，再多执行几行代码，然后选择 Continue，让程序继续运行。浏览器窗口显示如下结果:

    ![Django tutorial: result of the modified program](images/django-tutorial/debug-run-result.png)

1. 完成后，关闭浏览器并停止调试器。要停止调试器，请使用 stop工具栏按钮(红色方块) 或 **Debug** > **Stop Debugging** 命令 。

> **提示**: 为了更方便地重复导航到一个特定的URL，比如 `http://127.0.0.1:8000/hello/VSCode`，可以在类似 `views.py` 的文件中使用 `print` 语句输出该URL。URL出现在 VS Code 终端中，您可以使用 `kbstyle(Ctrl+click)` 在浏览器中打开它。

## 转到定义 和 查看定义 命令

在使用Django或任何其他库的过程中，您可能想检查这些库本身的代码。VS Code 提供了两个方便的命令，可以直接导航到任何代码中的类和其他对象的定义:

- **转到定义** 从你的代码跳转到定义对象的代码。例如, 在 `views.py` 内, 在 `home` 函数内的 `HttpResponse` 上右键选择 **转到定义**, 导航到Django库中的类定义。

- **查看定义** (同样在右键菜单上), 但是直接在编辑器中显示类定义(会在编辑器窗口中腾出空间以避免遮盖代码)。按 `kbstyle(Escape)` 键或点击右上角的 **x** 关闭 Peek 窗口。

    ![Django tutorial: Peek Definition showing the Flask class inline](images/django-tutorial/peek-definition.png)

## 使用模版和渲染页面

到目前为止，您在本教程中创建的应用程序用Python代码仅生成纯文本web页面 。虽然可以直接在代码中生成HTML，但开发人员避免这样做，因为这样会将应用程序以 [cross-site scripting (XSS) attacks](https://en.wikipedia.org/wiki/Cross-site_scripting) 方式打开 。例如，在本教程的 `hello_there` 函数中，可以考虑使用类似 `content = "<h1>Hello there, " + clean_name + "!</h1>` 的代码格式来格式化输出，其中 `content` 的结果直接提供给浏览器 。这个开放允许攻击者将恶意HTML(包括JavaScript代码)放置在 `clean_name` 后面的URL中运行。

一个更好的实践是通过使用 **templates** 将HTML完全排除在您的代码之外，这样您的代码只关心数据值而不关心渲染。

在Django中，模板是一个HTML文件，其中包含代码在运行时提供的值的占位符。 Django模板引擎在渲染页面时负责进行替换，并提供自动转义来防止XSS攻击(也就是说，如果您尝试在数据值中使用HTML，您将看到仅以纯文本形式渲染的HTML)。因此，代码只与数据值有关，模板只与标记有关。  Django模板提供了灵活的选项，比如模板继承，它允许您使用通用标记定义基页面，然后在此基础上添加特定于页面的内容。

在本节中，首先使用模板创建一个页面。在后面的小节中，您将应用程序配置为提供静态文件，然后为应用程序创建多个页面，每个页面都包含一个来自基本模板的导航栏。Django 模板还支持控制流和迭代，您将在本教程后面的模板调试上下文中看到这一点。

1. 在 `web_project/settings.py` 文件中，找到 `INSTALLED_APPS` 列表并添加以下条目，以确保 project 知道 app，以便处理模板：

    ```python
    'hello',
    ```

1. 在 `hello` 文件夹中，创建一个名为 `templates`的文件夹，然后创建另一个名为 `hello` 的子文件夹，以匹配 app 名称 (这种两层的文件夹结构是典型的Django约定)。

1. 在 `templates/hello` 文件夹中，创建一个名为 `hello_there.html` 的文件，其内容如下。此模板包含两个占位符，分别表示名为 "name" 和 "date" 的数据值，它们由一对大括号 `{{` 和 `}}` 来描述。所有其他不变的文本和格式化标记(如 `<strong>`)都是模板的一部分。如您所见，模板占位符还可以包括格式、管道 `|` 符号后面的表达式，在本例中使用的是Django内置的 [date filter](https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#date) 和 [time filter](https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#time) 。然后，代码只需要传递 datetime 值 ，而不是预格式化的字符串:

    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8" />
            <title>Hello, Django</title>
        </head>
        <body>
            <strong>Hello there, {{ name }}!</strong> It's {{ date | date:"l, d F, Y" }} at {{ date | time:"H:i:s" }}
        </body>
    </html>
    ```

1. 在 `views.py` 的顶部添加以下import语句:

    ```python
    from django.shortcuts import render
    ```

1. 同样在 `views.py` 中，修改 `hello_there` 函数以使用 `django.shortcuts.render` 方法加载模板并提供 *template context* 。上下文只是模板中使用的一组变量。`render` 函数接受请求对象，然后是相对于 `templates` 文件夹的模板路径，然后是上下文对象。(开发人员通常将模板命名为与使用它们的函数相同的名称，但是不需要匹配名称，因为您总是在代码中引用准确的文件名。)

    ```python
    def hello_there(request, name):
        return render(
            request,
            'hello/hello_there.html',
            {
                'name': name,
                'date': datetime.now()
            }
        )
    ```

    您可以看到，代码现在简单多了，而且只关心数据值，因为标记和格式都包含在模板中。

1. 启动程序 ( 在调试器内部或外部，使用 `kb(workbench.action.debug.run)` )，导航到 /hello/name URL，然后观察结果。

1. 还可以尝试使用 `<a%20value%20that%20could%20be%20HTML>` 之类的名称导航到 /hello/name URL，以查看Django在工作时的自动转义。 "name" 值在浏览器中显示为纯文本，而不是渲染的实际元素

## 提供静态文件

静态文件是web应用程序按原样返回的内容片段，用于某些请求，如CSS文件。服务静态文件需要 `settings.py` 中的 `INSTALLED_APPS` 列表包含 `django.contrib.staticfiles`，

在Django中提供静态文件是一门艺术，尤其是在部署到生产环境中时。这里展示的是一种简单的方法，它既可以用于Django开发服务器，也可以用于像gunicorn这样的生产服务器。 但是，对静态文件的完整处理超出了本教程的范围，因此要了解更多信息，请参阅Django文档中的[管理静态文件](https://docs.djangoproject.com/en/2.1/howto/static-files/)。

在生产中，您还需要在 `settings.py` 中设置 `DEBUG=False`，这需要在使用容器时进行一些额外的工作。 详情请参阅 [Issue 13](https://github.com/Microsoft/python-sample-vscode-django-tutorial/issues/13)

### 为静态文件准备 app

1. 在项目的 `web_project/urls.py` 文件中添加下面的 `import` 语句:

    ```python
    from django.contrib.staticfiles.urls import staticfiles_urlpatterns
    ```

1. 在该文件中，在最后添加以下行，其中包括项目可识别的列表中的标准静态文件 url：

    ```python
    urlpatterns += staticfiles_urlpatterns()
    ```

### 引用模板中的静态文件

1. 在 `hello` 文件夹中，创建一个名为 `static` 的文件夹。

1. 在 `static` 文件夹中，创建一个名为 `hello` 的子文件夹，与应用程序名称匹配。

    这个额外的子文件夹的原因是，当您将Django项目部署到生产服务器时，您将所有静态文件收集到一个单独的文件夹中，然后由专用的静态文件服务器提供服务。 `static/hello` 子文件夹确保在收集应用程序的静态文件时，它们位于特定于应用程序的子文件夹中，不会与同一项目中其他应用程序的文件发生冲突。

1. 在 `static/hello` 文件夹中，创建一个名为 `site.css` 的文件，其内容如下。输入这段代码后，还可以观察 VS Code 为CSS文件提供的语法高亮显示，包括颜色预览。

    ```css
    .message {
        font-weight: 600;
        color: blue;
    }
    ```

1. 在 `templates/hello/hello_there.html` 中，在 `<title>` 元素之后添加以下行。 `{% load static %}` 标记是一个定制的 Django 模板标记集，它允许您使用 `{% static %}` 引用类似样式表的文件。

    ```html
    {% load static %}
    <link rel="stylesheet" type="text/css" href="{% static 'hello/site.css' %}" />
    ```

1. 同样在 `templates/hello/hello_there.html` , 用以下标记替换 `<body>` 元素使用 `message` 风格而不是 `<strong>` 标签:

    ```html
    <span class="message">Hello, there \{{ name }}!</span> It's \{{ date | date:'l, d F, Y' }} at \{{ date | time:'H:i:s' }}.
    ```

1. 运行 app，导航到 /hello/name URL，观察消息呈现为蓝色。完成后停止应用程序。

### 使用collectstatic命令

对于生产部署，通常使用 `python manage.py collectstatic` 命令将应用程序中的所有静态文件收集到一个文件夹中。然后可以使用专用的静态文件服务器来提供这些文件，这通常会带来更好的总体性能。下面的步骤展示了如何生成这个集合，但是在使用Django开发服务器时不使用这个集合。

1. 在 `web_project/settings.py` 中，添加以下行，它定义了使用 `collectstatic` 命令收集静态文件的位置:

    ```python
    STATIC_ROOT = os.path.join(BASE_DIR, 'static_collected')
    ```

1. 在终端，运行命令 `python manage.py collectstatic`，观察到 `hello/site.css` 与 `manage.py` 一起被复制到顶层的 `static_collected` 文件夹中。

1. 在实践中，只要您更改了静态文件，并在部署到生产环境之前，就可以运行 `collectstatic` 。

## 创建扩展基本模板的多个模板

因为大多数web应用程序有多个页面，而且这些页面通常共享许多公共元素，所以开发人员将这些公共元素分离到一个基页面模板中，然后其他页面模板进行扩展。(这也称为模板继承，即扩展页从基页继承元素。)

另外，因为您可能会创建许多扩展相同模板的页面，所以在 VS Code 中创建一个可以快速初始化新页面模板的代码片段是很有帮助的。代码段可以帮助您避免繁琐且容易出错的复制粘贴操作。

下面几节将介绍这个过程的不同部分。

### 创建基本页面模板和样式

Django中的基本页面模板包含一组页面的所有共享部分，包括对CSS文件、脚本文件等的引用。基本模板还定义了一个或多个 **block** 标记，其中包含扩展模板期望覆盖的内容。块标记由基本模板和扩展模板中的 `{% block %}` 和 `{% endblock %}` 来描述。

下面的步骤演示如何创建基本模板。

1. 在 `templates/hello` 文件夹中，创建一个名为 `layout.html` 的文件，其内容如下，其中包含了名为 "title" 和 "content" 的块。如您所见，标记定义了一个简单的导航条结构，其中包含指向 Home、About 和 Contact 页面的链接，您将在后面的部分中创建这些链接。请注意 Django 的 `{% url %}` 标记是通过相应URL模式的名称而不是通过相对路径来引用其他页面的。

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8"/>
        <title>{% block title %}{% endblock %}</title>
        {% load static %}
        <link rel="stylesheet" type="text/css" href="{% static 'hello/site.css' %}"/>
    </head>

    <body>
    <div class="navbar">
        <a href="{% url 'home' %}" class="navbar-brand">Home</a>
        <a href="{% url 'about' %}" class="navbar-item">About</a>
        <a href="{% url 'contact' %}" class="navbar-item">Contact</a>
    </div>

    <div class="body-content">
        {% block content %}
        {% endblock %}
        <hr/>
        <footer>
            <p>&copy; 2018</p>
        </footer>
    </div>
    </body>
    </html>
    ```

1. 将以下样式添加到 `static/hello/site.css` 文件的 "message" 样式下，并保存文件。(本演练不试图演示响应式设计;这些样式简单地生成了一个相当有趣的结果。

    ```css
    .navbar {
        background-color: lightslategray;
        font-size: 1em;
        font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
        color: white;
        padding: 8px 5px 8px 5px;
    }

    .navbar a {
        text-decoration: none;
        color: inherit;
    }

    .navbar-brand {
        font-size: 1.2em;
        font-weight: 600;
    }

    .navbar-item {
        font-variant: small-caps;
        margin-left: 30px;
    }

    .body-content {
        padding: 5px;
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    ```

此时您可以运行应用程序，但是因为您没有在任何地方使用基本模板，也没有更改任何代码文件，所以结果与前面的步骤相同。完成剩下的部分来查看最终的效果。

### 创建一个代码片段

因为在下一节中创建的三个页面将扩展为 `layout.html`，所以创建一个 **code snippet** 来初始化一个新模板文件将节省时间，该文件将包含对基本模板的适当引用。代码片段提供来自单个源的一致代码段，这避免了在使用现有代码的复制粘贴时可能出现的错误。

1. 在 VS Code, 选择 **File** (Windows/Linux) 或者 **Code** (macOS), 菜单, 然后选择 **Preferences** > **User snippets**.

1. 在出现的列表中, 选择 **html** 。 (如果您以前创建过代码片段，该选项可能会在列表的 **Existing Snippets** 部分显示为 "html.json" 。)

1. 在 VS code 打开 `html.json` 之后，将下面的代码添加到现有的花括号中。(此处未显示的注释描述了一些细节，比如 `$0` 行如何指示 VS code 在插入代码段后将光标放在何处):

    ```json
    "Django Tutorial: template extending layout.html": {
        "prefix": "djextlayout",
        "body": [
            "{% extends \"hello/layout.html\" %}",
            "{% block title %}",
            "$0",
            "{% endblock %}",
            "{% block content %}",
            "{% endblock %}"
        ],

        "description": "Boilerplate template that extends layout.html"
    },
    ```

1. 保存 html.json 文件

1. 现在，无论何时开始键入代码片段的前缀，例如 `djext` ,  VS Code 都会将代码片段作为自动完成选项提供，如下一节所示。您还可以使用 **Insert Snippet** 命令从菜单中选择一个代码段。

有关代码段的更多信息，请参考 [Creating snippets](/docs/editor/userdefinedsnippets.md).

### 使用代码片段添加页面

有了适当的代码片段，您就可以快速地为主页、About和Contact页面创建模板。

1. 在 `templates/hello` 文件夹中，创建一个名为 `home.html` 的新文件，然后开始键入 `djext` ，看到代码片段为完成:

    ![Django tutorial: autocompletion for the djextlayout code snippet](images/django-tutorial/autocomplete-for-code-snippet.png)

    当您选择完成时，代码片段的代码将与光标一起出现在代码片段的插入点上:

    ![Django tutorial: insertion of the djextlayout code snippet](images/django-tutorial/code-snippet-inserted.png)

1. 在 "title" 块中的插入点写入 `Home` ，在 "content" 块中写入 `<p>Home page for the Visual Studio Code Django tutorial.</p>` ，然后保存文件。这些行是扩展页面模板唯一独特的部分:

1. 在 `templates/hello` 文件夹中，创建 `about.html` ，使用代码段插入样板标记，分别在 "title" 和 "content" 块中插入 `About us` 和 `<p>About page for the Visual Studio Code Django tutorial.</p>` ，然后保存文件。

1. 重复前面的步骤，使用 `Contact us` 和 `<p>Contact page for the Visual Studio Code Django tutorial.</p>` 创建 `templates/hello/contact.html`

1. 在 app 的 `urls.py` 中，添加 /about 和 /contact 页面的路由。注意， `path` 函数的 `name` 参数定义了在模板的 `{% url %}` 标记中引用页面的名称。

    ```python
    path("about/", views.about, name="about"),
    path("contact/", views.contact, name="contact"),
    ```

1. 在 `views.py` 中，为引用各自页面模板的 /about 和 /contact 路由添加功能。还可以修改 `home` 函数以使用 `home.html` 模板。

    ```python
    # Replace the existing home function with the one below
    def home(request):
        return render(request, "hello/home.html")

    def about(request):
        return render(request, "hello/about.html")

    def contact(request):
        return render(request, "hello/contact.html")
    ```

### 运行 app

有了所有的页面模板，保存 `views.py` ，运行应用程序，并打开浏览器到主页查看结果。在页面之间导航，以验证页面模板是否正确地扩展了基本模板。

![Django tutorial: app rendering a common nav bar from the base template](images/django-tutorial/full-app.png)

## 处理数据、数据模型和迁移

许多web应用程序使用存储在数据库中的信息，Django使用 *models* 方便地表示数据库中的对象。在 Django 中，模型是一个Python类，派生自 `django.db.models.Model` ，它表示一个特定的数据库对象，通常是一个表。你把这些类放在应用的 `models.py` 文件中。

使用 Django，您几乎完全通过在代码中定义的模型来处理数据库。Django 的 "migrations" 会自动处理底层数据库的所有细节，随着时间的推移，您可以对模型进行改进。一般工作流程如下:

1. 更改 *models.py* 文件中的模型。
1. 运行 `python manage.py makemigrations` 以在 `migrations` 文件夹中生成将数据库从当前状态迁移到新状态的脚本。
1. 运行 `python manage.py migrate` 将脚本应用于实际的数据库。

迁移脚本有效地记录了您随时间对数据模型所做的所有增量更改。通过应用迁移，Django更新数据库以匹配您的模型。因为每个增量更改都有自己的脚本，所以Django可以自动地将任何以前版本的数据库(包括新数据库)迁移到当前版本。因此，您只需要关心 `models.py` 中的模型，而不需要关心底层数据库模式或迁移脚本。你让Django演那个角色!

在代码中，您也可以只使用模型类来存储和检索数据; Django 处理底层细节。唯一的例外是，可以使用Django管理实用程序 [loaddata command](https://docs.djangoproject.com/en/2.1/ref/django-admin/#loaddata) 将数据写入数据库。此实用程序通常用于在 `migrate` 命令初始化模式之后初始化数据集。

在使用 `db.sqlite3` 文件时，还可以使用 [SQLite browser](http://sqlitebrowser.org/) 之类的工具直接处理数据库。使用这样的工具在表中添加或删除记录是可以的，但是要避免更改数据库模式，因为这样数据库就会与应用程序的模型不同步。相反，更改模型，运行 `makemigrations` ，然后运行 `migrate` 。

### 数据库类型

默认情况下，Django为应用程序的数据库提供了一个适合于开发工作的 `db.sqlite3` 文件。正如在[When to use SQLite](https://www.sqlite.org/whentouse.html) (sqlite.org)中所描述的，SQLite适用于低流量或中等流量的网站，但不推荐用于高流量的网站。它还仅限于一台计算机，因此不能用于任何多服务器场景，如负载平衡和异地备份。

由于这些原因，可以考虑使用生产级的数据存储，如 [PostgreSQL](https://www.postgresql.org/)、[MySQL](https://www.mysql.com/) 和 [SQL Server](https://www.microsoft.com/en-ca/sql-server/) 。有关 Django 对其他数据库的支持的信息，请参阅 [Database setup](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#database-setup) 。您还可以使用 [Azure SDK for Python](https://docs.microsoft.com/azure/python/python-sdk-azure-get-started) 来处理 Azure 存储服务，比如 tables 和 blobs 。

### 定义模型

Django 模型也是一个派生自 `django.db.model.Models` 的 Python 类，您可以将它放在应用程序的 `models.py` 文件中。在数据库中，每个模型都自动给出一个名为 `id` 的唯一ID字段。所有其他字段被定义为使用 `django.db.models` 类型的类的属性，如 `CharField` (有限的文本)、 `TextField` (无限的文本)、 `EmailField` 、 `URLField` 、 `IntegerField` 、 `DecimalField` 、 `BooleanField` 、 `DateTimeField`， `ForeignKey`， `ManyToMany`，等等。(详细信息请参阅 Django 文档中的 [Model field reference](https://docs.djangoproject.com/en/2.1/ref/models/fields/) )

每个字段都有一些属性，比如 `max_length` 。 `blank=True` 属性表示该字段是可选的 ;  `null=true` 表示一个值是可选的。还有一个 `choices` 属性，它将值限制为数据值/显示值元组数组中的值。

例如，在 `models.py` 中添加以下类来定义一个数据模型，它表示一个简单的消息日志中的日期项:

```python
from django.db import models
from django.utils import timezone

class LogMessage(models.Model):
    message = models.CharField(max_length=300)
    log_date = models.DateTimeField("date logged")

    def __str__(self):
        """Returns a string representation of a message."""
        date = timezone.localtime(self.log_date)
        return f"'{self.message}' logged on {date.strftime('%A, %d %B, %Y at %X')}"
```

模型类可以包含从其他类属性计算的返回值的方法。模型通常包含 `__str__` 方法，该方法返回实例的字符串表示形式。

### 迁移数据库

因为通过编辑 `models.py` 更改了数据模型，所以需要更新数据库本身。在VS Code中，打开一个终端，激活您的虚拟环境(使用 **Terminal: Create New Integrated Terminal** 命令)，导航到项目文件夹，运行以下命令:

```bash
python manage.py makemigrations
python manage.py migrate
```

查看 `migrations` 文件夹，查看 `makemigrations` 生成的脚本。您还可以查看数据库本身，以查看模式是否更新。

如果在运行命令时看到错误，请确保没有使用前面步骤遗留下来的调试终端，因为它们可能没有激活虚拟环境。

### 通过模型使用数据库

有了模型并迁移了数据库之后，您只需要使用模型存储和检索数据。在本节中，您将向 app 添加一个表单页面，通过它可以记录消息。然后修改主页以显示这些消息。因为您在这里修改了许多代码文件，所以要注意细节。

1. 在 `hello` 文件夹中(其中有 `views.py` )，用以下代码创建一个名为 `forms.py` 的新文件，它定义了一个 Django 表单，其中包含从数据模型 `LogMessage` 中提取的字段:

    ```python
    from django import forms
    from hello.models import LogMessage

    class LogMessageForm(forms.ModelForm):
        class Meta:
            model = LogMessage
            fields = ("message",)   # NOTE: the trailing comma is required
    ```

1. 在 `templates/hello` 文件夹中，使用以下内容创建一个名为 `log_message.html` 的新模板，该模板假设有一个名为 `form` 的变量来定义表单主体。然后添加一个带有标签 "Log" 的提交按钮。

    ```html
    {% extends "hello/layout.html" %}
    {% block title %}
        Log a message
    {% endblock %}
    {% block content %}
        <form method="POST" class="log-form">
            {% csrf_token %}
            \{{ form.as_p }}
            <button type="submit" class="save btn btn-default">Log</button>
        </form>
    {% endblock %}
    ```

    > **注意**: Django的 `{% csrf_token %}` 标签提供了对跨站点请求伪造的保护。有关详细信息，请参阅Django文档中的 [Cross Site Request Forgery protection](https://docs.djangoproject.com/en/2.1/ref/csrf/) 。

1. 在应用程序的 `static/hello/site.css` 文件中，添加一条规则使输入表单更宽:

    ```css
    input[name=message] {
        width: 80%;
    }
    ```

1. 在应用程序的 `urls.py` 文件中，为新页面添加一条路径:

    ```python
    path("log/", views.log_message, name="log"),
    ```

1. 在 `views.py` 中，定义名为 `log_message` 的视图(通过URL路由引用)。这个视图同时处理HTTP GET和POST两种情况。在GET案例( `else:` 部分)中，它只显示您在前面步骤中定义的表单。在POST的情况下，它从表单中检索数据到一个数据对象( `message` )中，设置时间戳，然后保存该对象写入数据库的时间:

    ```python
    # Add these to existing imports at the top of the file:
    from django.shortcuts import redirect
    from hello.forms import LogMessageForm
    from hello.models import LogMessage

    # Add this code elsewhere in the file:
    def log_message(request):
        form = LogMessageForm(request.POST or None)

        if request.method == "POST":
            if form.is_valid():
                message = form.save(commit=False)
                message.log_date = datetime.now()
                message.save()
                return redirect("home")
        else:
            return render(request, "hello/log_message.html", {"form": form})
    ```

1. 在你准备好尝试一切之前，再多走一步! 在 `templates/hello/layout.html` 中，在 "navbar" div 中为消息日志页面添加一个链接:

    ```html
    <!-- Insert below the link to Home -->
    <a href="{% url 'log' %}" class="navbar-item">Log Message</a>
    ```

1. 运行该应用程序并打开主页的浏览器。选择导航栏上的 **Log Message** 链接，将显示消息日志页面:

    ![Django tutorial: the message logging page added to the app](images/django-tutorial/message-logging-page.png)

1. 输入一条消息，选择 **Log** ，您应该被带回到主页。主页还没有显示任何已记录的消息(稍后您将对此进行纠正)。您也可以随意记录更多的消息。如果需要，可以使用SQLite浏览器之类的工具在数据库中查看是否创建了记录。以只读方式打开数据库，或者在使用应用程序之前关闭数据库，否则应用程序将失败，因为数据库已被锁定。

1. 完成后停止应用程序。

1. 现在修改主页以显示记录的消息。首先用下面的标记替换 app 的 `templates/hello/home.html` 文件的内容。该模板需要一个名为 `message_list` 的上下文变量。如果它收到一条消息(通过 `{% if message_list %}` 标记检查)，那么它将遍历该列表( `{% for message in message_list %}` 标记)，为每个消息生成表行。否则，该页面表明还没有记录任何消息。

    ```html
    {% extends "hello/layout.html" %}
    {% block title %}
        Home
    {% endblock %}
    {% block content %}
        <h2>Logged messages</h2>

        {% if message_list %}
            <table class="message_list">
                <thead>
                <tr>
                    <th>Date</th>
                    <th>Time</th>
                    <th>Message</th>
                </tr>
                </thead>
                <tbody>
                {% for message in message_list %}
                    <tr>
                        <td>\{{ message.log_date | date:'d M Y' }}</td>
                        <td>\{{ message.log_date | time:'H:i:s' }}</td>
                        <td>
                            \{{ message.message }}
                        </td>
                    </tr>
                {% endfor %}
                </tbody>
            </table>
        {% else %}
            <p>No messages have been logged. Use the <a href="{% url 'log' %}">Log Message form</a>.</p>
        {% endif %}
    {% endblock %}
    ```

1. 在 `static/hello/site.css` 中，添加一个规则来稍微格式化表格:

    ```css
    .message_list th,td {
        text-align: left;
        padding-right: 15px;
    }
    ```

1. 在 `views.py` 中，导入 Django 的通用 `ListView` 类，我们将使用它来实现主页:

    ```python
    from django.views.generic import ListView
    ```

1. 同样在 `views.py` 中，将 `home` 函数替换为一个名为 `HomeListView` 的 *类* ,  `HomeListView` 派生于 `ListView` ，它将自己与 `LogMessage` 模型绑定，并实现一个 `get_context_data` 函数来生成模板的上下文。

    ```python
    # Remove the old home function if you want; it's no longer used

    class HomeListView(ListView):
        """Renders the home page, with a list of all messages."""
        model = LogMessage

        def get_context_data(self, **kwargs):
            context = super(HomeListView, self).get_context_data(**kwargs)
            return context
    ```

1. 在app的 `urls.py` 中。导入数据模型:

    ```python
    from hello.models import LogMessage
    ```

1. 同样在 `urls.py` 中，为新视图创建一个变量，该变量按降序检索最近的5个 `LogMessage` 对象(即查询数据库)，然后为模板上下文中的数据提供一个名称( `message_list` )，并标识要使用的模板:

    ```python
    home_list_view = views.HomeListView.as_view(
        queryset=LogMessage.objects.order_by("-log_date")[:5],  # :5 limits the results to the five most recent
        context_object_name="message_list",
        template_name="hello/home.html",
    )
    ```

1. 在 `urls.py`，修改主页的路径，使用 `home_list_view` 变量:

    ```python
        # Replace the existing path for ""
        path("", home_list_view, name="home"),
    ```

1. 启动应用程序，打开主页的浏览器，现在应该会显示以下信息:

    ![Django tutorial: app home page displaying message from the database](images/django-tutorial/app-with-message-list.png)

1. 完成后停止应用程序。

## 使用带有页面模板的调试器

如前一节所示，页面模板可以包含 `{% for message in message_list %}` 和 `{% if message_list %}` 这样的程序性指令，而不仅仅是 `{% url %}` 和 `{% block %}` 这样的被动的声明性元素。因此，与任何其他过程代码一样，模板内部也可能出现编程错误。

幸运的是，当您在调试配置中有 `"django": true` 时 ( 您已经这样做了 )，VS Code 的Python 扩展提供了模板调试。以下步骤演示了这种能力:

1. 在 `templates/hello/home.html` 中，在 `{% if message_list %}` 和 `{% for message in message_list %}` 线上设置断点，如下图中黄色箭头所示:

    ![Django tutorial: breakpoints set in a Django page template](images/django-tutorial/template-breakpoints.png)

1. 在调试器中运行该应用程序，并打开主页的浏览器。(如果你已经在运行调试器，你不需要在设置断点后重新启动应用程序;请刷新页面。)注意，VS代码在 `{% if %}` 语句的模板中进入调试器，并在 **Variables** 窗格中显示所有上下文变量:

    ![Django tutorial: debugger stopped at breakpoints in the page template](images/django-tutorial/template-debugger.png)

1. 使用 Step Over 命令逐步检查模板代码。观察调试器遍历所有声明性语句并暂停任何过程性代码。例如，遍历 `{% for message in message_list %}` 循环可以检查 `message` 中的每个值，还可以遍历像 `<td>\{{ message.log_date | date:'d M Y' }}</td>` 这样的行。

1. 您还可以使用 **Debug Console** 面板中的变量。(不过，像 `date` 这样的 Django 过滤器目前还不能在控制台中使用。)

1. 当你准备好了，选择继续完成运行应用程序，并在浏览器中查看呈现的页面。完成后停止调试器。

## 可选活动

以下部分描述了您在使用 Python 和 Visual Studio Code 时可能会发现有帮助的其他步骤。

### 为环境创建一个 requirements.txt 文件

当您通过源代码控制或其他方式共享应用程序代码时，复制虚拟环境中的所有文件是没有意义的，因为收件人总是可以自己重新创建该环境。

因此，开发人员通常会忽略源代码控制中的虚拟环境文件夹，而是使用 `requirements.txt` 文件描述应用程序的依赖关系。

虽然你可以手动创建文件，但你也可以使用 `pip freeze` 命令，根据安装在激活环境中的库生成文件:

1. 使用 **Python: Select Interpreter** 命令选择您所选择的环境后，运行 **Terminal: Create New Integrated Terminal** 命令以打开激活该环境的终端。

1. 在终端中，运行 `pip freeze > requirements.txt` 以在项目文件夹中创建 `requirements.txt` 文件。

任何接收到项目副本的人(或任何构建服务器)只需要运行 `pip install -r requirements.txt` 命令就可以在活动环境中重新安装应用程序所依赖的包。

> **注意**: `pip freeze` 列出了您在当前环境中安装的所有Python包，包括您当前没有使用的包。该命令还列出了具有确切版本号的包，您可能希望将这些版本号转换为范围，以便将来获得更大的灵活性。有关更多信息，请参见 pip 命令文档中的[Requirements files](https://pip.readthedocs.io/1.1/requirements.html)。

### 创建超级用户并启用管理界面

缺省情况下，Django 为受身份验证保护的 web 应用程序提供管理界面。该接口通过内置的 `django.contrib.admin` 应用程序实现， `django.contrib.admin` 应用程序默认包含在项目的 `INSTALLED_APPS` 列表( `settings.py` )中，认证使用内置的 `django.contrib.auth` 应用程序处理， `django.contrib.auth` 应用程序默认也在 `INSTALLED_APPS` 中。

执行以下步骤以启用管理界面:

1. 在 app 中为你的虚拟环境打开一个 VS Code 的终端，创建一个超级用户账号，然后运行命令 `python manage.py createsuperuser --username=<username> --email=<email>` ，当然是用你的个人信息代替 `<username>` 和 `<email>` 。当您运行该命令时，Django会提示您输入并确认您的密码。

    请务必记住您的用户名和密码组合。这些是您用来验证应用程序的凭据。

1. 在项目级 `urls.py` (本教程中的 `web_project/urls.py` )中添加以下URL路由，以指向内置的管理界面:

    ```python
    # This path is included by default when creating the app
     path("admin/", admin.site.urls),
    ```

1. 运行服务器，打开浏览器到应用程序的 /admin 页面(如使用开发服务器时的 `http://127.0.0.1:8000/admin` )。

1. 出现一个登录页面，由 `django.contrib.auth` 提供。输入您的超级用户凭证。

    ![Django tutorial: default Django login prompt](images/django-tutorial/login-prompt.png)

1. 一旦你通过认证，你可以看到默认的管理页面，通过这个页面你可以管理用户和组:

    ![Django tutorial: the default Django administrative interface](images/django-tutorial/default-admin-interface.png)

您可以根据需要定制管理接口。例如，您可以提供编辑和删除数据库中的条目的功能。有关定制的更多信息，请参考 [Django admin site documentation](https://docs.djangoproject.com/en/2.1/ref/contrib/admin/).

## 下一个步骤

祝贺您完成了在Visual Studio Code中使用Django的演练!

本教程中的完整代码项目可以在GitHub上找到: [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial).

在本教程中，我们只讨论了Django所能做的所有事情的皮毛。请务必访问 [Django documentation](https://docs.djangoproject.com/en/2.1/) 和 [official Django tutorial](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)，以获得关于视图、模板、数据模型、URL路由、管理接口、使用其他类型的数据库、部署到生产环境等等的更多详细信息。

要在产品网站上试用你的应用程序，请查看教程 [Deploy Python apps to Azure App Service using Docker Containers](https://docs.microsoft.com/azure/python/tutorial-deploy-containers-01)。Azure还提供了一个标准的容器，[App Service on Linux](https://docs.microsoft.com/azure/python/tutorial-deploy-app-service-on-linux-01)，您可以在 VS Code 中部署 web 应用程序。

你也可以在 VS Code 文档中查看以下与 Python 相关的文章:

- [编辑Python代码](/docs/python/editing.md)
- [Linting](/docs/python/linting.md)
- [管理Python环境](/docs/python/environments.md)
- [Python调试](/docs/python/debugging.md)
- [测试](/docs/python/testing.md)

如果您在本教程中遇到任何问题，请随时在 [VS Code documentation repository](https://github.com/Microsoft/vscode-docs/issues) 中提交问题
