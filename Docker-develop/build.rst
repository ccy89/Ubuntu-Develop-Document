使用 Dockerfile 构建镜像
--------------------------
Dockerfile 文件
^^^^^^^^^^^^^^^^

``Dockerfile`` 是一个文本文件，文件包含了一条条的 ``指令(Instruction)``，对应 ``Docker`` 的镜像是分层存储的，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。

``Docker`` 的指令很简单，下面会介绍常用的指令。

构建镜像命令
^^^^^^^^^^^^^
有了 ``Dockerfile`` 后，使用构建命令 ``docker build`` 就可以根据 ``Dockerfile`` 构建镜像：

.. code-block:: bash

  # [OPTIONS] 具体可以使用 docker build --help 来查看
  docker build [OPTIONS] PATH 
  
  # 使用当前目录的 Dockerfile 创建镜像，镜像名为 test:v1
  docker build -t test:v1 . 

  # 使用 URL github.com/creack/docker-firefox 的 Dockerfile 创建镜像
  docker build github.com/creack/docker-firefox

  # 通过 -f 指定 Dockerfile 文件的位置
  docker build -f Dockerfile路径 -t test:v1 .

:red:`如果构建镜像失败，将会在系统中产生无用的虚悬镜像和一个停止状态的容器，需要删除`。 删除虚悬镜像详见 :ref:`dangling-image` 章节。 


Dockerfile 指令
^^^^^^^^^^^^^^^^^^

FROM 指定基础镜像
>>>>>>>>>>>>>>>>>
所谓构建镜像，那一定是以一个镜像为基础，在其上进行构建，而 ``FROM`` 就是指定 **基础镜像**，因此一个 ``Dockerfile`` 中 ``FROM`` 是必备的指令，并且 :red:`必须是第一条指令。`：

.. code-block:: Dockerfile

  FROM 镜像名
  # 例如
  FROM ubuntu:18.04

RUN 执行命令
>>>>>>>>>>>>>
``RUN`` 指令是用来执行命令行命令的。由于命令行的强大能力，``RUN`` 指令在定制镜像时是最常用的指令之一。其主要格式为：

.. code-block:: Dockerfile

  RUN shell命令行
  # 例如
  RUN apt-get update
  RUN apt-get install -y gcc libc6-dev make wget
  RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
  RUN mkdir -p /usr/src/redis
  RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
  RUN make -C /usr/src/redis
  RUN make -C /usr/src/redis install

之前说过，``Dockerfile`` 中每一个指令都会建立一层，每一条 ``RUN`` 指令，都会在上一层基础上新建立一层，构成新的镜像。

而上面的这种写法，创建了7层镜像，这是完全没有意义的。
因为很多指令都是可以合并的，而且很多运行时不需要的东西，都被装进了镜像里，比如编译环境、更新的软件包等等。结果就是产生非常臃肿、非常多层的镜像，不仅仅增加了构建部署的时间，也很容易出错。

上面的 ``Dockerfile`` 正确的写法应该是这样：

