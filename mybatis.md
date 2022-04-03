Dao（DataObject)类：

由于 Java 中的基本类型会有默认值，例如当某个类中存在 private int age ；宇段时 创建这个类时， age 会有默认值 当使用 age 属性时，它总会有值 因此在某些情况下 便无法实现使 age 为口ull 并且在动态 SQL 的部分，如果使用 age != null 进行判断 结果总会为 true ，因而会导致很多隐藏的问题 所以，在实体类中不要使用基本类型 基本类型包括 byte int short lo float double char boolean。





两级缓存:

- 一级缓存：sqlsession级别的缓存，一直开启的，与数据库同一次会话期间查询到的数据会放到本地缓存中，以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库
   - 一级缓存失效的四种情况
   - sqlsession不同
   - sqlsession相同，查询条件不同
   - sqlsession相同，两次查询之间执行过增删改
   - sqlsession相同，手动清除过一级缓存
- 二级缓存:基于namespace级别的缓存，一个namespace对应一个二级缓存
   - 工作机制：
    - 一个会话，查询一条数据，这个查询数据会被放在当前会话的一级缓存中
    - 如果会话关闭，一级缓存中的数据会被保存到二级缓存中；新的会话查询信息，就可以参照二级缓存
    - 不同namespace查询的数据会放在自己的map中



MBG.xml配置文件

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!--mybatis的代码生成器相关配置-->
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application-dev.properties"/>

    <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
        property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
        javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
    -->
    <context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat">

        <!-- 这个插件给生成的Java模型对象增加了equals和hashCode方法 -->
        <!--<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>-->

        <!-- 注释 -->
        <commentGenerator>
            <!-- 是否不生成注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 不希望生成的注释中包含时间戳 -->
            <!--<property name="suppressDate" value="true"/>-->
            <!-- 添加 db 表中字段的注释，只有suppressAllComments为false时才生效-->
            <!--<property name="addRemarkComments" value="true"/>-->
        </commentGenerator>


        <!-- jdbc连接 -->
        <jdbcConnection driverClass="${spring.datasource.driverClassName}"
                        connectionURL="${spring.datasource.url}"
                        userId="${spring.datasource.username}"
                        password="${spring.datasource.password}">
            <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!--是否使用bigDecimal，默认false。
                false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
                true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
            <property name="forceBigDecimals" value="true"/>
            <!--默认false
                false，将所有 JDBC 的时间类型解析为 java.util.Date
                true，将 JDBC 的时间类型按如下规则解析
                    DATE	                -> java.time.LocalDate
                    TIME	                -> java.time.LocalTime
                    TIMESTAMP               -> java.time.LocalDateTime
                    TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
                    TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
                -->
            <!--<property name="useJSR310Types" value="false"/>-->
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="com.wqlm.boot.user.po" targetProject="src/main/java">
            <!-- 是否让 schema 作为包的后缀，默认为false -->
            <!--<property name="enableSubPackages" value="false"/>-->
            <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成Mapper.xml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <!--<property name="enableSubPackages" value="false"/>-->
        </sqlMapGenerator>

        <!-- 生成 XxxMapper.java 接口-->
        <javaClientGenerator targetPackage="com.wqlm.boot.user.dao" targetProject="src/main/java" type="XMLMAPPER">
            <!--<property name="enableSubPackages" value="false"/>-->
        </javaClientGenerator>


        <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
             tableName为对应的数据库表名
             domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
             enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
             -->
        <table schema="" tableName="user" domainObjectName="User"
               enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false" selectByExampleQueryId="false">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
    </context>
</generatorConfiguration>

```

