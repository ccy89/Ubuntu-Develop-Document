CUDA 开发环境搭建
=====================
`CUDA（Compute Unified Device Architecture，统一计算架构） <https://developer.nvidia.com/cuda-toolkit>`_，
是由 ``NVIDIA`` 所推出的一种集成技术，是该公司对于 ``GPGPU`` 的正式名称。
通过这个技术，用户可利用 ``NVIDIA`` 的 ``GeForce 8`` 以后的 ``GPU`` 和较新的 ``Quadro GPU`` 进行计算。


CUDA Toolkit安装
-------------------
**1. 安装** ``CUDA``： 直接参照官网教程即可，官网 ``CUDA`` 下载地址: https://developer.nvidia.com/cuda-downloads

官网提供了3种安装方式，建议网络条件差的选择第1种或第2种方式，网络条件好的选择第3种方式，:red:`注意在安装` ``CUDA`` :red:`的时候同时会安装显卡驱动。`

.. image:: /_static/images/cuda-1.png
  :align: center

**2. 添加环境变量**： 需要向系统的两个环境变量  ``PATH`` 和 ``LD_LIBRARY_PATH`` 提供 ``CUDA`` 的相关信息。

在 ``~/.bashrc`` (``Bash``) 或者 ``~/.zshrc`` (``Zsh``) 的末尾添加两句话：

.. code-block:: bash

  export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
  export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

打开一个新的终端，测试环境变量：

.. code-block:: bash

  echo $PATH              # 查看 PATH 环境变量，应该包含 /usr/local/cuda/bin
  echo $LD_LIBRARY_PATH   # 查看 LD_LIBRARY_PATH 环境变量，应该包含 /usr/local/cuda/lib64
  nvcc --version          # 检查 是否能找到 nvcc 命令

**3. 重启系统**：使显卡驱动和 ``CUDA`` 生效。重启后，在终端输入命令 ``nvidia-smi`` 查看相关信息：

.. image:: /_static/images/cuda-2.png
  :align: center
