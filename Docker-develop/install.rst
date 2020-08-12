Docker安装和基本配置
---------------------
``Docker`` 分为 ``CE`` 和 ``EE`` 两大版本。``CE`` 即社区版（免费，支持周期 7 个月），``EE`` 即企业版，强调安全，付费使用，支持周期 24 个月。

官方网站上有各种环境下的 `安装指南 <https://docs.docker.com/engine/install/>`_，这里主要介绍 ``Docker CE`` 在 ``Ubuntu`` 上的安装和配置。

.. _install-docker:

安装 Docker CE
^^^^^^^^^^^^^^^^^
安装 ``Docker CE`` 建议使用 ``Docker`` 官方软件源，而不要安装 ``Ubuntu`` 软件源中的 ``Docker`` 版本。
清华大学镜像站有 ``Docker`` 官方软件源的同步镜像，安装时使用 `清华源 <https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/>`_ 会更快：

**1. 添加** ``Docker`` **软件源的** ``GPG`` **公钥：**

.. code-block:: bash
    
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 

**2. 添加软件源：**

对于 ``amd64`` 架构的计算机，运行

.. code-block:: bash

  sudo add-apt-repository "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

对于 树莓派或其它 ``ARM`` 架构计算机，运行

.. code-block:: bash

  sudo add-apt-repository "deb [arch=armhf] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list


**3. 安装** ``Docker CE``：

.. code-block:: bash

  sudo apt update   # 更新软件源
  sudo apt install --no-install-recommends docker-ce  # 安装 docker

**4. 修改执行权限(可选)：** 在安装完 ``Docker`` 后，默认普通用户是没有运行 ``docker`` 命令的权限的，运行时需要加 ``sudo``。 

``Docker`` 在安装后会创建一个名为 ``docker`` 的新用户组，可以将当前用户添加进 ``docker`` 用户组，以获得运行 ``docker`` 命令的权限：

.. code-block:: bash

  sudo groupadd docker            # 创建 docker 用户组，一般安装完 Docker后会自动创建，执行后会提示该用户组已存在
  sudo usermod -aG docker $USER   # $USER 环境变量可以获取当前用户名

重启系统后，可以不加 ``sudo`` 也能执行 ``docker`` 命令。如果想立即使用，可以切换当前用户组实现：

.. code-block:: bash

  newgrp docker   # 切换到 docker 用户组


Zsh 命令补全
^^^^^^^^^^^^^^^
对于使用 ``Zsh``，可以开启 ``oh-my-zsh`` 内置的 ``docker-compose`` 插件进行 ``docker`` 命令的补全功能，

.. image:: /_static/images/docker-3.png
  :align: center

在新的终端中输入 ``docker`` 后使用 ``Tab``：

.. image:: /_static/images/docker-4.png
  :align: center

如果补全无效，则尝试删除主目录下的隐藏文件 ``rm -r ~/.zcompdump*`` 后，打开新的终端再尝试。

