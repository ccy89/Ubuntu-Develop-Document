Ubuntu基础软件安装
--------------------------

修改镜像源
^^^^^^^^^^^^
方法一(常用)
>>>>>>>>>>>>>>>>>>>>
利用 **Ubuntu** 自带的软件 ``Software & Updates`` 修改源：

1. 打开软件 ``Software & Updates``：

.. image:: /_static/images/base-1.png

2. 切换源：

.. image:: /_static/images/base-1.gif

方法二(简洁)
>>>>>>>>>>>>>>>>
打开终端，执行如下命令：

.. code-block:: bash

  sudo sed -i "s|http://.*ubuntu.com|http://mirrors.tuna.tsinghua.edu.cn|g" /etc/apt/sources.list   # 更换镜像源为清华源
  sudo apt update   # 更新镜像源


更新系统和软件包
^^^^^^^^^^^^^^^^^^^
安装完系统后建议将系统Package更新到最新状态：

.. code-block:: bash

  sudo apt update   # 更新镜像源
  sudo apt upgrade  # 升级软件包
  sudo apt autoremove --purge   # 清除无效的依赖包

建议在个人电脑上定期更新Package，在服务器上只在装机完成后更新一次即可。


安装NVIDIA显卡驱动
^^^^^^^^^^^^^^^^^^^^
.. warning::
  如果要做深度学习，在安装 ``CUDA`` 的时候会自动安装 ``NVIDIA`` 的显卡驱动，因此请跳过该步骤！！！

通过 ``Ubuntu`` 自带的软件 ``Software & Updates`` 就可以安装 ``NVIDIA`` 显卡驱动。

在 ``Software & Updates`` 中选择 ``Additional Drivers``，选中想安装的显卡驱动，点击 ``Apply Changes`` 后就会安装显卡驱动。等待安装完成后，重启系统，显卡驱动就会生效。

.. image:: /_static/images/base-2.png

执行下面的命令确定驱动信息：

.. code-block:: bash

  nvidia-smi    # 查看 显卡信息

.. image:: /_static/images/base-3.png


安装中文输入法
^^^^^^^^^^^^^^^^^
安装英文系统默认是不带中文输入法的。``Ubuntu 18.04`` 之后的默认输入法引擎为 ``ibus``。安装 ``ibus`` 中文输入法：

.. code-block:: bash

  sudo apt install --no-install-recommends ibus-libpinyin   # 安装 ibus 中文输入法

:blue:`重启系统之后`，在 ``Settings`` 之中选择 ``Region & Language`` 选项，添加中文输入法：

.. image:: /_static/images/base-2.gif

添加完中文输入法后，可以通过桌面右上角的输入法选项对输入法进行配置：

.. image:: /_static/images/base-4.png

:green:`建议打开` ``Remember every input as a phrase`` :green:`功能，该功能会让输入法记录用户已经输入过但不存在与内置词典中的短语：`

.. image:: /_static/images/base-5.png



安装Chrome浏览器
^^^^^^^^^^^^^^^^^^^^
``Ubuntu`` 自带 ``Firefox`` 浏览器，由于后续代理配置相关内容使用 ``Chrome`` 浏览器较为方便，因此建议更改浏览器。

``Chrome`` 浏览器下载地址：`chrome官网 <https://www.google.cn/chrome/>`_，点击 ``下载Chrome`` ，选择 ``64位.deb包``，最后点 ``接受并安装``。 下载好的安装包默认放在 ``~/Downloads`` 目录下，执行安装命令：

.. code-block:: bash

  sudo dpkg -i ~/Downloads/google-chrome-stable_current_amd64.deb   # 安装 chrome浏览器
  sudo apt remove --purge firefox*    # 卸载 firefox
  sudo apt autoremove --purge         # 清除无效的依赖包
  rm -r .mozilla .cache/mozilla       # 删除 firefox的配置文件和缓存 


安装git
^^^^^^^^^^^^

可以直接安装 ``Ubuntu`` 官方源提供的版本：

.. code-block:: bash

  sudo apt install --no-install-recommends git

也可以安装 ``git`` 官网的版本：

.. code-block:: bash

  sudo add-apt-repository ppa:git-core/ppa      # 添加git官方源
  sudo apt update                               # 更新镜像源 
  sudo apt install --no-install-recommends git


Linux Kernel 安装和卸载
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
在 ``Ubuntu`` 更新系统和软件包的时候有时会更新 ``Ubuntu`` 的 ``Linux Kernel``，导致系统存在冗余的 ``Kernel``。
由于 ``Ubuntu`` 的部分包为了保证能够在系统所有 ``Kernel`` 上工作，例如显卡驱动，在安装时会在各个 ``Kernel`` 上编译一遍，生成对应的文件。如果 ``Kernel`` 太多，这个过程会很慢。

一般来说系统里面只需要保留两个版本的 ``Linux Kernel`` (一老一新)。

利用 ``dpkg -l | grep linux-image`` 查看当前系统所有的 ``Kernel``

.. image:: /_static/images/base-6.png

可以看到当前系统有 ``4.18.0-25`` 和 ``5.4.0-42`` 两个版本的内核。

利用 ``uname -r`` 可以查看当前正在使用的 ``Kernel`` 版本：

.. image:: /_static/images/base-7.png


卸载 Kernel
>>>>>>>>>>>>>
.. code-block:: bash

  # x.xx.x-xx 为内和版本，例如 4.18.0-25
  sudo apt remove --purge linux-headers-x.xx.x-xx linux-headers-x.xx.x-xx-generic linux-image-x.xx.x-xx-generic linux-modules-x.xx.x-xx-generic linux-modules-extra-x.xx.x-xx-generic

安装 Kernel 
>>>>>>>>>>>>>>
.. code-block:: bash

  # x.x.x-x 为内和版本，例如 4.18.0-25
  sudo apt install --no-install-recommends linux-headers-x.x.x-x linux-headers-x.x.x-x-generic linux-image-x.x.x-x-generic linux-modules-x.x.x-x-generic linux-modules-extra-x.x.x-x-generic


禁止 Kernel 更新
>>>>>>>>>>>>>>>>>>>
如果嫌自己后续维护系统太麻烦，可以禁止 ``Kernel`` 更新。

.. code-block:: bash

  # x.x.x-x 为内核版本，例如 4.18.0-25
  sudo apt-mark hold linux-image-x.x.x-x-generic    # 禁止 内核更新

  # x.x.x-x 为内核版本，例如 4.18.0-25
  sudo apt-mark unhold linux-image-x.x.x-x-generic  # 重启 内核更新


