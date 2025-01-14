.. _cn_api_distributed_ProcessMesh:

ProcessMesh
-------------------------------


.. py:class:: paddle.distributed.ProcessMesh(mesh, parent=None)


类ProcessMesh描述逻辑进程的拓扑结构。mesh是表示逻辑进程组织结构的N-维数组。该数组的形状表示逻辑进程的拓扑结构，数组的元素值表示某个逻辑进程。例如，2-维数组[[2, 4, 5], [0, 1, 3]]描述6个逻辑进程，这些进程的拓扑结构为[2, 3]，即2-维数组的形状。对于上述拓扑结构，存在2个并行组；第一个并行组的并行度为2，第二个并行组的并行度为3。此外，第一个逻辑进程的id是2。


参数：
    - **mesh** (list) - 表示进程逻辑拓扑的N-维数组(嵌套链表)，该数组的形状表示逻辑进程的拓扑结构，元素值表示某个逻辑进程。
    - **parent** (ProcessMesh, 可选) - 父ProcessMesh，None表示没有父ProcessMesh。默认值为None。

返回值：
    None。

抛出异常：
    ValueError: 如果mesh不是list实例。

**代码示例**:

.. code-block:: python

    import numpy as np
    import paddle
    import paddle.distributed as dist
    
    paddle.enable_static()
    
    mesh = dist.ProcessMesh([[2, 4, 5], [0, 1, 3]])
    assert mesh.parent is None
    assert mesh.topology == [2, 3]
    assert mesh.process_group == [2, 4, 5, 0, 1, 3]
    mesh.set_placement([0, 1, 2, 3, 4, 5])

   

属性
::::::::::::
.. py:attribute:: topology
ProcessMesh表示的进程拓扑结构，即用于初始化该ProcessMesh结构的mesh参数的形状。

.. py:attribute:: process_group
ProcessMesh表示的所有进程，类型为list。

.. py:attribute:: parent
ProcessMesh的父ProcessMesh，类型为ProcessMesh。

方法
::::::::::::
.. py:function:: set_placement(order)
使用用户设置的order设置逻辑进程到物理进程的映射关系。

参数：
    - **order** (list): 物理进程id的顺序

返回：
   None。


**代码示例**:

.. code-block:: python

   import numpy as np
   import paddle
   import paddle.distributed as dist

   paddle.enable_static()

   mesh = dist.ProcessMesh([[2, 4, 5], [0, 1, 3]])
   mesh.set_placement([0, 1, 2, 3, 4, 5])
