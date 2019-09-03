# PyODPS节点 {#concept_ijm_dzl_q2b .concept}

DataWorks支持PyODPS任务类型，集成了MaxCompute的Python SDK，您可以直接编辑DataWorks的PyODPS节点中的Python代码，来操作MaxCompute。

## 新建PyODPS节点 {#section_e5k_pzl_q2b .section}

1.  单击左侧导航栏中的**手动业务流程**，进入**手动业务流程**面板。
2.  新建业务流程。
    1.  单击左侧导航栏中的**手动业务流程**，选择**新建业务流程**。

        ![新建业务流程](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15675079457961_zh-CN.png)

    2.  填写**业务名称**和**描述**，单击**新建**，即可完成业务流程的新建。
3.  新建PyODPS节点。
    1.  打开新建的手动业务流程，右键单击**数据开发**，选择**新建数据开发节点** \> **PyODPS**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16322/15675079458036_zh-CN.png)

    2.  填写新建节点对话框中的配置，单击**提交**。
4.  编辑PyODPS节点。
    1.  ODPS入口

        DataWorks 的PyODPS 节点中，将会包含一个全局的变量'odps'或'o'，即ODPS入口，您不需要手动定义ODPS入口。

        ``` {#codeblock_6y5_uix_cg2}
        print(odps.exist_table('PyODPS_iris'))
        ```

    2.  执行SQL

        PyODPS支持ODPS SQL的查询，并可以读取执行的结果。execute\_sql或run\_sql方法的返回值是运行实例。

        **说明：** 并非所有在ODPS Console中可以执行的命令都是ODPS可以接受的SQL语句。在调用非DDL/DML语句时，请使用其它方法，例如GRANT/REVOKE等语句，请使用run\_security\_query方法，PAI命令请使用run\_xflow或execute\_xflow方法。

        ``` {#codeblock_dza_ao9_sjy}
        o.execute_sql('select * from dual')  #同步的方式执行，会阻塞直到SQL执行完成。
        instance = o.run_sql('select * from dual')  # 异步的方式执行。
        print(instance.get_logview_address())  # 获取logview地址。
        instance.wait_for_success()  # 阻塞直到完成。
        ```

    3.  设置运行参数

        您可以通过设置hints参数来设置运行时的参数，参数类型是dict。

        ``` {#codeblock_g3o_9gf_zq1}
        o.execute_sql('select * from PyODPS_iris', hints={'odps.sql.mapper.split.size': 16})
        ```

        对全局配置设置sql.settings后，每次运行时都需要添加相关的运行时参数。

        ``` {#codeblock_8yn_g54_dre}
        from odps import options
        options.sql.settings = {'odps.sql.mapper.split.size': 16}
        o.execute_sql('select * from PyODPS_iris')  # 根据全局配置添加hints。
        ```

    4.  读取SQL执行结果

        运行SQL的instance能够直接执行open\_reader的操作，一种情况是SQL返回了结构化的数据。

        ``` {#codeblock_e1j_r8u_8wv}
        with o.execute_sql('select * from dual').open_reader() as reader:
        for record in reader:  # 处理每一个record。
        ```

        另一种情况是SQL可能执行的desc等，通过reader.raw属性取到原始的SQL执行结果。

        ``` {#codeblock_0yl_d31_em4}
        with o.execute_sql('desc dual').open_reader() as reader:
        print(reader.raw)
        ```

        **说明：** 在数据开发使用了自定义调度参数，页面上直接触发运行PyODPS节点时，需要写死时间，PyODPS节点无法像SQL一样直接替换。

5.  节点调度配置。

    单击节点编辑区右侧的**调度配置**，即可进入节点调度配置页面，详情请参见[调度配置](intl.zh-CN/使用指南/数据开发/调度配置/基础属性.md#)模块。

6.  提交节点任务。

    完成调度配置后，单击左上角的**保存**，提交（提交并解锁）到开发环境。

7.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理/任务发布.md#)。

8.  在生产环境测试。

    具体操作请参见[手动任务](intl.zh-CN/使用指南/运维中心/手动任务运维/手动任务.md#)。


