# SQL代码编码原则与规范 {#concept_q3g_zg2_q2b .concept}

本文向您介绍SQL编码的基本原则和详细的编码规范。

## 编码原则 {#section_a44_2h2_q2b .section}

SQL代码的编码原则如下：

-   代码功能完善，健壮。
-   代码行清晰、整齐，具有一定的可观赏性。
-   代码编写要充分考虑执行速度最优的原则。
-   代码行整体层次分明、结构化强。
-   代码中应有必要的注释以增强代码的可读性。
-   规范要求非强制性约束代码开发人员的代码编写行为，在实际应用中在不违反常规要求的前提下允许存在可理解的偏差本规范在对日常的代码开发工作起到指导作用的同时也将得到不断的完善和补充。
-   SQL代码中应用到的所有关键字、保留字都使用小写，如select、from、where、and、or、union、insert、delete、group、having、count等。
-   SQL代码中应用到的除关键字、保留字之外的代码，也都使用小写，如字段名、表别名等。
-   四个空格为一个缩进量，所有的缩进皆为一个缩进量的整数倍，按代码层次对齐。
-   禁止使用select \* 操作，所有操作必须明确指定列名。
-   对应的括号要求在同一列的位置上。

## SQL编码规范 {#section_yf2_xh2_q2b .section}

SQL代码的编码规范如下：

-   代码头部

    代码头部添加主题、功能描述、作者和日期等信息，并预留修改日志及标题栏，以便后续修改同学添加修改记录。注意每一行不超过80个字符。模板如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700337938_zh-CN.png)

    ```
    -- MaxCompute(ODPS) SQL
    --**************************************************************************
    -- ** 所属主题: 交易
    -- ** 功能描述: 交易退款分析
    -- ** 创建者 : 有码
    -- ** 创建日期: 20170616 
    -- ** 修改日志:
    -- ** 修改日期 修改人 修改内容
    -- yyyymmdd name comment 
    -- 20170831 无码 增加对biz_type=1234交易的判断 
    --**************************************************************************
    ```

-   字段排列要求
    -   select语句选择的字段按每行一个字段方式编排。
    -   select单字后面一个缩进量后直接跟首个选择的字段，即字段离首起二个缩进量。
    -   其它字段前导二个缩进量，再在逗号后放置字段名。
    -   两个字段之间的逗号分割符紧跟在第二个字段的前面。
    -   as语句应与相应的字段在同一行。多个字段的as建议尽量对齐在同一列上。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700338881_zh-CN.jpg)

-   INSERT子句排列要求

    insert子句写在同一行，请勿换行。

-   SELECT子句排列要求

    select语句中所用到的from、where、group by、having、order by、join、union等子句，需要遵循如下要求：

    -   换行编写。
    -   与相应的select语句左对齐编排。
    -   子句后续的代码离子句首字母二个缩进量起编写。
    -   where子句下的逻辑判断符and、or等与where左对齐编排。
    -   超过两个缩进量长度的子句加一空格后编写后续代码，如order by、group by等。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700338882_zh-CN.jpg)

-   运算符前后间隔要求算术运算符、逻辑运算符前后要保留一个空格，除非超过每行80个字符长度的限制，否则都写在同一行。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700338883_zh-CN.jpg)

-   CASE语句的编写

    select语句中对字段值进行判断取值的操作将用到的case语句，正确的编排case语句的写法对加强代码行的可阅读性也是很关键的一部分。

    对case语句编排作如下约定：

    -   when子语在case语句的同一行并缩进一个缩进量后开始编写。
    -   每个when子语一行编写，当然如果语句较长可换行编排。
    -   case语句必须包含else子语，else子句与when子句对齐。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700338884_zh-CN.jpg)

-   查询嵌套编写规范

    子查询嵌套在数据仓库系统ETL开发中是经常要用到，因此代码的分层编排就非常重要。示例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700338885_zh-CN.jpg)

-   表别名定义约定
    -   所有的表都加上别名。因为一旦在SELECT语句中给操作表定义了别名，在整个语句中对此表的引用都必须惯以别名替代。考虑到编写代码的方便性，约定别名尽量简单、简洁，同时避免使用关键字。
    -   表别名采用简单字符命名，建议按a、b、c、d……的顺序进行命名。
    -   多层次的嵌套子查询别名之前要体现层次关系，SQL语句别名的命名，分层命名，从第一层次至第四层次，分别用P 、S、 U 、D表示，取意为Part, Segment, Unit, Detail。也可用a、b、c、d来表示第一层次到第四层次；对于同一层次的多个子句，在字母后加1、2、3、4……区分。有需要的情况下对表别名加注释。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700348886_zh-CN.jpg)

-   SQL注释

    -   每条SQL语句均应添加注释说明。
    -   每条SQL语句的注释单独成行、放在语句前面。
    -   字段注释紧跟在字段后面。
    -   应对不易理解的分支条件表达式加注释。
    -   对重要的计算应说明其功能。
    -   过长的函数实现，应将其语句按实现的功能分段加以概括性说明。
    -   常量及变量注释时，应注释被保存值的含义（必须），合法取值的范围（可选）。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15414700347939_zh-CN.png)


