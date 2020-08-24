Ubuntu界面美化(可选)
----------------------

从 ``Ubuntu 18.04`` 开始 ``Ubuntu`` 系统采用 ``Gnome3`` 图形化界面。因此 ``Ubuntu`` 界面美化其实就是配置 ``Gnome3`` 图形界面的插件。

安装Ubuntu美化工具
^^^^^^^^^^^^^^^^^^^^
.. code-block:: bash

  sudo apt install --no-install-recommends gnome-tweak-tool     # gnome-tweak 用来管理gnome插件
  sudo apt install --no-install-recommends chrome-gnome-shell   # 在Chrome浏览器中安装gnome插件的工具


安装和配置Gnome插件
^^^^^^^^^^^^^^^^^^^^
``Gnome`` 插件下载安装地址：`extensions.gnome.org <https://extensions.gnome.org/>`_

 :orange:`安装插件的例子：`

1. 安装 ``Gnome`` 的插件只需要在搜索栏中搜索想要的插件，选择插件后，点击右上角类似开关的按键变为 ``ON`` 后，在跳出来的界面中点击 ``Install`` 即可：

.. image:: /_static/images/gnome-1.png
  :align: center

.. image:: /_static/images/gnome-2.png
  :align: center

.. image:: /_static/images/gnome-3.png
  :align: center


2. 安装好插件后，可以用 ``gnome-tweak`` 配置插件。在终端输入命令 ``gnome-tweaks`` 或者通过 ``Dash`` 栏中的 ``Show Applications`` 找到 ``Tweaks`` 可以调出该工具。
选择 ``Extensions``，可以对安装的插件进行配置(:orange:`自己去折腾吧`)：

.. image:: /_static/images/gnome-4.png
  :align: center

:orange:`推荐插件：`

* **User Themes**：允许 ``Gnome Shell`` 使用 桌面的 ``Theme`` 
* **Dash to Dock**： 将 ``Ubuntu`` 的 ``Dash`` 改成 ``Dock`` 风格
* **Hide Top Bar**：可以隐藏顶栏
* **Appfolders Management extension**：可以对 ``Ubuntu`` 的应用分组，``Ubuntu 20.04`` 自带该功能。


修改Ubuntu桌面主题
^^^^^^^^^^^^^^^^^^^^^^^^
``Ubuntu`` 的桌面主题存放在 ``/usr/share/themes`` 目录下。通过 ``gnome-tweak`` 中的 ``Appearance`` 可以变更桌面主题：

.. image:: /_static/images/gnome-5.png
  :align: center

``Ubuntu`` 内置一些桌面主题（除了 ``Ubuntu 20.04`` 的 ``Yaru`` 主题，一般都挺丑的），也可以在网上(`pling-theme <https://www.pling.com/browse/cat/366/order/latest/>`_)下载第三方的主题。下载好的主题解压出来后拷贝到 ``/usr/share/themes`` 目录下，就可以通过 ``gnome-tweak`` 进行修改。

修改Ubuntu Icon(图标)主题
^^^^^^^^^^^^^^^^^^^^^^^^^^
``Ubuntu`` 的 ``Icon`` 主题存放在 ``/usr/share/icons`` 目录下。通过 ``gnome-tweak`` 中的 ``Appearance`` 可以变更 ``Icon`` 主题：

.. image:: /_static/images/gnome-6.png
  :align: center

``Ubuntu`` 内置一些 ``Icon`` 主题（一般都挺丑的），也可以在网上(`pling-icon <https://www.pling.com/browse/cat/132/order/latest/>`_)下载第三方的主题。下载好的主题解压出来后拷贝到 ``/usr/share/icons`` 目录下，就可以通过 ``gnome-tweak`` 进行修改。


修改桌面壁纸主题
^^^^^^^^^^^^^^^^^^^^^^^^^^
通过 ``gnome-tweak`` 中的 ``Appearance`` 可以变更桌面背景和锁屏背景：

.. image:: /_static/images/gnome-7.png
  :align: center
