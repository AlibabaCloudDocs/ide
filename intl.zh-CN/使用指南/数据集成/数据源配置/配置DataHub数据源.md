# 配置DataHub数据源 {#concept_jwx_3qv_42b .concept}

DataHub数据源作为数据中枢，为您提供从其它数据源写入数据至DataHub的功能，支持DataHub Writer插件。

DataHub提供完善的数据导入方案，能够快速解决海量数据的计算问题。

**说明：** 标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks控制台](https://workbench.data.aliyun.com/console)，单击相应工作空间后的**进入数据集成**。
2.  选择**同步资源管理** \> **数据源**，单击**新增数据源**。

    ![新增数据源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15677448397595_zh-CN.png)

3.  在新增数据源弹出框中，选择数据源类型为**DataHub**。
4.  填写DataHub数据源的各配置项。

    ![配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15677448397527_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源的简单描述，不超过80个字。|
    |**适用环境**|可以选择**开发**或**生产**环境。 **说明：** 仅标准模式工作空间会显示此配置。

 |
    |**DataHub Endpoint**|默认只读，从系统配置中自动读取。|
    |**DataHub Project**|对应的DataHub Project标识。|
    |**AccessKey ID/AceessKey Secret**|访问密钥（AccessKeyID和AccessKeySecret），相当于登录密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**完成**。

    提供测试连通性功能，可以判断输入的信息是否正确 。


## 后续步骤 {#section_qzn_4qv_42b .section}

现在，您已经学习了如何配置DataHub数据源，您可以继续学习下一个教程。在该教程中，您将学习如何配置DataHub Writer插件，详情请参见[配置DataHub Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置DataHub Writer.md#)。

