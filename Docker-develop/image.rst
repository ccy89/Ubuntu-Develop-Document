使用Docker镜像
--------------------
``Docker`` 镜像官方地址为 `Docker Hub <https://hub.docker.com/>`_，上面有大量的高质量的镜像可以用。

.. _image-accelerate:

Docker镜像加速
^^^^^^^^^^^^^^^^^
国内从 ``Docker Hub`` 拉取镜像速度十分缓慢，需要修改镜像源。``Docker`` 官方和国内很多云服务商都提供了国内加速器服务，例如：

* ``Docker`` **官方提供的中国源**： https://registry.docker-cn.com
* `百度云加速器 <https://cloud.baidu.com/doc/CCE/s/Yjxppt74z#%E4%BD%BF%E7%94%A8dockerhub%E5%8A%A0%E9%80%9F%E5%99%A8>`_： https://mirror.baidubce.com
* **阿里云加速器(需登录账号获取)**： https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors，加速地址如下图所示：

.. image:: /_static/images/docker-6.png

在 ``Docker`` 镜像源配置文件 ``/etc/docker/daemon.json`` 中写入以下内容（如果文件不存在需要新建该文件 ``sudo touch /etc/docker/daemon.json``）：

.. code-block:: json

  {
    "registry-mirrors": ["加速镜像源地址"]
  }

修改完之后重启 ``Docker`` 服务即可：

.. code-block:: bash

  sudo systemctl daemon-reload    # 重载所有修改过的配置文件
  sudo systemctl restart docker   # 重启 docker 服务


获取镜像
^^^^^^^^^^^^^^
之前提到过，``Docker Hub`` 上有大量的高质量的镜像可以用。从 ``Docker`` 镜像仓库获取镜像的命令是 ``docker pull``。其命令格式为：

.. code-block:: bash

  docker pull 仓库名:标签

其中镜像名是两段式名称，即 ``<仓库名>:<标签>`` 会对应一个镜像，例如：

.. image:: /_static/images/docker-7.png

上面的命令中没有给出 ``Docker`` 镜像仓库地址，因此将会从 ``Docker Hub`` 获取镜像。而镜像名称是 ``ubuntu:18.04``，因此将会获取官方镜像 ``library/ubuntu`` 仓库中标签为 ``18.04`` 的镜像。

从下载过程中可以看到分层存储的概念，镜像是由多层存储所构成。下载也是一层层的去下载，并非单一文件。下载过程中给出了每一层的 ``ID`` 的前12位。
并且下载结束后，给出该镜像完整的 ``sha256`` 的摘要，以确保下载一致性。


列出镜像
^^^^^^^^^^^^^
不加任何参数的情况下，``docker image ls`` 会列出所有顶层镜像：

.. image:: /_static/images/docker-8.png

列表包含了 ``仓库名``、``标签``、``镜像ID``、``创建时间`` 以及 ``镜像大小``。

.. note::
  这里标识的镜像大小和在 ``Docker Hub`` 上看到的镜像大小不同，是因为 ``Docker Hub`` 中显示的是压缩后的体积，本地显示的是解压后的大小

.. _dangling-image:

虚悬镜像
>>>>>>>>>>
上面的镜像列表中，还可以看到一个特殊的镜像，这个镜像的 ``仓库名`` 和 ``标签`` 均为 ``<none>``，这是一个 **虚悬镜像**。

**虚悬镜像(dangling image)** 是 :red:`自己构建失败的镜像` 或者 :red:`docker pull 新镜像时，失去作用的老镜像` 。一般来说，虚悬镜像已经失去了存在的价值，建议删除。
:red:`如果删除的虚悬镜像是由于自己构建镜像失败产生的,需要先删除构建失败所产生的容器，之后才能删除对应的虚悬镜像`。

.. image:: /_static/images/docker-9.png

.. code-block:: bash

  docker ps -a        # 查看系统中所有容器，根据构建失败的镜像ID 找到对应的容器ID
  docker rm 容器ID     # 删除容器，例如这里容器ID就是 e6596970f85d
  docker ps -a        # 再次查看系统中所有容器，应该看到容器被删除了

  docker image prune    # 自动删除所有虚悬镜像
  docker image ls       # 应该看到虚悬镜像已经不再了

中间层镜像
>>>>>>>>>>>>>
为了加速镜像构建、重复利用资源，``Docker`` 会利用 **中间层镜像**，尤其是自己构建镜像的时候，:red:`中间层镜像是无法直接删除的`。
如果希望显示包括中间层镜像在内的所有镜像的话，需要加 ``--all`` 参数，也可以简写为 ``-a``：

.. image:: /_static/images/docker-10.png

.. note::
  **虚悬镜像** 和 **中间层镜像** 的 ``仓库名`` 和 ``标签`` 均为 ``<none>``，区分两者的方法是命令行。
  在 ``docker image ls`` 就显示的是 **虚悬镜像**，在 ``docker image ls -a`` 才显示的是 **中间层镜像**。


删除本地镜像
^^^^^^^^^^^^^^
使用 ``docker rmi`` 命令可以删除本地镜像：

.. code-block:: bash

  docker rmi 镜像ID|镜像名    # 指定删除的镜像时可以使用 镜像ID 或者 镜像名

.. image:: /_static/images/docker-11.png

可以看到：:red:`当删除自己构建的镜像时，会将其依赖的中间层镜像全部删除。` 执行 ``docker image ls -a`` 应该可以看到中间层镜像已经被删除了。


镜像的导入和导出
^^^^^^^^^^^^^^^^^
``Docker`` 还提供了 ``docker save`` 和 ``docker load`` 命令，用以将镜像保存为一个文件，然后传输到另一个位置上，再加载进来。
这是在没有 ``Docker Registry`` 时的做法，现在已经不推荐，镜像迁移应该直接使用 ``Docker Registry``，无论是直接使用 ``Docker Hub`` 还是使用内网私有 ``Registry`` 都可以。

导出镜像
>>>>>>>>>
使用 ``docker save`` 命令可以将镜像保存为归档文件：

.. code-block:: bash

  docker save 镜像名|镜像ID -o filename
  # 例如
  docker save vins:latest -o vins.tar

这里的 ``filename`` 可以为任意名称甚至任意后缀名，但文件的本质都是归档文件，:red:`如果有同名文件，会直接覆盖，不会有任何提示。`


导入镜像
>>>>>>>>>
使用 ``docker load`` 命令可以从归档文件导入镜像：

.. code-block:: bash

  docker load -i filename
  # 例如
  docker load -i vins.tar
