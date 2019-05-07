# 循环（do-while）节点 {#concept_h34_qts_jgb .concept}

您可在do-while节点中定义相互依赖的任务，任务中包含一个名为end的循环判断节点。Dataworks会不断重复执行这一批任务，直到循环判断节点end把判断结果置为false，Dataworks才会退出整个循环。

**说明：** 循环节点最多可循环128次，一旦超过便会报错。

循环（do- while）节点支持ODPS SQL、SHELL和Python三种赋值语言。选择ODPS SQL赋值语言时，您可以通过case when语句进行判断，示例如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090241110_zh-CN.png)

## 循环节点的简单示例 {#section_kxm_glj_wgb .section}

本节将为您介绍使用循环节点循环5次，每次循环中把当前的循环次数打印出来的简单场景。

1.  进入数据开发页面，选择**新建** \> **控制** \> **do-while**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239350_zh-CN.png)

2.  填写**新建节点**对话框中的配置，单击**提交**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239469_zh-CN.png)

3.  双击新建的do-while节点，对循环体进行定义。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239471_zh-CN.png)

    双击进入do-while节点内部时，会自带start-sql-end三个节点。

    -   start节点是一个循环开始的标记节点，并无业务作用。
    -   SQL节点是Dataworks为引导用户给出的一个业务处理节点示例，此处需要把它删除，替换为自己的业务处理Shell节点“打印当前循环次数”，代码如下所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239477_zh-CN.png)

    -   end节点有标记循环结束和判断是否开启下一次循环两大功能，此处对do-while节点的结束条件进行定义。

        end节点本质上是一个赋值节点，仅输出true/false两种字符串，分别代表继续下一个循环和不再继续循环。代码如下：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239479_zh-CN.png)

        在“打印循环次数”和end节点中，都用到了$\{dag.loopTimes\}变量。它是系统的保留变量，代表当前的循环次数，从1开始，do-while的内部节点可以直接引用这个变量。

        代码中把dag.loopTimes和5进行比较，这样便限制了整体的循环次数，第一次循环dag.loopTimes为1、第二次为2，以此类推，第五次为5。至此表达式$\{dag.loopTimes\}<5结果为false，退出循环。

4.  执行do-while节点。

    您可根据自身需求进行调度配置后，把do-while节点提交至运维中心执行。

    -   外层do-while节点：do-while节点整体在运维中心中被当作一个整体节点来展示，如果您想要查看do-while节点的循环详情，可右键单击节点，选择**查看内部节点**，即可跳转至内部节点视图。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239504_zh-CN.png)

    -   内部循环体：该视图分三个部分。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090239505_zh-CN.png)

        -   视图左侧为do-while节点的重跑历史列表，只要do-while实例整体执行一次，历史列表便会产生一条相应的记录。
        -   视图中部为循环记录列表，会列出当前do-while节点共执行多少次循环，以及每次循环的状态。
        -   视图右侧为每次循环的具体信息，单击循环记录列表中的某次循环，即可展示出该循环每个实例的执行情况。
5.  查看运行结果。

    进入内部循环体后，单击循环记录列表中的第3次循环，即可看到执行日志中打印出本次的循环次数3。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090339507_zh-CN.png)

    您也可查看第3次循环和第5次循环中，end节点的执行日志。

    ![](images/39508_zh-CN.png "第3次循环end节点的执行日志")

    ![](images/39509_zh-CN.png "第5次循环end节点的执行日志")

    由上面两张end节点日志图可见，第3次循环的结果是3<5 =\> True，第5次循环的结果是5<5 =\> False。所以在第5次执行完毕后，退出循环。


由上述简单示例可总结出do-while节点的工作流程如下：

1.  从start节点开始执行。
2.  按照定义的任务依赖关系依次执行每个任务。
3.  在end节点中定义循环的结束条件。
4.  一组任务执行完毕之后，执行end的结束条件语句。
5.  如果end的判断语句在日志中打印true，则从1开始继续下一个循环。
6.  如果end的判断语句在日志中打印false，则退出整个循环，do-while节点整体结束。

## 循环节点的复杂示例 {#section_byd_wk2_xgb .section}

除了上文中提到的简单场景外，还会遇到用循环的方式依次处理一组数据的每一行的复杂场景。处理此场景前，需要满足以下条件：

