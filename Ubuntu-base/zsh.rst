Zsh安装(可选)
--------------


Zsh简介
^^^^^^^^^^^^
大部分的 ``Linux`` 发行版的默认 ``Shell`` 解释器是 ``Bash``。相比于默认的 ``Bash``，``Zsh`` 有更多的自定义选项，并支持扩展。因此 ``Zsh`` 可以实现更强大的命令补全，命令高亮等一系列酷炫功能。

例如 ``Tab`` 补全时大小写模糊：

.. image:: /_static/images/zsh-1.gif

例如 ``Tab`` 后，选项可以用方向键进行选择：

.. image:: /_static/images/zsh-2.gif

Zsh安装和配置
^^^^^^^^^^^^^^^^^
**1. 安装** ``Zsh``：

.. code-block:: bash

  sudo apt install --no-install-recommends zsh

**2. 配置** ``Zsh``： 默认的 Zsh 配置有点麻烦。因此一个名为 ``robbyrussel`` 的用户在 ``GitHub`` 上制作了一个配置文件 ``oh-my-zsh``，这是目前为止最流行的 ``Zsh`` 配置:

2.1 把 ``oh-my-zsh`` 项目 ``clone`` 下来

.. code-block:: bash

  git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh


2.2 复制 ``.zshrc``

.. code-block:: bash

  cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc


2.3 更改默认的 ``Shell``

.. code-block:: bash

  chsh -s $(which zsh)    # 将 Zsh 作为默认的 shell   重启系统生效

重启系统后终端变为 ``Zsh``。


oh-my-zsh更新和卸载
^^^^^^^^^^^^^^^^^^^^^
安装 ``oh-my-zsh`` 后，如果在联网状态下开机，``upgrade_oh_my_zsh`` 脚本会自动检查更新。如果有更新，打开终端后就会有提醒，是否更新。也可以通过命令行脚本检查更新：

.. code-block:: bash

  upgrade_oh_my_zsh     # 更新 oh-my-zsh

  uninstall_oh_my_zsh   # 卸载 oh-my-zsh


oh-my-zsh安装插件
^^^^^^^^^^^^^^^^^

``oh-my-zsh`` 的自带插件都储存在 ``~/.oh-my-zsh/plugins`` 目录中，基本上都是 :orange:`命令补全` 和 :orange:`命令增强`。
如果想安装一个第三方插件，需要：

1. 先将插件 ``git clone`` 到 ``~/.oh-my-zsh/custom/plugins`` 目录之中:

.. code-block:: bash

  git clone 插件github地址 ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/插件名称

2. :red:`激活插件`： 需要在 ``~/.zshrc`` 的 ``plugins=(xxx xxx ...)`` 这一行里加入插件名称，如下图所示:

.. image:: /_static/images/zsh-2.png
  :align: center

:orange:`推荐插件`：

* `zsh-syntax-highlighting <https://github.com/zsh-users/zsh-syntax-highlighting>`_ ： shell的语法高亮。除此之外，此插件还能够验证命令的正确性。
* `zsh-autosuggestions <https://github.com/zsh-users/zsh-autosuggestions>`_ ： 根据命令的历史记录，自动提示输入提示，使用 ``方向右键`` 来将建议的历史整条填充到命令行输入，或者  ``Ctrl+方向右键`` 填入下个字符串：

.. image:: /_static/images/zsh-3.gif
  :align: center

* sudo(内置) : 连按两下 ``Esc`` 按键 可以在命令的头部插入 ``sudo``：

.. image:: /_static/images/zsh-4.gif
  :align: center




Zsh修改主题
^^^^^^^^^^^^^^^

内置主题
>>>>>>>>>>>
Zsh 支持主题修改。 ``oh-my-zsh``内置了大量主题，存放在 ``~/.oh-my-zsh/themes`` 目录下。

Zsh打开 ``~/.zshrc`` 文件，可以看到有一行为 ``ZSH_THEME="robbyrussell"`` 的脚本，如下图所示：

.. image:: /_static/images/zsh-3.png

可以看到 ``Zsh`` 默认主题为 ``robbyrussell``，修改该字段就可以修改主题。前往 ``oh-my-zsh`` 的 `wiki <https://github.com/robbyrussell/oh-my-zsh/wiki/Themes>`_ 就可以看到大多数 ``oh-my-zsh`` 的内置主题以及它们的截图。
如果看中的其中的一款，可以通过修改 ``~/.zshrc`` 中的 ``ZSH_THEME="xxx"`` 来修改主题。


非内置主题
>>>>>>>>>>>
很多主题不存在于 ``Zsh`` 的内置主题之中，例如 `Powerlevel10k <https://github.com/romkatv/powerlevel10k>`_，需要自行去了解安装和配置方法。
