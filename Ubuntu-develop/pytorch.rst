PyTorch开发环境搭建
=======================
`PyTorch <https://pytorch.org/>`_ 是一个开源的 ``Python`` 机器学习库，底层由 ``C++`` 实现。
``PyTorch`` 主要应用于人工智能领域，如深度学习，它最初由 ``Facebook`` 的人工智能研究团队开发。

``PyTorch`` 主要有两大特征：

* 类似于 ``NumPy`` 的张量计算，可使用 ``GPU`` 加速
* 带自动微分系统的深度神经网络

安装PyTorch
---------------
.. code-block:: bash

  conda create -n pytorch python=3.7  # 建议 创建一个新的 python 环境
  # 指定 安装的pytorch的版本、cuda的版本 和 下载源
  # cuda版本 和 pytorch版本 根据自己需求修改
  conda install -n pytorch pytorch=1.5 torchvision cudatoolkit=10.2 --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch

.. image:: /_static/images/pytorch-1.png

.. hint::
  在安装 ``PyTorch`` 的 ``CUDA`` 版本时，其实会安装 ``CUDA`` 到 ``conda`` 的虚拟环境之中。
  这意味着如果只使用 ``Python`` 的 ``PyTorch`` 做深度学习，可以只安装显卡驱动，不用在系统中安装 ``CUDA``。


测试PyTorch
----------------
.. code-block:: bash

  conda activate pytorch
  python3     # 进入 python 交互终端

执行 ``Python`` 代码：

.. code-block:: python

  import torch
  torch.cuda.is_available()

如果上述 ``Python`` 代码返回 ``True``，代表 ``PyTorch`` + ``CUDA`` 配置成功。
