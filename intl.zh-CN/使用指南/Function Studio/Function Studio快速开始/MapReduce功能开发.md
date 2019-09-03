# MapReduce功能开发 {#concept_td3_c1j_5gb .concept}

工程创建完成后会自动生成框架代码，支持新建MapReduce任务。本文将以WordCount示例代码为例，为您介绍如何从0开始测试和发布。

## 新建工程 {#section_qs2_wfk_5gb .section}

1.  进入Function Studio页面，单击**工作空间**页面的**新建代码工程**。
2.  填写**新建项目**对话框中的**工程名**和**工程描述**，选择相应的模板（本文选择**UDFJava Project**）。

    ![新建工程](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133012_zh-CN.png)

3.  配置完成后，单击**提交**。

## 开发项目 {#section_tc2_2kk_5gb .section}

在mapred包下，已存在WordCount的MapReduce示例代码。示例代码的功能是对输入表中的单词进行次数统计，将统计结果写入到输出表中，输入输出分别是两个表，详情请参见[MapReduce](../../../../intl.zh-CN/开发/MapReduce/概要/MapReduce概述.md#)。

![开发项目](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133013_zh-CN.png)

## 调试项目 {#section_asw_clk_5gb .section}

MapReduce项目目前不能在Function Studio中进行Debug，需要将代码发布至DataWork开发环境，然后跳转至DataWorks中进行逻辑验证。

**说明：** Function Studio目前仅支持编码和编译打包两个功能。

## 发布项目 {#section_d4g_qlk_5gb .section}

1.  Function Studio编译打包代码发布至Data Studio开发环境。
    1.  鼠标悬停至**提交**图标，**提交资源至Data Studio开发环境**。

        ![提交资源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968132996_zh-CN.png)

    2.  在**提交资源至Data Studio开发环境**对话框中，选择**目标业务空间**、**目标业务流程**和**资源**，默认**已经存在强制更新**。

        ![填写配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968132997_zh-CN.png)

    3.  单击**确认**，即可进行发布。发布成功后，即可获取资源在DataStudio中的定位链接。

        ![确认](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968132998_zh-CN.png)

2.  在DataWorks中创建MapReduce节点进行测试。
    1.  打开DataWorks同名的工作空间，创建ODPS MR节点，详情请参见[ODPS MR节点](intl.zh-CN/使用指南/数据开发/节点类型/ODPS MR节点.md#)。

        ![创建节点](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133017_zh-CN.png)

    2.  计算节点需要写入以下固定脚本，目前脚本的一些变量还需被手工替换成相关Jar包的信息。

        ![脚本文件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133019_zh-CN.png)

        **说明：** 请使用在Function Studio中发布的Jar包的信息来替换脚本中的信息，生成最后的代码。

        -   jar -resources：FunctionStudio发布的Jar包名称。
        -   -classpath：Jar包在DataWorks中的路径。
        -   包含main函数的入口类全类名main函数参数空格分隔。
    3.  选择相应业务流程下的**资源**，查看在Function Studio中发布的Jar包信息，来替换脚本中的相关信息。

        -   Jar包的名称为WordCountDemo\_1.0.0.jar，对应脚本中的-resource。
        -   右键单击Jar包，选择**查看历史版本**，即可查看路径的名称`http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493`，对应脚本中的-classpath。

            ![查看历史版本](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133030_zh-CN.png)

            ![版本信息](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968133040_zh-CN.png)

        最后的脚本：

        ``` {#codeblock_0v0_50a_av6}
        # 手工将刚才从发布jar包中的信息填入脚本，脚本完成。
        jar -resources WordCountDemo_1.0.0.jar
        -classpath http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493
        com.alibaba.dataworks.mapred.WordCount wordcount_demo_input wordcount_demo_output
        ```

    4.  创建测试表并添加测试数据。

        准备好数据后，在开发环境运行脚本。

        ![测试数据](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968233045_zh-CN.png)

        至此，WordCount在开发环境的测试全部完成。由于WordCount的计算节点、Jar包、输入输出表都在开发环境，所以需要分别发布至正式环境。

3.  将资源包，数据表，节点分别发布到DataWorks正式环境。
    1.  单击**提交**图标，提交计算节点代码。
    2.  单击右侧的**调度配置**，进行发布配置。
    3.  单击右上角的**任务发布**，进入发布页面，勾选提交的Jar包和节点进行发布。

        ![任务发布](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968233048_zh-CN.png)

    4.  提交数据表至生产环境。
    5.  在**数据开发**页面，单击右上角的**运维中心**，对MapReduce任务进行线上测试。

        ![运维中心](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/156750968233051_zh-CN.png)


Function Studio可以编码和编译发布代码到DataWorks节点，DataWorks需要手工操作生成计算节点，分别在开发环境和生产环境运行。

