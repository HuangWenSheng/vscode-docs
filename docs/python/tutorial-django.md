# 使用 VS Code 开发 Django 项目教程

Django 是一个高水平的，可用来快速、 安全、可伸缩开发 web 网站的 Python 框架。 Django包含对URL路由、页面模板和数据处理的丰富支持。

在这个 Django 教程里, 你创建一个简单的 Django 应用，使用了基于模板的三个页面。为了理解Django在VS Code终端、编辑器和调试器中如何工作，在Visual Studio Code中创建了这个应用。 本教程不探讨Django本身的各种细节, 比如使用数据模型和创建管理接口。有关这些方面的指导，请参阅本教程末尾的Django文档链接。

本 Django 教程中完成的代码可以在GitHub上找到: [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial).

如果有任何问题, 您可以在 [VScode 文档仓库](https://github.com/Microsoft/vscode-docs/issues) 中提交本教程中的问题。

## 要求

要成功地完成本 Django 教程，您必须执行以下操作 ( 与 [general Python教程](/docs/python/python-tutorial.md) 中的步骤相同 ):

1. Install the [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

1. 安装Python 3的一个版本(本教程就是为这个版本编写的)。 包括:
   - (All operating systems) A download from [python.org](https://www.python.org/downloads/); typically use the **Download Python 3.7.0** button that appears first on the page (or whatever is the latest version).
   - (Linux) The built-in Python 3 installation works well, but to install other Python packages you must run `sudo apt install python3-pip` in the terminal.
   - (macOS) An installation through [Homebrew](https://brew.sh/) on macOS using `brew install python3` (the system install of Python on macOS is not supported).
   - (All operating systems) A download from [Anaconda](https://www.anaconda.com/download/) (for data science purposes).

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

    > **Note**: Use a stock Python installation when running the above commands. If you use `python.exe` from an Anaconda installation, you see an error because the ensurepip module isn't available, and the environment is left in an unfinished state.

1. 在此项目目录中执行 `code .` 命令启动 VS Code , 或者运行 VS Code 后，点 **File** 菜单，再选 **Open Folder** 菜单打开项目目录.

1. 在 VS Code 界面 , 打开"命令面板"( 点 **View** 菜单，再选 **Command Palette** 菜单)。 然后选择 **Python: Select Interpreter** 命令:

    ![Django tutorial: opening the Command Palette in VS Code](images/shared/command-palette.png)

1. 选择此命令后，会显示 VS Code 中可用的解释器列表 ( 会有所差异 ; 但如果没有找到所需的解释器, 请参见 [Configuring Python environments](/docs/python/environments.md) )。 从列表中为项目选择一个虚拟环境 ，起始是 `./env` 或 `.\env` :

    ![Django tutorial: Selecting the virtual environment for Python](images/shared/select-virtual-environment.png)

1. 打开"命令面板" 输入 **Terminal: Create New Integrated Terminal** 选择执行后，会打开一个自动激活虚拟环境的终端。

    > **Note**: On Windows, if your default terminal type is PowerShell, you may see an error that it cannot run activate.ps1 because running scripts is disabled on the system. The error provides a link for information on how to allow scripts. Otherwise, use **Terminal: Select Default Shell** to set "Command Prompt" or "Git Bash" as your default instead.

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

## Go to Definition 和 Peek Definition 命令

在使用Django或任何其他库的过程中，您可能想检查这些库本身的代码。VS Code 提供了两个方便的命令，可以直接导航到任何代码中的类和其他对象的定义:

- **Go to Definition** 从你的代码跳转到定义对象的代码。例如, 在 `views.py` 内, 在 `home` 函数内的 `HttpResponse` 上右键选择 **Go to Definition**, 导航到Django库中的类定义。

- **Peek Definition** (同样在右键菜单上), 但是直接在编辑器中显示类定义(会在编辑器窗口中腾出空间以避免遮盖代码)。按 `kbstyle(Escape)` 键或点击右上角的 **x** 关闭 Peek 窗口。

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

1. 在 `templates/hello` 文件夹中，创建一个名为 `hello_there.html` 的文件，其内容如下。此模板包含两个占位符，分别表示名为 "name" 和 "date" 的数据值，它们由一对大括号 `\{{` 和 `}}` 来描述。所有其他不变的文本和格式化标记(如 `<strong>`)都是模板的一部分。如您所见，模板占位符还可以包括格式、管道 `|` 符号后面的表达式，在本例中使用的是Django内置的 [date filter](https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#date) 和 [time filter](https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#time) 。然后，代码只需要传递 datetime 值 ，而不是预格式化的字符串:

    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8" />
            <title>Hello, Django</title>
        </head>
        <body>
            <strong>Hello there, \{{ name }}!</strong> It's \{{ date | date:"l, d F, Y" }} at \{{ date | time:"H:i:s" }}
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

Serving static files in Django is something of an art, especially when deploying to production. What's shown here is a simple approach that works with the Django development server and also a production server like gunicorn. A full treatment of static files, however, is beyond the scope of this tutorial, so for more information, see [Managing static files](https://docs.djangoproject.com/en/2.1/howto/static-files/) in the Django documentation.

In production, you also need to set `DEBUG=False` in `settings.py`, which necessitates some additional work when using containers. For details, see [Issue 13](https://github.com/Microsoft/python-sample-vscode-django-tutorial/issues/13).

### Ready the app for static files

1. In the project's `web_project/urls.py`, add the following `import` statement:

    ```python
    from django.contrib.staticfiles.urls import staticfiles_urlpatterns
    ```

1. In that same file, add the following line at the end, which includes standard static file URLs to the list that the project recognizes:

    ```python
    urlpatterns += staticfiles_urlpatterns()
    ```

### Refer to static files in a template

1. In the `hello` folder, create a folder named `static`.

1. Within the `static` folder, create a subfolder named `hello`, matching the app name.

    The reason for this extra subfolder is that when you deploy the Django project to a production server, you collect all the static files into a single folder that's then served by a dedicated static file server. The `static/hello` subfolder ensures that when the app's static files are collected, they're in an app-specific subfolder and won't collide with file from other apps in the same project.

1. In the `static/hello` folder, create a file named `site.css` with the following contents. After entering this code, also observe the syntax highlighting that VS Code provides for CSS files, including a color preview.

    ```css
    .message {
        font-weight: 600;
        color: blue;
    }
    ```

1. In `templates/hello/hello_there.html`, add the following lines after the `<title>` element. The `{% load static %}` tag is a custom Django template tag set, which allows you to use `{% static %}` to refer to a file like the stylesheet.

    ```html
    {% load static %}
    <link rel="stylesheet" type="text/css" href="{% static 'hello/site.css' %}" />
    ```

1. Also in `templates/hello/hello_there.html`, replace the contents `<body>` element with the following markup that uses the `message` style instead of a `<strong>` tag:

    ```html
    <span class="message">Hello, there \{{ name }}!</span> It's \{{ date | date:'l, d F, Y' }} at \{{ date | time:'H:i:s' }}.
    ```

1. Run the app, navigate to a /hello/name URL, and observe that the message renders in blue. Stop the app when you're done.

### Use the collectstatic command

For production deployments, you typically collect all the static files from your apps into a single folder using the `python manage.py collectstatic` command. You can then use a dedicated static file server to serve those files, which typically results in better overall performance. The following steps show how this collection is made, although you don't use the collection when running with the Django development server.

1. In `web_project/settings.py`, add the following line that defines a location where static files are collected when you use the `collectstatic` command:

    ```python
    STATIC_ROOT = os.path.join(BASE_DIR, 'static_collected')
    ```

1. In the Terminal, run the command `python manage.py collectstatic` and observe that `hello/site.css` is copied into the top level `static_collected` folder alongside `manage.py`.

1. In practice, run `collectstatic` any time you change static files and before deploying into production.

## Create multiple templates that extend a base template

Because most web apps have more than one page, and because those pages typically share many common elements, developers separate those common elements into a base page template that other page templates then extend. (This is also called template inheritance, meaning the extended pages inherit elements from the base page.)

Also, because you'll likely create many pages that extend the same template, it's helpful to create a code snippet in VS Code with which you can quickly initialize new page templates. A snippet helps you avoid tedious and error-prone copy-paste operations.

The following sections walk through different parts of this process.

### Create a base page template and styles

A base page template in Django contains all the shared parts of a set of pages, including references to CSS files, script files, and so forth. Base templates also define one or more **block** tags with content that extended templates are expected to override. A block tag is delineated by `{% block <name> %}` and `{% endblock %}` in both the base template and extended templates.

The following steps demonstrate creating a base template.

1. In the `templates/hello` folder, create a file named `layout.html` with the contents below, which contains blocks named "title" and "content". As you can see, the markup defines a simple nav bar structure with links to Home, About, and Contact pages, which you create in a later section. Notice the use of Django's `{% url %}` tag to refer to other pages through the names of the corresponding URL patterns rather than by relative path.

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

1. Add the following styles to `static/hello/site.css` below the existing "message" style, and save the file. (This walkthrough doesn't attempt to demonstrate responsive design; these styles simply generate a reasonably interesting result.)

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

You can run the app at this point, but because you haven't made use of the base template anywhere and haven't changed any code files, the result is the same as the previous step. Complete the remaining sections to see the final effect.

### Create a code snippet

Because the three pages you create in the next section extend `layout.html`, it saves time to create a **code snippet** to initialize a new template file with the appropriate reference to the base template. A code snippet provides a consistent piece of code from a single source, which avoids errors that can creep in when using copy-paste from existing code.

1. In VS Code, select the **File** (Windows/Linux) or **Code** (macOS), menu, then select **Preferences** > **User snippets**.

1. In the list that appears, select **html**. (The option may appear as "html.json" in the **Existing Snippets** section of the list if you've created snippets previously.)

1. After VS code opens `html.json`, add the code below within the existing curly braces. (The explanatory comments, not shown here, describe details such as how the `$0` line indicates where VS Code places the cursor after inserting a snippet):

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

1. Save the `html.json` file (`kb(workbench.action.files.save)`).

1. Now, whenever you start typing the snippet's prefix, such as `djext`, VS Code provides the snippet as an autocomplete option, as shown in the next section. You can also use the **Insert Snippet** command to choose a snippet from a menu.

For more information on code snippets in general, refer to [Creating snippets](/docs/editor/userdefinedsnippets.md).

### Use the code snippet to add pages

With the code snippet in place, you can quickly create templates for the Home, About, and Contact pages.

1. In the `templates/hello` folder, create a new file named `home.html`, Then start typing `djext` to see the snippet appear as a completion:

    ![Django tutorial: autocompletion for the djextlayout code snippet](images/django-tutorial/autocomplete-for-code-snippet.png)

    When you select the completion, the snippet's code appears with the cursor on the snippet's insertion point:

    ![Django tutorial: insertion of the djextlayout code snippet](images/django-tutorial/code-snippet-inserted.png)

1. At the insertion point in the "title" block, write `Home`, and in the "content" block, write `<p>Home page for the Visual Studio Code Django tutorial.</p>`, then save the file. These lines are the only unique parts of the extended page template:

1. In the `templates/hello` folder, create `about.html`, use the snippet to insert the boilerplate markup, insert `About us` and `<p>About page for the Visual Studio Code Django tutorial.</p>` in the "title" and "content" blocks, respectively, then save the file.

1. Repeat the previous step to create `templates/hello/contact.html` using `Contact us` and `<p>Contact page for the Visual Studio Code Django tutorial.</p>`.

1. In the app's `urls.py`, add routes for the /about and /contact pages. Be mindful that the `name` argument to the `path` function defines the name with which you refer to the page in the `{% url %}` tags in the templates.

    ```python
    path("about/", views.about, name="about"),
    path("contact/", views.contact, name="contact"),
    ```

1. In `views.py`, add functions for the /about and /contact routes that refer to their respective page templates. Also modify the `home` function to use the `home.html` template.

    ```python
    # Replace the existing home function with the one below
    def home(request):
        return render(request, "hello/home.html")

    def about(request):
        return render(request, "hello/about.html")

    def contact(request):
        return render(request, "hello/contact.html")
    ```

### Run the app

With all the page templates in place, save `views.py`, run the app, and open a browser to the home page to see the results. Navigate between the pages to verify that the page templates are properly extending the base template.

![Django tutorial: app rendering a common nav bar from the base template](images/django-tutorial/full-app.png)

## Work with data, data models, and migrations

Many web apps work with information stored in a database, and Django makes it easy to represent the objects in that database using *models*. In Django, a model is a Python class, derived from `django.db.models.Model`, that represents a specific database object, typically a table. You place these classes in an app's `models.py` file.

With Django, your work with your database almost exclusively through the models you define in code. Django's "migrations" then handle all the details of the underlying database automatically as you evolve the models over time. The general workflow is as follows:

1. Make changes to the models in your *models.py* file.
1. Run `python manage.py makemigrations` to generate scripts in the `migrations` folder that migrate the database from its current state to the new state.
1. Run `python manage.py migrate` to apply the scripts to the actual database.

The migration scripts effectively record all the incremental changes you make to your data models over time. By applying the migrations, Django updates the database to match your models. Because each incremental change has its own script, Django can automatically migrate *any* previous version of a database (including a new database) to the current version. As a result, you need concern yourself only with your models in `models.py`, never with the underlying database schema or the migration scripts. You let Django do that part!

In code, too, you work exclusively with your model classes to store and retrieve data; Django handles the underlying details. The one exception is that you can write data into your database using the Django administrative utility [loaddata command](https://docs.djangoproject.com/en/2.1/ref/django-admin/#loaddata). This utility is often used to initialize a data set after the `migrate` command has initialized the schema.

When using the `db.sqlite3` file, you can also work directly with the database using a tool like the [SQLite browser](http://sqlitebrowser.org/). It's fine to add or delete records in tables using such a tool, but avoid making changes to the database schema because the database will then be out of sync with your app's models. Instead, change the models, run `makemigrations`, then run `migrate`.

### Types of databases

By default, Django includes a `db.sqlite3` file for an app's database that's suitable for development work. As described on [When to use SQLite](https://www.sqlite.org/whentouse.html) (sqlite.org), SQLite works fine for low to medium traffic sites with fewer than 100 K hits/day, but is not recommended for higher volumes. It's also limited to a single computer, so it cannot be used in any multi-server scenario such as load-balancing and geo-replication.

For these reasons, consider using a production-level data store such as [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/), and [SQL Server](https://www.microsoft.com/en-ca/sql-server/). For information on Django's support for other databases, see [Database setup](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#database-setup). You can also use the [Azure SDK for Python](https://docs.microsoft.com/azure/python/python-sdk-azure-get-started) to work with Azure storage services like tables and blobs.

### Define models

A Django model is again a Python class derived from `django.db.model.Models`, which you place in the app's `models.py` file. In the database, each model is automatically given a unique ID field named `id`. All other fields are defined as properties of the class using types from `django.db.models` such as `CharField` (limited text), `TextField` (unlimited text), `EmailField`, `URLField`, `IntegerField`, `DecimalField`, `BooleanField`. `DateTimeField`, `ForeignKey`, and `ManyToMany`, among others. (See the [Model field reference](https://docs.djangoproject.com/en/2.1/ref/models/fields/) in the Django documentation for details.)

Each field takes some attributes, like `max_length`. The `blank=True` attribute means the field is optional; `null=true` means that a value is optional. There is also a `choices` attribute that limits values to values in an array of data value/display value tuples.

For example, add the following class in `models.py` to define a data model that represents dated entries in a simple message log:

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

A model class can include methods that return values computed from other class properties. Models typically include a `__str__` method that returns a string representation of the instance.

### Migrate the database

Because you changed your data models by editing `models.py`, you need to update the database itself. In VS Code, open a Terminal with your virtual environment activated (use the **Terminal: Create New Integrated Terminal** command, `kb(workbench.action.terminal.new)`)), navigate to the project folder, and run the following commands:

```bash
python manage.py makemigrations
python manage.py migrate
```

Take a look in the `migrations` folder to see the scripts that `makemigrations` generates. You can also look at the database itself to see that the schema is updated.

If you see errors when running the commands, make sure you're not using a debugging terminal that's left over from previous steps, as they may not have the virtual environment activated.

### Use the database through the models

With your models in place and the database migrated, you can store and retrieve data using only your models. In this section, you add a form page to the app through which you can log a message. You then modify the home page to display those messages. Because you modify many code files here, be mindful of the details.

1. In the `hello` folder (where you have `views.py`), create a new file named `forms.py` with the following code, which defines a Django form that contains field drawn from the data model, `LogMessage`:

    ```python
    from django import forms
    from hello.models import LogMessage

    class LogMessageForm(forms.ModelForm):
        class Meta:
            model = LogMessage
            fields = ("message",)   # NOTE: the trailing comma is required
    ```

1. In the `templates/hello` folder, create a new template named `log_message.html` with the following contents, which assumes that the template is given a variable named `form` to define the body of the form. It then adds a submit button with the label "Log".

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

    > **Note**: Django's `{% csrf_token %}` tag provides protection from cross-site request forgeries. See [Cross Site Request Forgery protection](https://docs.djangoproject.com/en/2.1/ref/csrf/) in the Django documentation for details.

1. In the app's `static/hello/site.css` file, add a rule to make the input form wider:

    ```css
    input[name=message] {
        width: 80%;
    }
    ```

1. In the app's `urls.py` file, add a route for the new page:

    ```python
    path("log/", views.log_message, name="log"),
    ```

1. In `views.py`, define the view named `log_message` (as referred to by the URL route). This view handles both HTTP GET and POST cases. In the GET case (the `else:` section), it just displays the form that you defined in the previous steps. In the POST case, it retrieves the data from the form into a data object (`message`), sets the timestamp, then saves that object at which point it's written to the database:

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

1. One more step before you're ready to try everything out! In `templates/hello/layout.html`, add a link in the "navbar" div for the message logging page:

    ```html
    <!-- Insert below the link to Home -->
    <a href="{% url 'log' %}" class="navbar-item">Log Message</a>
    ```

1. Run the app and open a browser to the home page. Select the **Log Message** link on the nav bar, which should display the message logging page:

    ![Django tutorial: the message logging page added to the app](images/django-tutorial/message-logging-page.png)

1. Enter a message, select **Log**, and you should be taken back to the home page. The home page doesn't yet show any of the logged messages yet (which you remedy in a moment). Feel free to log a few more messages as well. If you want, peek in the database using a tool like SQLite Browser to see that records have been created. Open the database as read-only, or otherwise remember to close the database before using the app, otherwise the app will fail because the database is locked.

1. Stop the app when you're done.

1. Now modify the home page to display the logged messages. Start by replacing the contents of app's `templates/hello/home.html` file with the markup below. This template expects a context variable named `message_list`. If it receives one (checked with the `{% if message_list %}` tag), it then iterates over that list (the `{% for message in message_list %}` tag) to generate table rows for each message. Otherwise the page indicates that no messages have yet been logged.

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

1. In `static/hello/site.css`, add a rule to format the table a little:

    ```css
    .message_list th,td {
        text-align: left;
        padding-right: 15px;
    }
    ```

1. In `views.py`, import Django's generic `ListView` class, which we'll use to implement the home page:

    ```python
    from django.views.generic import ListView
    ```

1. Also in `views.py`, replace the `home` function with a *class* named `HomeListView`, derived from `ListView`, which ties itself to the `LogMessage` model and implements a function `get_context_data` to generate the context for the template.

    ```python
    # Remove the old home function if you want; it's no longer used

    class HomeListView(ListView):
        """Renders the home page, with a list of all messages."""
        model = LogMessage

        def get_context_data(self, **kwargs):
            context = super(HomeListView, self).get_context_data(**kwargs)
            return context
    ```

1. In the app's `urls.py`, import the data model:

    ```python
    from hello.models import LogMessage
    ```

1. Also in `urls.py`, make a variable for the new view, which retrieves the five most recent `LogMessage` objects in descending order (meaning that it queries the database), and then provides a name for the data in the template context (`message_list`), and identifies the template to use:

    ```python
    home_list_view = views.HomeListView.as_view(
        queryset=LogMessage.objects.order_by("-log_date")[:5],  # :5 limits the results to the five most recent
        context_object_name="message_list",
        template_name="hello/home.html",
    )
    ```

1. In `urls.py`, modify the path to the home page to use the `home_list_view` variable:

    ```python
        # Replace the existing path for ""
        path("", home_list_view, name="home"),
    ```

1. Start the app and open a browser to the home page, which should now display messages:

    ![Django tutorial: app home page displaying message from the database](images/django-tutorial/app-with-message-list.png)

1. Stop the app when you're done.

## Use the debugger with page templates

As shown in the previous section, page templates can contain procedural directives like `{% for message in message_list %}` and `{% if message_list %}`, rather than only passive, declarative elements like `{% url %}` and `{% block %}`. As a result, you can have programming errors inside templates as with any other procedural code.

Fortunately, the Python Extension for VS Code provides template debugging when you have `"django": true` in the debugging configuration (as you do already). The following steps demonstrate this capability:

1. In `templates/hello/home.html`, set breakpoints on both the `{% if message_list %}` and `{% for message in message_list %}` lines, as indicated by the yellow arrows in the image below:

    ![Django tutorial: breakpoints set in a Django page template](images/django-tutorial/template-breakpoints.png)

1. Run the app in the debugger and open a browser to the home page. (If you're already running the debugger, you don't have to restart the app after setting breakpoints; just refresh the page.) Observe that VS Code breaks into the debugger in the template on the `{% if %}` statement and shows all the context variables in the **Variables** pane:

    ![Django tutorial: debugger stopped at breakpoints in the page template](images/django-tutorial/template-debugger.png)

1. Use the Step Over (`kb(workbench.action.debug.stepOver)`) command to step through the template code. Observe that the debugger steps over all declarative statements and pauses at any procedural code. For example, stepping through the `{% for message in message_list %}` loops lets you examine each value in `message` and lets you step to lines like `<td>\{{ message.log_date | date:'d M Y' }}</td>`.

1. You can also work with variables in the **Debug Console** panel. (Django filters like `date`, however, are not presently available in the console.)

1. When you're ready, select Continue (`kb(workbench.action.debug.continue)`) to finish running the app and view the rendered page in the browser. Stop the debugger when you're done.

## Optional activities

The following sections describe additional steps that you might find helpful in your work with Python and Visual Studio Code.

### Create a requirements.txt file for the environment

When you share your app code through source control or some other means, it doesn't make sense to copy all the files in a virtual environment because recipients can always recreate that environment themselves.

Accordingly, developers typically omit the virtual environment folder from source control and instead describe the app's dependencies using a `requirements.txt` file.

Although you can create the file by hand, you can also use the `pip freeze` command to generate the file based on the exact libraries installed in the activated environment:

1. With your chosen environment selected using the **Python: Select Interpreter** command, run the **Terminal: Create New Integrated Terminal** command (`kb(workbench.action.terminal.new)`)) to open a terminal with that environment activated.

1. In the terminal, run `pip freeze > requirements.txt` to create the `requirements.txt` file in your project folder.

Anyone (or any build server) that receives a copy of the project needs only to run the `pip install -r requirements.txt` command to reinstall the packages on which the app depends within the active environment.

> **Note**: `pip freeze` lists all the Python packages you have installed in the current environment, including packages you aren't currently using. The command also lists packages with exact version numbers, which you might want to convert to ranges for more flexibility in the future. For more information, see [Requirements files](https://pip.readthedocs.io/1.1/requirements.html) in the pip command documentation.

### Create a superuser and enable the administrative interface

By default, Django provides an administrative interface for a web app that's protected by authentication. The interface is implemented through the build-in `django.contrib.admin` app, which is included by default in the project's `INSTALLED_APPS` list (`settings.py`), and authentication is handled with the built-in `django.contrib.auth` app, which is also in `INSTALLED_APPS` by default.

Perform the following steps to enable the administrative interface:

1. Create a superuser account in the app by opening a Terminal in VS Code for your virtual environment, then running the command `python manage.py createsuperuser --username=<username> --email=<email>`, replacing `<username>` and `<email>`, of course, with your personal information. When you run the command, Django prompts you to enter and confirm your password.

    Be sure to remember your username and password combination. These are the credentials you use to authenticate with the app.

1. Add the following URL route in the project-level `urls.py` (`web_project/urls.py` in this tutorial) to point to the built-in administrative interface:

    ```python
    # This path is included by default when creating the app
     path("admin/", admin.site.urls),
    ```

1. Run the server, the open a browser to the app's /admin page (such as `http://127.0.0.1:8000/admin` when using the development server).

1. A login page appears, courtesy of `django.contrib.auth`. Enter your superuser credentials.

    ![Django tutorial: default Django login prompt](images/django-tutorial/login-prompt.png)

1. Once you're authenticated, you see the default administration page, through which you can manage users and groups:

    ![Django tutorial: the default Django administrative interface](images/django-tutorial/default-admin-interface.png)

You can customize the administrative interface as much as you like. For example, you could provide capabilities to edit and remove entries in the database. For more information on making customizations, refer to the [Django admin site documentation](https://docs.djangoproject.com/en/2.1/ref/contrib/admin/).

## Next steps

Congratulations on completing this walkthrough of working with Django in Visual Studio Code!

The completed code project from this tutorial can be found on GitHub: [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial).

In this tutorial, we've only scratched the surface of everything Django can do. Be sure to visit the [Django documentation](https://docs.djangoproject.com/en/2.1/) and the [official Django tutorial](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) for many more details on views, templates, data models, URL routing, the administrative interface, using other kinds of databases, deployment to production, and more.

To try your app on a production website, check out the tutorial [Deploy Python apps to Azure App Service using Docker Containers](https://docs.microsoft.com/azure/python/tutorial-deploy-containers-01). Azure also offers a standard container, [App Service on Linux](https://docs.microsoft.com/azure/python/tutorial-deploy-app-service-on-linux-01), to which you deploy web apps from within VS Code.

You may also want to review the following articles in the VS Code docs that are relevant to Python:

- [Editing Python code](/docs/python/editing.md)
- [Linting](/docs/python/linting.md)
- [Managing Python environments](/docs/python/environments.md)
- [Debugging Python](/docs/python/debugging.md)
- [Testing](/docs/python/testing.md)

If you encountered any problems in the course of this tutorial, feel free to file an issue in the [VS Code documentation repository](https://github.com/Microsoft/vscode-docs/issues).
