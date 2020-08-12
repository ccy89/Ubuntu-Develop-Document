Python开发环境搭建
====================

``Python`` 是一种广泛使用的解释型、高级编程、通用型编程语言，由 `Guido van Rossum <https://en.wikipedia.org/wiki/Guido_van_Rossum>`_ 创造。

`Anaconda <https://www.anaconda.com/>`_ 是一个用于科学计算的 ``Python`` 发行版，支持 ``Linux``, ``Mac``, ``Windows``, 包含了众多流行的科学计算、数据分析的 ``Python`` 包。

``Miniconda`` 是一个 ``Anaconda`` 的轻量级替代，默认只包含了 ``Python`` 和 ``conda``，但是可以通过 ``pip`` 和 ``conda`` 命令来安装所需要的包。

``conda`` 可以隔离式地管理 ``Python`` 虚拟环境，各个虚拟环境之间互不影响。 本教程利用 ``Miniconda`` 管理 ``Python`` 开发环境。

安装Miniconda和基本配置
--------------------------
1. 下载安装包：``Miniconda`` 安装包可以到 `清华大学miniconda镜像 <https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/>`_ 下载 ``Miniconda3-latest-Linux-x86_64.sh``。

2. 安装：

.. code-block:: bash

  # 按默认的安装到 $HOME/miniconda3 目录下即可
  bash ~/Downloads/Miniconda3-latest-Linux-x86_64.sh

3. 激活 ``conda`` 命令：

.. code-block:: bash

  # 如果用的是 bash
  echo "\nsource $HOME/miniconda3/etc/profile.d/conda.sh\n" >> ~/.bashrc
  source ~/.bashrc

  # 如果用的是 zsh
  echo "\nsource $HOME/miniconda3/etc/profile.d/conda.sh\n" >> ~/.zshrc
  source ~/.zshrc

4. 修改 ``conda`` 下载软件包的镜像源：先执行命令 ``conda config --set show_channel_urls yes`` 创建文件 ``~/.condarc``，再修改 ``~/.condarc`` 文件为如下内容：

.. code-block:: none

  show_channel_urls: true
  channels:
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo 
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/simpleitk


5. 如果使用 ``Zsh``，添加 ``conda`` 命令补全功能：

.. code-block:: bash

  git clone https://github.com/esc/conda-zsh-completion.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/conda-zsh-completion

之后在 ``~/.zshrc`` 文件的 ``plugins`` 中 添加 ``conda-zsh-completion``。



conda基本使用
---------------
conda 在安装好后，自带一个名字叫 ``base`` 的虚拟环境。

激活/退出虚拟环境
^^^^^^^^^^^^^^^^^^

.. code-block:: bash

  conda activate 环境名字   # 激活 虚拟环境
  
  # 例如 激活 base
  conda activate base 

  conda deactivate  # 退出 当前虚拟环境


创建/删除 虚拟环境
^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

  conda create -n 环境名字 python=x.x   # 创建虚拟环境
  conda remove -n 环境名字 --all        # 删除虚拟环境
  
  # 例如 创建一个基于 python3.6的 名字叫ccy的 虚拟环境
  conda create -n ccy python=3.6

  # 例如 删除 名为ccy的 虚拟环境
  conda remove -n ccy --all

搜索/安装/删除包
^^^^^^^^^^^^^^^^^

.. code-block:: bash

  conda search 包名字   # 搜索包
  # 例如 搜索 opencv
  conda search opencv

  conda install -n 环境名字 包名字    # 安装包
  # 例如 在虚拟环境ccy中 安装 opencv
  conda install -n ccy opencv 
  # 指定安装包的版本
  conda install -n ccy opencv=3.4.7
  # 指定安装包的源
  conda install --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge -n base numpy
  # 从指定源安装指定版本的包
  conda install --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge -n base numpy=1.19.1

  # 从虚拟环境ccy中 删除 numpy包
  conda remove -n ccy numpy


VsCode开发Python
------------------
基本配置
^^^^^^^^^^
1. 安装 ``Python`` 插件： 插件支持 ``.py`` 的普通 ``Python`` 文件，也支持 ``.ipynb`` 的 ``Jupyter Notebook`` 文件

2. 在想用的 ``Python`` 环境中安装 ``Python Linter``, ``Python`` 智能提示工具，例如：

.. code-block:: bash

  conda install -n base pylint


编辑代码
^^^^^^^^^^^
1. 打开已有的 ``Python`` 文件 或者 用 ``VsCode`` 新建一个 ``Python`` 文件。

2. 选择 ``Python`` 环境：按 ``Ctrl+Shift+P`` 打开命令栏，输入 ``Python: Select Interpreter`` 命令，或者点击底部状态栏的 ``Select Python Interpreter`` 来选择 ``Python`` 环境：

.. image:: /_static/images/python-1.png

3. 可以开始写代码了


Debug代码
^^^^^^^^^^^^
1. 点击左边工具栏的 ``运行``，点击 ``创建launch.json文件``，在弹出的界面中依次选择 ``Python`` 和 ``Python file``，``VsCode`` 会在 ``.vscode`` 目录下创建 ``launch.json`` 文件： 

.. image:: /_static/images/python-2.png

2. 修改 ``launch.json`` 文件中的 ``program`` 项为自己想要调试的文件路径。``${file}`` 代表 ``VsCode`` 当前正在编辑的文件。

.. image:: /_static/images/python-3.png

3. 在 ``Python`` 代码中设置好断点后，按 ``F5`` 即可调试代码：

.. image:: /_static/images/python-4.png

