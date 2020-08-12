在VsCode中管理和开发Docker
--------------------------
``VsCode`` 官方支持对 ``Docker`` 的镜像和容器进行管理，同时可以将 ``Docker`` 容器当作远程开发环境进行开发。
以下内容参考 `官方文档 <https://code.visualstudio.com/docs/containers/overview>`_ 而写。


管理镜像和容器
^^^^^^^^^^^^^^^^^^^^^^^^

.. _vscode-docker:

安装Docker插件
>>>>>>>>>>>>>>>
在 ``VsCode`` 内安装 ``Docker`` 插件，该插件提供了对镜像和容器的管理功能，以及 ``Dockerfile`` 文件的高亮和智能补全：

.. image:: /_static/images/docker-14.png

安装后会在左侧工具栏出现对应的图标：

.. image:: /_static/images/docker-15.png


管理镜像
>>>>>>>>>
插件的镜像界面会列出当前系统中的镜像（不包括虚悬镜像）。对某个镜像 ``鼠标右键`` 可以进行 ``启动`` 等操作：

.. image:: /_static/images/docker-16.png


管理容器
>>>>>>>>>>
插件的容器界面会列出当前系统中的容器，(绿色为运行中的镜像，红色为停止的镜像）。

对某个容器 ``鼠标右键`` 可以进行 ``启动``、``删除``、``停止`` 等操作，利用该功能可以方便实现对 :red:`镜像构建失败遗留的容器` 进行删除操作：

.. image:: /_static/images/docker-17.png


Docker命令
>>>>>>>>>>>>
许多最常见的 ``Docker`` 命令都内置在命令面板(``Ctrl+Shift+P``)中：

.. image:: /_static/images/docker-18.png

可以运行 ``Docker`` 命令来管理 ``镜像`` 、``容器``、 ``仓库``、``网络``、``数据卷``。

另外， ``Docker: Prune System`` 命令将 **删除停止的容器、虚悬镜像以及未使用的网络和数据卷**。


.. _vscode-docker-remote:

连接容器
^^^^^^^^^^
``VsCode`` 可以将本地镜像，当成是一个远程开发环境进行访问。只需要安装插件 ``Remote - Containers`` 即可：

.. image:: /_static/images/docker-19.png

安装完插件后会在底部工具栏的最左边出现 ``远程连接`` 的图标，点击该图标后选择 ``Attach to Running Container``，最后选择想要连接的容器即可让 ``VsCode`` 访问目标容器：

.. image:: /_static/images/docker-20.png

利用 ``VsCode`` 在 ``Docker`` 环境下开发代码的实例详见 :ref:`docker-example`
