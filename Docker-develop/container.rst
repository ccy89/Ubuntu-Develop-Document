操作容器 
^^^^^^^^^^^^^
有了镜像后，我们就能够以这个镜像为基础启动并运行一个或多个容器。以上面的 ``ubuntu:18.04`` 为例，对容器的操作进行介绍。

启动容器
>>>>>>>>>>
启动容器的主要命令为 ``docker run``：

.. code-block:: bash

  # 详细参数可以使用命令 docker run --help 查看
  docker run [OPTIONS] 镜像名字 [COMMAND] [ARG...]


例如，下面的命令将在容器中输出一个 “Hello World”，之后终止容器。

.. code-block:: bash

  docker run ubuntu:18.04 /bin/echo 'Hello world'               # 启动时没有指定容器名字，会自动分配一个名字
  docker run --name test ubuntu:18.04 /bin/echo 'Hello world'   # 启动时指定容器的名字

命令执行后会在终端输出 ``Hello World`` 后停止容器，这跟在本地直接执行 ``echo 'Hello world'`` 几乎感觉不出任何区别。

.. warning:: 
  默认情况下，停止的容器并不会立即删除，该容器依然存在与系统当中，:red:`即便重启系统也依然不会删除，除非用户手动删除`

.. note::
  个人理解：这样的好处是即使系统崩溃或断电后，用户依然可以随时重启容器继续开发。

利用下面的命令可以查看系统中存在的容器：

.. code-block:: bash

  docker ps         # 显示正在运行的容器
  docker ps --all   # 显示所有容器，包括已经停止但没有删除的容器
  docker ps -a      # 上一条命令的简化版

.. image:: /_static/images/docker-12.png

可以看到，容器依然存在于系统当中。利用 ``start`` 命令可以启动已经停止的容器：

.. code-block:: bash

  # -i 是指 启动容器并进入交互模式，如果没有 -i，终端只会输出容器的ID
  # 其他的命令参数可以用 docker start --help 来查看
  docker start -i 容器ID|容器名     # 容器ID 或者 容器名 可以在输入命令的时候利用 TAB 进行补全查看

.. image:: /_static/images/docker-13.png

**如果希望停止容器的时候同时删除容器**，可以增加命令行参数 ``--rm`` 实现：

.. code-block:: bash

  docker run --rm ubuntu:18.04 /bin/echo 'Hello world'  # 停止容器时会删除容器
  docker ps --all   # 显示所有容器时将不会出现新的容器


停止容器
>>>>>>>>>>
使用命令 ``docker stop`` 可以停止一个或多个正在运行的容器：

.. code-block:: bash

  # OPTIONS 具体可以通过 docker stop --help 查看
  docker stop [OPTIONS] 容器ID|容器名   # 容器ID 或者 容器名 可以在输入命令的时候利用 TAB 进行补全查看


删除容器
>>>>>>>>>>>>
**容器造作终止** 或者 **构建镜像失败** 都会产生停止状态的容器。
停止的容器默认情况下并不会立即删除，需要手动删除。删除容器的命令为 ``docker rm``，可以删除一个或多个容器：

.. code-block:: bash

  # OPTIONS 具体可以通过 docker rm --help 查看
  docker rm [OPTIONS] 容器ID|容器名   # 容器ID 或者 容器名 可以在输入命令的时候利用 TAB 进行补全查看
  
  # 例如 删除之前创建的两个容器
  docker rm 0dbabd535632 test   # 删除 ID 为 0dbabd535632 的容器
  docker rm test                # 删除名字叫 test 的容器
  docker ps -a                  # 应该看不见之前的两个容器


进入容器
>>>>>>>>>>>>>>

如果需要进入容器内，进行一些交互式的操作，有2种方式。

**1.启动容器的时候就直接进入容器。** 利用参数 ``-it`` 就可以在启动容器的同时进入容器： 

.. code-block:: bash

  # -i 和容器进行交互， -t 分配一个伪终端
  docker run -it [OPTIONS] 容器ID|容器名   # 容器ID 或者 容器名 可以在输入命令的时候利用 TAB 进行补全查看
  
  # 例如
  docker run -it --rm --name example ubuntu:18.04

执行完命令后就进入到容器内部了，按 ``Ctrl`` + ``D`` 可以停止容器，由于加了参数 ``--rm``，在停止容器后会自动删除容器。

**2.进入一个已经启动的容器。** 利用命令 ``docker exec`` 可以进入已经启动的容器或者让已经启动的容器执行某条命令：

.. code-block:: bash

  # OPTIONS 具体可以通过 docker exec --help 查看
  docker exec [OPTIONS] 容器ID|容器名 COMMAND [ARG...]  # 容器ID 或者 容器名 可以在输入命令的时候利用 TAB 进行补全查看

  # 例如
  docker run -it --rm --name example ubuntu:18.04   # 先启动一个容器

  # 在一个新的终端输入下面命令
  docker exec example apt update  # 让容器执行 apt update 命令
  docker exec -it example bash    # 进入容器的 bash 终端
  
