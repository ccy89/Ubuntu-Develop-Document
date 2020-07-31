C++开发环境搭建
================
``C++`` 是一种被广泛使用的计算机程序设计语言，是20世纪80年代由 **Bjarne Stroustrup** 在贝尔实验室工作期间发明并实现。它是一种通用程序设计语言，支持多重编程模式，例如过程化程序设计、数据抽象、面向对象程序设计、泛型程序设计和设计模式等。

``C++`` 由 ``JTC1/SC22/WG21`` 的ISO工作组标准化。到目前为止，已经发布了 ``C++`` 标准的五个修订版，并且目前正在研究下一个修订版 ``C++ 20``：

.. table:: 
  :align: center

  ======  ====================  ===============
  Year    C++ Standard          Informal name
  ======  ====================  ===============
  1998    ISO/IEC 14882:1998    C++98
  2003    ISO/IEC 14882:2003    C++03
  2011    ISO/IEC 14882:2011    C++11, C++0x
  2014    ISO/IEC 14882:2014    C++14, C++1y
  2017    ISO/IEC 14882:2017    C++17, C++1z
  2020    to be determined      C++20, C++2a
  ======  ====================  ===============

安装CMake
----------------------
在 ``Linux`` 上开发 ``C++`` 基本都是 ``CMake`` 工程。因此要安装相应的工具链。
 
.. code-block:: bash

  # 不同Ubuntu系统官方源的cmake版本不一样
  # Ubuntu 18.04  3.10
  # Ubuntu 20.04  3.16
  sudo apt install --no-install-recommends cmake make

如果想要安装最新的 ``CMake``，需要添加 ``CMake`` 官方的源后再安装。

.. code-block:: bash

  wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
  sudo apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
  sudo apt update
  sudo apt install --no-install-recommends cmake make

``CMake`` 官方镜像源在国外，建议使用代理加速 ``apt`` 下载。


安装编译器
--------------
目前跨平台的主流工具链有两套 ``GNU`` 和 ``LLVM``，对应的 ``C/C++`` 编译器分别为 ``GCC`` 和 ``Clang``，对应的代码 ``Debug`` 工具分别为 ``GDB`` 和 ``LLDB``，各官网详情看外部链接。

:green:`(TODO 两者的优劣抽空写吧，一般用起来没啥区别)`，一般安装 ``GNU`` 工具链。

安装GNU
^^^^^^^^^^^
.. code-block:: bash
  
  # 不同Ubuntu系统官方源默认的GNU版本不一样
  # Ubuntu 18.04  GCC 7.4     GDB 8.1
  # Ubuntu 20.04  GCC 9.3     GDB 9.1
  sudo apt install --no-install-recommends gcc g++ gdb

  # gcc g++ 也可以安装别的版本，安装非默认的版本 是不会在 /usr/bin 下创建 gcc g++ 对应的链接文件的，需要自己创建
  sudo apt install --no-install-recommends gcc-8 g++-8 gdb

安装LLVM
^^^^^^^^^^^
.. code-block:: bash
  
  # 不同Ubuntu系统官方源默认的LLVM版本不一样
  # Ubuntu 18.04  6.0
  # Ubuntu 20.04  10.0
  sudo apt install --no-install-recommends clang lldb llvm-dev

  # 也可以安装别的版本，安装非默认的版本 是不会在 /usr/bin 下创建 clang lldb llvm 对应的链接文件的，需要自己创建
  sudo apt install --no-install-recommends clang-10 lldb-10 llvm-10

``LLVM`` 官网上提供了官方的预编译好的文件，可以下载下来直接解压就可以用。


VsCode开发C++
--------------
基本配置
^^^^^^^^^^^
``VsCode`` 通过安装插件可以支持 ``C++`` 开发，至少需要安装以下3个插件：

#. **C/C++**：提供C/C++ 智能提示, 代码调试等功能

#. **CMake**：提供CMake语言支持

#. **CMake Tools**：提供CMake工程创建、编译，智能提示等功能

创建CMake工程
^^^^^^^^^^^^^^^
**CMake Tools** 插件可以创建 ``CMake`` 工程。

