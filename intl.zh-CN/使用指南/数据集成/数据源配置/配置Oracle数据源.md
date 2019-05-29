# 配置Oracle数据源 {#concept_jkd_5nb_p2b .concept}

Oracle数据源提供了读取和写入Oracle双向通道的能力，您可以通过向导模式和脚本模式配置同步任务。

**说明：** 标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  选择**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15590928747556_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**Oracle**。
4.  填写**新增Oracle数据源**对话框中的配置信息。

    Oracle数据源类型分为**连接串模式（数据集成网络可直接连通）**和**连接串模式（数据集成网络不可直接连通）**，您可根据自身情况进行选择。

    以新增**Oracle** \> **连接串模式（数据集成网络可直接连通）**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15590928747557_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|连接串模式（数据集成网络可直接连通）。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**适用环境**|根据自身需求勾选**开发**或**生产**。|
    |**JDBC URL**|JDBC连接信息，格式为`jdbc:oracle:thin:@host:port:SID`或`jdbc:oracle:thin:@//host:port/service_name`。|
    |**用户名/密码**|数据库对应的用户名和密码。|

    以新增**Oracle** \> **连接串模式（数据集成网络不可直接连通）**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15590928747558_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|连接串模式（数据集成网络不可直接连通），此种类型的数据源需要使用自定义调度资源才能进行同步，可单击**帮助手册**进行查看。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**适用环境**|根据自身需求勾选**开发**或**生产**。|
    |**资源组**|选择相应的资源组，您也可新增自定义资源组。|
    |**JDBC URL**|JDBC连接信息，格式为`jdbc:oracle:thin:@host:port:SID`或`jdbc:oracle:thin:@//host:port/service_name`。|
    |**用户名/密码**|数据库对应的用户名和密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

## 测试连通性说明 {#section_eq3_lnb_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL、用户名/密码是否正确。
-   专有网络无公网和其他本地网络无公网，数据集成不可直接连通（不支持测试连通性），需要通过JDBC连接形式配置数据源，并且通过自定义资源组进行任务同步。

## 后续步骤 {#section_bbg_jnb_p2b .section}

现在，您已经学习了如何配置Oracle数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置Oracle Writer插件。详情请参见[配置Oracle Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置Oracle Writer.md#)和[配置Oracle Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置Oracle Reader.md#)。