-   需要部署一个上游节点，能够把查询出的数据输出给下游节点使用，您可使用赋值节点实现此条件。
-   循环节点需要能够拿到上游赋值节点的输出，您可通过配置上下文依赖来实现这此条件。
-   循环节点的内部节点需要能够引用到每一行的数据，这里我们已有的节点上下文做了增强，并且额外下发了系统变量$\{dag.offset\}，可帮忙您快速引用循环节点的上下文。

下文将为您介绍如何实现用循环节点把表tb\_dataset的每一行数据分别在每次循环中打印出来，数据为c、0、1。

1.  进入数据开发页面，选择**新建** \> **控制** \> **do-while**。
2.  填写**新建节点**对话框中的配置，单击**提交**。
3.  双击新建的do-while节点，对循环体进行定义。
    1.  本示例需要给do-while节点添加一个上游节点“数据集初始化”，它会产生测试数据集。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090339529_zh-CN.png)

    2.  创建节点后，对do-while节点进行调度配置，为其单独配置一个节点上下文，key为input，值为上游节点数据集初始化的outputs。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090339531_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090339533_zh-CN.png)

    3.  输入业务处理节点“打印每一行数据”的代码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090439534_zh-CN.png)

        -   `dag.offset`：Dataworks系统的保留变量，代表每一次循环次数相对于第一次的偏移量。即第一次循环中offset为0、第二次为1、第三次为2…第n次为n-1。
        -   `dag.input`：这个变量是用户配置的do-while上下文依赖，上文中提到do-while节点配置了一个叫input的上下文依赖，值为上游节点“数据集初始化”的outputs。

            do-while内部节点可直接用$\{dag.$\{ctxKey\}\}的方式来引用上下文依赖的值，因为上文中配置的上下文依赖key为input，因此可以使用$\{dag.input\}来引用这个值。

        -   `${dag.input[${dag.offset}]}`：节点数据集初始化的输出是一个表格，Dataworks可以用偏移量的方式来获取表格数据的某一行。由于每次循环中dag.offset的值从0开始递增，则最后打印出来的数据为$\{dag.input\[0\]\}、$\{dag.input\[1\]\}，以此类推达到遍历数据集的效果。
    4.  定义end节点的结束条件。如下图所示，把dag.loopTimes与dag.input.length变量进行比较，小于则输出True继续循环，不小于则输出False退出循环。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090439552_zh-CN.png)

        **说明：** dag.input.length变量标识的是上下文参数input数组的行数，是系统自动根据节点配置的上下文下发的变量。

4.  执行节点并查看执行结果。
    -   数据集初始化节点最终会产生0和1两条数据。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/90301/155722090439556_zh-CN.png)

    -   打印每一行数据节点执行结果如下所示。

        ![](images/39560_zh-CN.png "打印第1行数据")

        ![](images/39562_zh-CN.png "打印第2行数据")

    -   end节点的结果如下所示。

        ![](images/39565_zh-CN.png "第1次end执行结果")

        ![](images/39566_zh-CN.png "第2次end执行结果")

        由上面两张end节点日志图可见，第1次循环的次数小于行数，返回True继续执行，第2次循环的次数等于行数，返回False停止循环。


## 总结 {#section_uyq_13f_xgb .section}

-   do-while与while/foreach/do…while三种循环类型对比如下：
    -   do-while能够实现先循环再判断的循环体，即do…while语句，能够通过系统的变量dag.offset结合节点上下文间接实现foreach语句。
    -   do-while不能实现先判断再循环的方式，即while语句。
-   do-while执行流程
    1.  从start开始按任务依赖关系依次执行循环体中的任务。
    2.  执行用户在end节点中定义的代码。
        -   如果end节点输出True，则继续下一个循环。
        -   如果end节点输出False，则终止循环。
-   如何使用上下文依赖：do-while的内部节点可以通过$\{dag.上下文变量名\}的方式引用到do-while节点定义的节点上下文。
-   系统参数：Dataworks会为do-while内部节点自动下发两个系统变量。
    -   dag.loopTimes从1开始标识这一次循环的次数。
    -   dag.offset从0开始标识这一次循环相对于第一次循环的次数偏移量。

