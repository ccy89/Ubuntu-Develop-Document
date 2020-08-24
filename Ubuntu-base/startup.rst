Ubuntu启动配置
----------------
双系统索引
^^^^^^^^^^^^
``Ubuntu`` 采用 ``grub`` 管理系统的引导项，默认情况下 ``grub`` 是可以检索到 ``Windows`` 引导的。如果在 ``Bios`` 里把 ``Ubuntu`` 放在第一启动顺序，那么在开机的时候会出现 ``grub`` 启动界面：

.. image:: /_static/images/startup-1.png

* **Ubuntu**： 从系统中最新的内核启动 ``Ubuntu``
* **Advance options for Ubuntu**：可以选择从特定的内核启动 ``Ubuntu`` 
* **Windows Boot Manager**： 启动 ``Windows``
* **System setup**： 进入 ``BIOS``

如果 ``grub`` 没有检索到 ``Windows`` 引导，则执行 ``sudo update-grub`` 命令更新 ``grub`` 的索引：

.. image:: /_static/images/startup-2.png


Ubuntu 修改默认启动内核 
^^^^^^^^^^^^^^^^^^^^^^^^
在使用 ``Ubuntu`` 时可能存在多个内核版本，一般情况下，``grub`` 默认启动的是系统里最新的内核，如果想要把老的内核版本设为默认启动项，需要修该 grub 的配置。

例如在电脑启动时，``grub`` 菜单中有如下启动项:

.. image:: /_static/images/startup-1.png

其中 **Ubuntu** 的索引为0，**Advanced options for Ubuntu** 为索引为1，依次类推。

在 **Advanced options for Ubuntu** 的子目录如下可以看到所有可以启动的内核以及对应的 ``recovery mode``:

.. image:: /_static/images/startup-3.png

同样，第一个选项是索引0，第二个是1，第三个是2，依此类推。

可以直接通过 ``grub`` 启动界面选择其他内核启动，也可以更改配置文件 ``/etc/default/grub`` 中的 ``GRUB_DEFAULT`` 来修改默认启动内核：

``GRUB_DEFAULT=2`` 则将默认启动 ``Windows`` 系统。

``GRUB_DEFAULT=1>2`` 则将默认启动 **Advanced options for Ubuntu** 目录下 索引为2的内核。