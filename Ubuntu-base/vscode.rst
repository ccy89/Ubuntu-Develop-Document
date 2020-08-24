VsCode安装与配置
--------------------

``VsCode`` 是 ``MicroSoft`` 推出的一款 :red:`免费的全平台文本编辑器`，由于其可以通过安装插件的方式实现各种功能，相比于传统的 ``IDE`` 软件，具有小巧高效的优点。

之后所有的开发任务都将基于 ``VsCode`` 进行。

安装
^^^^^^^^^^^
直接去 `VsCode官网 <https://code.visualstudio.com/>`_ 上下载 ``.deb`` 包，下载完后执行安装命令：

.. code-block:: bash

  # 注意这里的 deb包名称用Tab补全成自己下载的
  sudo dpkg -i ~/Downloads/code安装包.deb      # 安装 vscode浏览器
  sudo apt remove --purge gedit gedit-common  # 卸载 Ubuntu默认文本编辑器gedit
  sudo apt autoremove --purge                 # 清除无效的依赖包
  rm -r .config/gedit                         # 删除 gedit的配置文件


安装拓展（插件）
^^^^^^^^^^^^^^^^^^
点击 ``VsCode`` 的 ``左侧边栏`` 的 ``Extensions(扩展)`` 选项可以进入插件的搜索和安装：

.. image:: /_static/images/vscode-1.png

:red:`推荐安装的插件：`

* Chinese (Simplified) Language Pack for Visual Studio Code  
* C/C++
* Python
* CMake


颜色主题设置
^^^^^^^^^^^^^^^^
``VsCode`` 可以修改颜色主题，其内置了部分颜色主题：

.. image:: /_static/images/vscode-2.png

也可以通过 ``插件`` 的形式安装其他的颜色主题。在插件中搜索 ``theme`` 就可以找到其他颜色主题：

.. image:: /_static/images/vscode-3.png


VsCode同步设置
^^^^^^^^^^^^^^^^^^^
在 ``VsCode 1.48`` 版本开始，同步设置的功能从预览版变为正式版。同步功能可以将 ``VsCode`` 的各项设置在云端备份，共享到不同的机器当中。
使用同步功能需要拥有一个 ``微软账号`` 或者 ``github`` 账号。

通过 ``左侧边栏`` 底部的 ``管理`` 齿轮菜单中的 ``打开设置同步...`` 条目来打开同步功能：

.. image:: /_static/images/vscode-4.png

打开后需要选择同步哪些配置，目前支持的有 ``设置`` 、``键盘快捷方式``、``扩展``、``用户代码片段`` 和 ``UI状态``：

.. image:: /_static/images/vscode-5.png

选好需要同步的项目后点击 ``登录并打开``， 选择想要使用的账户：

.. image:: /_static/images/vscode-6.png

登陆账户会跳转到浏览器界面，要求输入 ``用户`` 和 ``密码``，登陆成功后会出现提示网页，到这里同步功能就打开了。

:red:`如果你已经在另一台机器上打开过同步功能`，那么打开当前机器的同步功能后，则会显示以下 ``合并或替换`` 的对话框，用于同步或者合并云端的设置：

.. image:: /_static/images/vscode-7.png