**1. 创建基本工程**：打开 ``VsCode``，按 ``Ctrl+Shift+P`` 打开命令栏，输入 ``CMake: Quick Start`` 命令后回车，然后输入想要创建的工程的名字，最后选择工程类型为 ``Executable``。
``VsCode`` 会创建一个源文件 ``main.cpp`` 和 ``CMakeLists.txt``。

.. image:: /_static/images/c++-1.png

**2. 选择一个** ``kit``：按 ``Ctrl+Shift+P`` 打开命令栏后输入 ``CMake: Select a Kit`` 命令后选择编译器， 或者点击底部状态栏对应的图标后选择编译器。

.. image:: /_static/images/c++-2.png

**3. 选择编译类型**：按 ``Ctrl+Shift+P`` 打开命令栏后输入 ``CMake: Select Variant`` 命令后选择编译类型， 或者点击底部状态栏对应的图标后选择编译类型。默认为 ``Debug``。

.. image:: /_static/images/c++-3.png

4. ``CMake: Configure``： 按 ``Ctrl+Shift+P`` 打开命令栏后输入 ``CMake: Configure`` 命令， 或者点击左侧工具栏的 ``CMake`` 工具，点击 ``Configure All Projects``。
``VsCode`` 会自动在目录下生成 ``build`` 目录，并执行 ``cmake`` 命令：

.. image:: /_static/images/c++-4.png

**5. 配置** ``IntelliSense``：``CMake: Configure`` 之后右下角会弹出通知，是否需要由 ``CMake`` 为工程提供 Intellisense，选择 ``允许`` 后会生成一个隐藏文件夹 ``.vscode`` 和相关的 ``settings.json`` 文件。
如果提示缩回去了，可以点击右下角的铃铛图标后再一次显示。

``VsCode`` 会根据 ``CMake Configure`` 提供智能提示：

.. image:: /_static/images/c++-5.png

**6. 编译工程**：按 ``Ctrl+Shift+P`` 打开命令栏后输入 ``CMake: Build`` 命令， 或者点击底部状态栏对应的图标后执行编译：

.. image:: /_static/images/c++-6.png


Debug C++代码
^^^^^^^^^^^^^^^
1. 在 工程目录的隐藏文件 ``.vscode`` 中创建文件 ``launch.json``：

.. image:: /_static/images/c++-7.png

2. 在 ``launch.json`` 中写入调试相关配置：

.. code-block:: json

  {
    "version": "0.2.0",
    "configurations": [
      {
        "name": "debug",
        "type": "cppdbg",
        "request": "launch",
        "program": "${workspaceFolder}/build/example",
        "args": [],
        "stopAtEntry": true,
        "cwd": "${workspaceFolder}",
        "environment": [],
        "externalConsole": true,
        "MIMode": "gdb",
        "setupCommands": [
          {
            "description": "为 gdb 启用整齐打印",
            "text": "-enable-pretty-printing",
            "ignoreFailures": true
          }
        ]
      }
    ]
  }

以下几项需要更据自己的要求进行修改：

* ``name``： debug 工程的名字，一般不用改。
* ``program`` ：被 degbug的程序的路径，``${workspaceFolder}`` 是 CMake工程的根目录。
* ``args``：程序命令行参数，如果有的话，例子： ``"args": ["${workspaceFolder}/1.yaml", "${workspaceFolder}/2.txt"]``。
* ``externalConsole`` : debug 时是否打开一个新的终端。

3. 配置文件写好后，按 ``F5``，就可以进入调试了：

.. image:: /_static/images/c++-8.png


C++代码风格
--------------
参考 Google C++ 代码风格：`link <https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/>`_



外部链接
------------
``GNU`` 官网：`GNU Operating System <https://www.gnu.org/home.en.html>`_

``GCC`` 官网：`GCC, the GNU Compiler Collection <https://gcc.gnu.org/>`_

``GDB`` 官网：`GDB: The GNU Project Debugger <https://www.gnu.org/software/gdb/>`_

``LLVM`` 官网：`The LLVM Compiler Infrastructure <http://llvm.org/>`_

``Clang`` 官网：`Clang: a C language family frontend for LLVM <https://clang.llvm.org/>`_

``LLDB`` 官网：`The LLDB Debugger <https://lldb.llvm.org/>`_