.. warning::
  如果用命令 ``docker exec`` 让容器执行命令或者进入容器，**当退出容器时，容器是不会进入停止状态的**。

  如果用命令 ``docker run -it`` 进入容器，**退出容器的同时会停止容器**，如果带参数 ``--rm``，则容器会被删除。


容器文件目录拷贝
>>>>>>>>>>>>>>>>
命令 ``docker cp`` 可以实现本地系统和容器之间文件或者目录的复制拷贝操作：

.. code-block:: bash

  # 从容器复制文件或目录到本地
  docker cp 容器名|容器ID:容器内路径 本地路径
  # 例如
  docker cp vins:/root/catkin_ws ~/  # 将容器vins中的 /root/catkin_ws 拷贝至 本地 ~/catkin_ws

  # 从本地复制文件或路径到容器
  docker cp 本地路径 容器名|容器ID:容器内路径
  # 例如
  docker cp  ~/Downloads/MH_05_difficult.bag vins:/root   # 将本地文件 ~/Downloads/MH_05_difficult.bag 拷贝至 容器内 /root目录之下


容器挂载本地文件或目录
>>>>>>>>>>>>>>>>>>>>>>>
``-v`` 或者 ``--mount`` 参数可以直接在容器启动的时候挂载本地文件或者目录。

``-v`` 参数使用非常简单：

.. code-block:: bash

  docker run -v 本地路径:容器内路径 镜像名 
  # 例如 
  # 启动容器时挂载本地目录，启动容器后执行在挂载目录下创建文件 test.txt
  docker run --rm -v $HOME/Documents:/root/Documents ubuntu:18.04 touch /root/Documents/test.txt  

执行上面的命令后，可以在 ``~/Documents`` 目录下查看是否多了一个 ``test.txt`` 文件。

``--mount`` 参数使用比较复杂，但是功能更强大：

.. code-block:: bash

  docker run --mount type=挂载类型,src=源路径,dst=容器内路径 镜像名 

```type` 可以是 ``bind``、``volume`` 或者 ``tmpfs``，这里只介绍 ``bind``，用于挂载本地文件或目录：

.. code-block:: bash

  docker run --rm --mount type=bind,src=$HOME/Documents,dst=/root/Documents ubuntu:18.04 touch /root/Documents/example.txt

同样，执行上面的命令后，可以在 ``~/Documents`` 目录下查看是否多了一个 ``example.txt`` 文件。

.. note:: 
  以上两者的区别在于：

  使用 ``-v`` 参数时，如果本地目录不存在 ``Docker`` 会自动为你创建一个文件夹。
  
  使用 ``--mount`` 参数时，如果本地目录不存在，``Docker`` 会报错。

在后台运行容器
>>>>>>>>>>>>>>>
如果希望容器在后台运行，可以增加命令行参数 ``-d`` 实现：

.. code-block:: bash

  # 启动容器在后台运行
  docker run -d ubuntu:18.04 /bin/echo 'Hello world'    # 该命令在后台运行后，容器会停止
  docker run -dit ubuntu:18.04 bash                     # -dit 是 -d -i -t 的简写，该命令在后台运行后，容器会一直在后台处于启动状态

  # 让启动的容器在后台执行命令
  docker exec -d 容器ID|容器名 shell命令

导出容器
>>>>>>>>>>>>
导出某个容器为容器快照文件，可以使用 ``docker export`` 命令： 

.. code-block:: bash

  docker export 容器ID|容器名 -o filename
  # 例如
  docker export 3f0c5aa886bc > test.tar


导入容器为镜像
>>>>>>>>>>>>>>>
可以使用 ``docker import`` 从容器快照文件中再导入为镜像：

.. code-block:: bash

  docker import file|URL 镜像名
  # 例如
  docker import  my_ubuntu_v3.tar runoob/ubuntu:v4 
  docker import http://example.com/exampleimage.tgz example/imagerepo   # 从链接导入镜像


容器中使用网络代理
>>>>>>>>>>>>>>>>>>>>
在启动容器的时候，可以利用参数 ``--network``、 ``-e http_proxy``、``-e https_proxy`` 和 ``-e no_proxy`` 来配置网络代理，例如：

.. code-block:: bash

  # --network host 指定使用宿主机的网络
  # -e http_proxy 指定 http 代理
  # -e https_proxy 指定 https 代理
  # -e no_proxy 指定哪些地址不使用代理
  docker run -it \
             --rm \
             --network host \
             -e http_proxy=http://127.0.0.1:8118 \
             -e https_proxy=http://127.0.0.1:8118 \
             -e no_proxy=.tuna.tsinghua.edu.cn \
             vins:latest