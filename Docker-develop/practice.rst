.. _docker-example:

实例-基于容器环境进行开发
-------------------------
现在用一个实例将之前学到的 ``Docker`` 知识串联起来。该实例为 ``容器`` 中运行并开发 ``VINS Fusion``：

**1. 准备：** 完成 :ref:`install-docker`、:ref:`nvidia-docker`、:ref:`image-accelerate`、:ref:`vscode-docker`、:ref:`vscode-docker-remote`

**2. 下载ROS镜像**：

.. code-block:: bash

  docker pull osrf/ros:melodic-desktop-full   # 下载 ros melodic-desktop-full 镜像

**3. 构建VINS镜像**：
新建文件夹 ``vins-docker``，用 ``VsCode`` 打开。在当前工程目录下新建一个文件 ``Dockerfile``，在 ``Dockerfile`` 中写入如下内容：

.. code-block:: Dockerfile

  # 基础镜像为 ROS 镜像
  FROM osrf/ros:melodic-desktop-full

  # 修改 Ubuntu源 和 ROS源 为清华源
  RUN sed -i "s|http://.*ubuntu.com|http://mirrors.tuna.tsinghua.edu.cn|g" /etc/apt/sources.list && \
      sed -i "s|packages.ros.org|mirrors.tuna.tsinghua.edu.cn|g" /etc/apt/sources.list.d/ros1-latest.list

  # 安装 ceres
  RUN apt-get update && apt-get install -y libgoogle-glog-dev \
                                          libatlas-base-dev \
                                          libsuitesparse-dev \
                                          python-catkin-tools \ 
                                          wget \
      && rm -rf /var/lib/apt/lists/* \
      && wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz && tar -xzvf ceres-solver-1.14.0.tar.gz \
      && cd ceres-solver-1.14.0 \
      && mkdir build \
      && cd build \
      && cmake .. -DCXX11=ON \
      && make -j6 install \
      && rm -rf /ceres-solver*

注意 ``Dockerfile`` 中有需要用到网络的指令，构建镜像时需要联网。之后在工程目录下执行构建镜像的命令：

.. code-block:: bash

  # --network host  构建镜像时，使用宿主机网络
  # -t vins:latest  指定镜像名为 vins:latest
  docker build --network host -t vins:latest .

**4. 启动容器**：启动容器之前先下载 ``VINS-Fusion`` 代码和运行时会用到的一个数据集 ``rosbag`` 文件：

.. code-block:: bash

  git clone https://github.com/HKUST-Aerial-Robotics/VINS-Fusion.git
  wget http://robotics.ethz.ch/\~asl-datasets/ijrr_euroc_mav_dataset/machine_hall/MH_05_difficult/MH_05_difficult.bag

由于启动容器的命令比较长，因此在工程目录下新建一个 ``bash`` 脚本文件 ``run.sh``，在脚本文件内填入运行命令：

.. code-block:: bash
  :emphasize-lines: 14,15

  #!/bin/bash
  
  xhost +local:docker

  docker run -it \
            --rm \
            --name vins \
            --privileged \
            --gpus all \
            -e DISPLAY=$DISPLAY \
            -e QT_X11_NO_MITSHM=1 \
            -e NVIDIA_DRIVER_CAPABILITIES=compute,video,utility,graphics \
            -v /tmp/.X11-unix:/tmp/.X11-unix \
            -v VINS-Fusion文件夹代码路径:/root/catkin_ws/src/VINS-Fusion \
            -v 数据集rosbag文件路径:/root/test.bag \
            vins:latest

  xhost -local:docker

:red:`注意`：高亮的两行需要自己填写自己的 ``VINS Fusion`` 和 ``数据集rosbag文件`` 的 **绝对路径**，用于将本地的代码和数据集挂载到容器中使用。

解释一下上面的很多命令和参数的含义：

* ``-i`` 和容器进行交互， ``-t`` 分配一个伪终端
* ``--rm`` 停止容器的同时删除容器 
* ``--name vins`` 将容器命名为 ``vins``
* ``--privileged`` 允许容器访问主机当中的设备，例如 ``/dev`` 目录下的相机
* ``-e DISPLAY=$DISPLAY``、``-e QT_X11_NO_MITSHM=1`` 和 ``-v /tmp/.X11-unix:/tmp/.X11-unix`` 容器可以显示程序的图像化界面，参考 `ROS Using GUI's with Docker <http://wiki.ros.org/docker/Tutorials/GUI>`_
* ``--gpus all`` 在容器中可以访问所有的 ``GPU``
* ``-e NVIDIA_DRIVER_CAPABILITIES=compute,video,utility,graphics`` 使用 ``NVIDIA`` 的 ``GPU`` 功能

之后运行 ``bash run.sh`` 命令启动并进入容器。


**5. 编译并运行VINS**

.. code-block:: bash
  
  # 下面命令在进入容器的终端中运行
  cd /root/catkin_ws/                             # 进入 catkin_ws
  catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=ON  # 编译 VINS，-DCMAKE_EXPORT_COMPILE_COMMANDS=ON 用于生成 compile_commands.json 文件
  source /root/catkin_ws/devel/setup.bash         # 在当前终端 激活 VINS 的 ROS环境
  roslaunch vins vins_rviz.launch                 # 启动 VINS Rviz

  # Ctrl + Shift + T 打开一个新终端
  docker exec -it vins bash   # 进入已经启动的vins容器
  source /root/catkin_ws/devel/setup.bash         # 在当前终端 激活 VINS 的 ROS环境
  rosrun vins vins_node ~/catkin_ws/src/VINS-Fusion/config/euroc/euroc_stereo_imu_config.yaml   # 启动 VINS

  # Ctrl + Shift + T 打开一个新终端
  docker exec -it vins bash   # 进入已经启动的vins容器
  source /root/catkin_ws/devel/setup.bash         # 在当前终端 激活 VINS 的 ROS环境
  rosrun loop_fusion loop_fusion_node ~/catkin_ws/src/VINS-Fusion/config/euroc/euroc_stereo_imu_config.yaml   # 启动 回环

  # Ctrl + Shift + T 打开一个新终端
  docker exec -it vins bash   # 进入已经启动的vins容器
  source /root/catkin_ws/devel/setup.bash         # 在当前终端 激活 VINS 的 ROS环境
  rosbag play /root/test.bag

.. image:: /_static/images/docker-21.png


**6. 利用容器环境开发VINS**

**6.1 VsCode 连接已经运行的容器 ``vins``**

.. image:: /_static/images/docker-20.png

**6.2 在容器中启用插件**：在左侧工具栏中选择 ``扩展(Extensions)`` 后，选择在容器中启用 ``C/C++`` 和 ``CMake``：

.. image:: /_static/images/docker-22.png

启用插件后，点击 ``需要重新加载`` 来重新连接容器，使插件生效。

**6.3 打开VINS代码文件并配置智能提示**：

打开容器内 ``/root/catkin_ws/src/VINS-Fusion`` 目录：

.. image:: /_static/images/docker-23.png

之后启动 ``命令栏（Ctrl + Shift + p）``，选择命令 ``C/C++: Edit COnfigurations (JSON)``，在隐藏目录 ``.vscode`` 下生成 ``C++`` 配置文件 ``c_cpp_properties.json``。
在其中添加之前编译生成的 ``compile_commands.json`` 文件路径，获得智能提示功能：

.. code-block:: json
  :emphasize-lines: 13

  {
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu11",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "clang-x64",
            "compileCommands": "/root/catkin_ws/build/compile_commands.json"
        }
    ],
    "version": 4
  }

**7. 可以开始进行开发了**