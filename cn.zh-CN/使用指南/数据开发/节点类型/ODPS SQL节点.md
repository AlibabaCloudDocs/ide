# ODPS SQL节点 {#concept_cc4_1x3_p2b .concept}

ODPS SQL采用类似SQL的语法，适用于海量数据（TB级）但实时性要求不高的分布式处理场景。

因为每个作业从前期准备到提交等阶段都需要花费较长时间，因此如果要求处理几千至数万笔事务的业务，可以使用ODPS SQL顺利完成。它是主要面向吞吐量的OLAP应用。

## 新建ODPS SQL节点 {#section_zfn_smw_vej .section}

1.  进入DataStudio（数据开发）页面，选择**新建** \> **数据开发** \> **ODPS SQL**。

    ![ODPS SQL](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345498851559_zh-CN.png)

    **说明：** 您也可以找到相应的业务流程，右键单击**数据开发**，选择**新建数据开发节点** \> **ODPS SQL**。

    ![ODPS SQL](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345498951560_zh-CN.png)

2.  填写新建节点对话框中的配置，单击**提交**。

    ![提交](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345498951563_zh-CN.png)

3.  编辑节点代码。

    编写符合语法的ODPS SQL代码，SQL语法请参见[SQL概述](../../../../intl.zh-CN/开发/SQL及函数/SQL概述.md#)。

    ![编写代码](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/15634549897680_zh-CN.png)

    **说明：** 由于国际标准化组织发布的中国时区信息调整，通过DataWorks执行相关SQL时，日期显示某些时间段会存在时间差异：1900-1928年的日期时间差异5分52秒，1900年之前的日期时间差异9秒。

    目前DataWorks不允许节点代码中只包含[set语句](../../../../intl.zh-CN/开发/常用命令/SET操作.md#)。如果您需要运行set语句，可以和其他SQL语句一起执行，如下所示：

    ``` {#codeblock_fc2_yrd_kep .lanuage-shell}
    setproject odps.sql.allow.fullscan=true;
    select 1;
    ```

    示例：创建一张表并向表中插入数据，查询结果。

    1.  创建一张表test1。

        ``` {#codeblock_utt_hmi_rph .lanuage-shell}
        CREATE TABLE IF NOT EXISTS test1 
        ( id BIGINT COMMENT '' ,
          name STRING COMMENT '' ,
          age BIGINT COMMENT '' ,
          sex STRING COMMENT '');
        ```

    2.  插入准备好的数据。

        ``` {#codeblock_7f4_e4q_e6j .lanuage-shell}
        INSERT INTO test1 VALUES (1,'张三',43,'男') ;
        INSERT INTO test1 VALUES (1,'李四',32,'男') ;
        INSERT INTO test1 VALUES (1,'陈霞',27,'女') ;
        INSERT INTO test1 VALUES (1,'王五',24,'男') ;
        INSERT INTO test1 VALUES (1,'马静',35,'女') ;
        INSERT INTO test1 VALUES (1,'赵倩',22,'女') ;
        INSERT INTO test1 VALUES (1,'周庄',55,'男') ;
        ```

    3.  查询表数据。

        ``` {#codeblock_qhu_zck_t46 .lanuage-shell}
        select * from test1;
        ```

    4.  写好SQL，单击顶部的**运行**或单击**F8**，此时系统会将我们的SQL按照从上往下的顺序执行，并打印日志。

        ![运行日志](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345498911262_zh-CN.jpg)

        **说明：** 当使用insert into语句时，日志中会提示：**!!!警告!!!** 。

        在SQL中使用insert into语句有可能造成不可预料的数据重复。尽管对于insert into语句已经取消SQL级别的重试，但仍然存在进行任务级别重试的可能性，请尽量避免对insert into语句的使用。如果继续使用insert into语句，表明您已经明确insert into语句存在的风险，且愿意承担由于使用insert into语句造成的潜在的数据重复后果。

        此提示可以根据实际使用需求选择。无论您如何选择，执行SQL的数据已经成功插入到表中，不需要重复执行来确定数据的插入情况，避免出现数据重复。

        创建好后，单击左上角的**保存**，即可保存当前SQL代码。

4.  查询结果展示。

    DataWorks的查询结果接入了电子表格功能，方便您对数据结果进行操作。

    查询出的结果，会直接以电子表格的风格展示出来，您可以在DataWorks中执行操作，或者在电子表格中打开，也可自由复制内容粘贴在本地Excel中。

    ![查询结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16335/15634549908260_zh-CN.png)

    -   隐藏列：选择隐藏其中的一列或多列，可以隐藏该列。
    -   复制该行：左侧选中需要复制的一行或多行后，单击复制该行。
    -   复制该列：顶部选中需要复制的一列或多列后，单击复制该列。
    -   复制：可以自由复制选中的内容。
    -   搜索：在查询结果的右上角会出现搜索框，方便对表中数据进行搜素。
5.  成本估计

    您可以点击**成本估计**按钮估算本次SQL任务可能产生的费用。

    ![成本估计](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345499032188_zh-CN.png)

    点击后会显示估算出的任务执行费用，如果预估费用一栏报错，您可以将鼠标悬停在**X**按钮上查看报错原因。

    ![查看报错原因](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/156345499032189_zh-CN.png)

6.  节点调度配置。

    单击节点任务编辑在区域右侧的**调度配置**，即可进入节点调度配置页面，详情请参见[调度配置](intl.zh-CN/使用指南/数据开发/调度配置/基本属性.md#)模块。

7.  提交节点任务。

    完成调度配置后，单击左上角的**保存**，提交（提交并解锁）到开发环境。

8.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理/任务发布.md#)。

9.  在生产环境测试。

    具体操作请参见[周期任务](intl.zh-CN/使用指南/运维中心/任务列表/周期任务.md#)。