.. code-block:: Dockerfile
  
  RUN buildDeps='gcc libc6-dev make wget' \
      && apt-get update \
      && apt-get install -y $buildDeps \
      && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
      && mkdir -p /usr/src/redis \
      && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
      && make -C /usr/src/redis \
      && make -C /usr/src/redis install \
      && rm -rf /var/lib/apt/lists/* \
      && rm redis.tar.gz \
      && rm -r /usr/src/redis \
      && apt-get purge -y --auto-remove $buildDeps

首先，之前所有的命令只有一个目的，就是编译、安装 ``redis`` 可执行文件。因此没有必要建立很多层，这只是一层的事情。
因此，这里没有使用很多个 ``RUN`` 对应不同的命令，而是仅仅使用一个 ``RUN`` 指令，并使用 ``&&`` 将各个所需命令串联起来，将之前的7层，简化为了1层。

此外，还可以看到这一组命令的最后添加了清理工作的命令，删除了为了编译构建所需要的软件，清理了所有下载、展开的文件，并且还清理了 ``apt`` 缓存文件。
这是很重要的一步，我们之前说过，镜像是多层存储，每一层的东西并不会在下一层被删除，会一直跟随着镜像。因此镜像构建时，:red:`一定要确保每一层只添加真正需要添加的东西，任何无关的东西都应该清理掉。`

WORKDIR 指定工作目录
>>>>>>>>>>>>>>>>>>>>
在构建镜像时有时需要切换工作目录，例如，想要在 ``/etc`` 目录下创建一个文件 ``test.txt``, 如果使用 ``RUN cd 工作目录`` 可能会出现问题：

.. code-block:: Dockerfile

  RUN cd /etc           
  RUN touch test.txt    

上面的结果： ``test.txt`` 文件会创建在根目录 ``/`` 之下，而不是在 ``/etc`` 之下。

这是因为之前说过每一个 ``RUN`` 都是启动一个容器、执行命令、然后提交存储层文件变更生成新镜像。
第1层 ``RUN cd /etc`` 的执行仅仅是当前进程的工作目录变更，内存上的变化而已，其结果不会造成任何文件变更。
而到第2层的时候，启动的是一个全新的容器，跟第一层的容器更完全没关系，自然不可能继承前一层构建过程中的内存变化。

为了解决上述问题，可以将所有需要在变更工作目录后执行的操作，写进一条指令里：

.. code-block:: Dockerfile

  RUN cd /etc && touch test.txt  

或者使用 ``WORKDIR`` 指令来指定工作目录（或者称为当前目录），:red:`以后各层的当前目录就被改为指定的目录`。同时，如该目录不存在，``WORKDIR`` 会帮你建立目录。该指令格式为:

.. code-block:: Dockerfile

  WORKDIR 工作目录路径
  
  # 例如
  WORKDIR /etc
  RUN touch test.txt 
  # 之后所有的操作都在 /etc 目录下进行

COPY|ADD 复制文件
>>>>>>>>>>>>>>>>>
``COPY`` 指令将本地源文件或目录拷贝到新的一层的镜像内的目标路径，比如：

.. code-block:: Dockerfile

  COPY 本地源文件或目录路径 镜像内的目标路径
  # 例如
  # 拷贝文件
  COPY package.json /usr/src/app/   
  # 拷贝目录
  COPY VINS-Mono /root/VINS-Mono

``ADD`` 指令和 ``COPY`` 的格式和性质基本一致。但是在 ``COPY`` 基础上增加了一些功能：

* 源文件 可以是一个 ``URL``， 即网上的文件。
* 如果 源文件 为一个 ``tar`` 压缩文件的话，压缩格式为 ``gzip``, ``bzip2`` 以及 ``xz`` 的情况下，``ADD`` 指令将会自动解压缩这个压缩文件到目标路径。


在 ``Docker`` 官方的 `Dockerfile 最佳实践文档 <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/>`_ 中要求，
尽可能的使用 ``COPY``，因为 ``COPY`` 的语义很明确，就是复制文件而已，而 ``ADD`` 则包含了更复杂的功能，其行为也不一定很清晰。

ENV 设置环境变量
>>>>>>>>>>>>>>>>>
``ENV`` 指令很简单，就是设置环境变量而已，无论是后面的其它指令，如 ``RUN``，还是容器运行时的应用，都可以直接使用这里定义的环境变量。其格式为：

.. code-block:: Dockerfile

  ENV 环境变量名 value
  ENV 环境变量名1=value1 环境变量名2=value2 ...

  # 例如
  ENV NVIDIA_VISIBLE_DEVICES all
  ENV NVIDIA_DRIVER_CAPABILITIES compute,utility,graphics,video

ARG 构建参数
>>>>>>>>>>>>>
``ARG`` 和 ``ENV`` 的效果一样，都是设置环境变量。所不同的是，``ARG`` 所设置的构建环境的环境变量，:red:`在将来容器运行时是不会存在这些环境变量的`。其格式为：

.. code-block:: Dockerfile

  ARG 参数名=默认值

  #例如
  ARG DEBIAN_FRONTEND=noninteractive

``Dockerfile`` 中的 ``ARG`` 定义的变量是可以在构建命令 ``docker build`` 中用 ``--build-arg 参数名=值`` 来覆盖。


构建镜像时使用代理
^^^^^^^^^^^^^^^^^^
构建镜像时会需要执行 ``RUN apt update && apt install ....`` 等需要联网获取数据的命令，
可以在构建时使用 ``--network``、``--build-arg http_proxy``、``--build-arg https_proxy`` 和 ``--build-arg no_proxy`` 来配置代理：

.. code-block:: Bash
  
  # --network host 指定使用宿主机的网络
  # --build-arg http_proxy 指定 http 代理
  # --build-arg https_proxy 指定 https 代理
  # --build-arg no_proxy 指定哪些地址不使用代理
  docker build --network host \
               --build-arg http_proxy=http://127.0.0.1:8118 \
               --build-arg https_proxy=http://127.0.0.1:8118 \
               --build-arg no_proxy=.tuna.tsinghua.edu.cn \
               -t test:latest .


构建镜像实例
^^^^^^^^^^^^
构建镜像实例请参照整个 ``Docker`` 教程最终的实例：:ref:`docker-example`
