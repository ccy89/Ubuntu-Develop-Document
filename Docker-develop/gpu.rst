.. _nvidia-docker:

NVIDIA GPU 支持
----------------
``NVIDIA`` 对 ``Docker`` 在 ``Linux`` 系统下有全方面的支持（不支持 ``MacOS`` 和 ``Windows``）。
`NVIDIA Container Toolkit <https://github.com/NVIDIA/nvidia-docker>`_ 允许用户构建和运行使用 ``GPU`` 功能的 ``Docker`` 容器。

.. warning:: 
  使用 ``NVIDIA Container Toolkit`` 需要先安装显卡驱动！！！

以下的安装教程参考 `官方 wiki <https://github.com/NVIDIA/nvidia-docker/wiki>`_ 来写：

安装 NVIDIA Container Toolkit
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: bash

  curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -   # 添加 gpg密钥
  distribution=$(. /etc/os-release;echo $ID$VERSION_ID)   # 获得 Ubuntu版本名称
  curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list  # 添加源
  sudo apt update     # 更新源
  sudo apt install nvidia-container-toolkit   # 安装 NVIDIA Container Toolkit
  sudo systemctl restart docker   # 重启 docker 服务，如果不生效，可以重启系统来生效

安装完成后可以重启系统，使 ``NVIDIA Container Toolkit`` 生效。运行下面命令，测试安装是否成功：

.. code-block:: bash

  docker pull ubuntu:18.04    # 下载 ubuntu:18.04 镜像
  docker run --rm --gpus all ubuntu:18.04  nvidia-smi   # 运行一个容器，输出显卡信息 

.. image:: /_static/images/docker-5.png


在容器中使用 NVIDIA GPU
^^^^^^^^^^^^^^^^^^^^^^^^
想要在容器内使用 ``NVIDIA`` 的 ``GPU`` 需要设置环境变量，环境变量有 ``NVIDIA_VISIBLE_DEVICES`` 和 ``NVIDIA_DRIVER_CAPABILITIES``

* ``NVIDIA_VISIBLE_DEVICES`` 用于设置使用哪一张 ``GPU``。设置为 ``0`` 代表使用设备号为 ``0`` 的 ``GPU``，设置为 ``all`` 代表使用所有的 ``GPU``。
* ``NVIDIA_DRIVER_CAPABILITIES`` 用于设置使用 ``GPU`` 的哪些功能。选项有 ``graphics``、``compute``、``utility`` 和 ``video``。

环境变量可以在 ``Dockerfile`` 或者 ``docker run`` 的时候设置。

Dockerfile
>>>>>>>>>>>>>>>
.. code-block:: Dockerfile
 
  ENV NVIDIA_VISIBLE_DEVICES all
  ENV NVIDIA_DRIVER_CAPABILITIES compute,utility,graphics,video

docker run
>>>>>>>>>>>>>>>
.. code-block:: bash

  # --gpus 可以替代 -e NVIDIA_VISIBLE_DEVICES
  docker run -it --rm --gpus all -e NVIDIA_DRIVER_CAPABILITIES=compute,video,utility,graphics 镜像名 bash