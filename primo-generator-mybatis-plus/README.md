# primo-generator  

项目地址： 
[https://github.com/chenhaoxiang/primo](https://github.com/chenhaoxiang/primo)  

## primo-generator-mybatis-plus-maven-plugin
自动生成代码插件,通过Maven插件配置，一键生成单表的所有CRUD操作，加速你的业务开发

持久层基于Mybatis-Plus

## 主要功能
- 单表增删改查
- 单表的条件查询，分页查询
- 基于druid，数据库监控
- 统一查询条件模型
- 统一controller层模型

# 使用说明  一步到位

注意，必须要修改的点：
- 其中的父类包名修改为自己需要的包名 
- 数据源的配置，连接、账号、密码

其他的一些配置项，按照自己的需要进行配置。  

## 插件依赖
```xml
<plugin>
    <groupId>wiki.primo.generator</groupId>
    <artifactId>primo-generator-mybatis-plus-maven-plugin</artifactId>
    <version>1.0.0</version> 
    <configuration>
        <!-- 输出目录(默认java.io.tmpdir) ,项目路径下-->
        <outputDir>src/main/java/</outputDir>
        <!-- 是否覆盖同名文件(默认false) -->
        <fileOverride>true</fileOverride>
        <!-- mapper.xml 中添加二级缓存配置(默认true) -->
        <enableCache>true</enableCache>
        <!-- 开发者名称 -->
        <author>chenhx</author>
        <!-- 是否开启 ActiveRecord 模式(默认true) -->
        <activeRecord>false</activeRecord>
<!--query.open 此配置数据在0.0.5版本已经进行删除-->
        <query>
            <!-- 是否开启 条件分页查询以及相关类生成 -->
            <open>true</open>
        </query>
        <!-- 数据源配置，( **必配** ) -->
        <dataSource>
            <driverName>com.mysql.jdbc.Driver</driverName>
            <url>jdbc:mysql://127.0.0.1:3306/love?useUnicode=true&amp;characterEncoding=UTF-8&amp;zeroDateTimeBehavior=convertToNull&amp;serverTimezone=GMT%2B8</url>
            <username>root</username>
            <password>12345678</password>
        </dataSource>
        <strategy>
            <!-- 字段生成策略，四种类型，从名称就能看出来含义：
                nochange(默认),
                underline_to_camel,(下划线转驼峰)
                remove_prefix,(去除前缀，后面保持不变) 
                remove_prefix_and_camel(去除前缀，后面转驼峰)  -->
            <naming>underline_to_camel</naming>
            <!-- 表前缀 -->
            <tablePrefix> </tablePrefix>
            <!--Entity中的ID生成策略（默认 id_worker）AUTO - 自动-->
            <idGenType>auto</idGenType>
            <!--自定义超类-->
            <!--<superServiceClass>com.baomidou.base.BaseService</superServiceClass>-->
            <!-- include（包含） 与exclude（排除） 二选一配置
                 可以都不进行配置，则默认生成数据库下全部的表 -->
            <include>
                <property>user</property>
<!--            <property>table1</property>-->
            </include>
            <!-- 要排除的表 -->
            <!--<exclude>-->
            <!--<property>schema_version</property>-->
            <!--</exclude>-->
        </strategy>
        <packageInfo>
            <!-- 父级包名称，如果不写，下面的service等就需要写全包名(默认com.baomidou) -->
            <parent>wiki.primo.generator.mybatis.plus.springbootdemo</parent>
            <!--service包名(默认service)-->
            <service>service</service>
            <!--serviceImpl包名(默认service.impl)-->
            <serviceImpl>service.impl</serviceImpl>
            <!--entity包名(默认entity)-->
            <entity>entity</entity>
            <!--mapper包名(默认mapper)-->
            <mapper>mapper</mapper>
            <!--xml包名(默认mapper.xml)-->
            <xml>mapper.xml</xml>
            <query>query</query>
        </packageInfo>
<!--模板路径配置项-->
        <template>
            <!-- 定义controller模板的路径 -->
            <!--<controller>/template/controller1.java.vm</controller>-->
        </template>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
    </dependencies>
</plugin>
```

# 其他运行项目必备的配置
当添加插件依赖后，已经可以运行项目生成的代码了。  

但是运行项目，会报错，那么进行下面几步后，可以解决。  

## 一 添加依赖
必备的jar包依赖，版本请自行控制  
```xml
<!--Spring Boot web依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--mybatis-plus依赖-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.2.0</version>
</dependency>
<!--druid依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.14</version>
</dependency>
<!--MySQL JDBC驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

## 二 扫描Mapper
在@SpringBootApplication注解的启动类上加入 
```java
//第二步：扫描mapper
@MapperScan("你mapper接口所在的包名")
```

## 三 mybati plus配置
```properties
##mybatis-plus mapper xml 文件地址
mybatis-plus.mapper-locations=classpath*:mapper/*Mapper.xml
##mybatis-plus type-aliases 文件地址
mybatis-plus.type-aliases-package=entity实体类的包名（例：wiki.primo.generator.mybatis.plus.springbootdemo.entity）
# 驼峰下划线转换
mybatis-plus.configuration.map-underscore-to-camel-case=true
# 配置的缓存的全局开关
mybatis-plus.configuration.cache-enabled=true
# 延时加载的开关
mybatis-plus.configuration.lazy-loading-enabled=true
```

## 四 其他
数据源配置、druid监控配置以及其他的配置，请查看演示项目下的application.properties文件  

后续若有其他功能，会及时更新演示项目的配置  
 
# 使用演示

## primo-generator-maven-plugin-spring-boot-demo
springboot项目使用primo-generator-maven-plugin演示  

可以看到，这个项目下目前是没有类的 
 
![4rEv9n](https://raw.githubusercontent.com/chenhaoxiang/github-images/master/2020/12/28/4rEv9n.jpg)

在项目的pom文件中开始进行添加插件的依赖（如果是子模块，在子模块的pom中添加）    

![Io7PZ2](https://raw.githubusercontent.com/chenhaoxiang/github-images/master/2020/12/28/Io7PZ2.jpg)  

我在这里只配置了一个表user进行生成
```xml
<!-- include（包含） 与exclude（排除） 二选一配置
     可以都不进行配置，则默认生成数据库下全部的表 -->
<include>
    <property>user</property>
</include>
``` 

当配置项都配置好之后，就可以运行了。  
 
有两种方式运行： 
（1）IDEA可以直接在Maven视图中双击运行  

![D58wIz](https://raw.githubusercontent.com/chenhaoxiang/github-images/master/2020/12/28/D58wIz.jpg)  

（2）也可以通过命令行执行：
```shell script
mvn -DskipTests=true wiki.primo.generator:primo-generator-mybatis-plus-maven-plugin:code
```

执行完成后，可以看到如下的代码生成  

![ofKNGY](https://raw.githubusercontent.com/chenhaoxiang/github-images/master/2020/12/28/ofKNGY.jpg)  

若生成后，有报错，请查看必备的配置，看看依赖和mapper扫描是否添加。  

# 版本更新说明 
请使用最新版本,从1.0.0开始上传到中央仓库

## 1.0.0
- 删除query.open开关的配置，强制开启条件分页查询以及相关类生成
- 将一些配置的数据进行通用的一个抽取，方便后期进行扩展类
- druid管理HTML页面上的“Reset All”功能开启 


## 0.0.4
- service层与controller层的page接口，页数与数量参数分离  
- QueryBo类分离查询条件属性，避免与数据库表列名一致导致异常  
- service层增加通过QueryWrapper查询的分页接口 
- service层增加单一数据的返回查询接口 
- 实体类生成toString方法  
- service层增加修改接口 
- 分页500的限制放开 
- service层增加根据一列的条件修改实体接口
- service层增加list查询接口


## 0.0.3
- 增加controller层的增删改查操作   
- 增加ResultModel的自动生成    
- 增加ResultCodeEnum的自动生成  
- 增加MybatisPlusConfig的自动生成 
- 支持配置的表生成，或者排除表生成代码 


## 0.0.2 
- 生成Query类，根据查询条件分页查询。支持配置是否生成分页查询方法  
```java
<query>
<!-- 是否开启 条件分页查询以及相关类生成 -->
    <open>true</open>
</query>
```

## 0.0.1 
- 仅支持单库全表生成  
- 支持设置entityColumnConstant，控制是否启用属性常量,默认开启  
- 自动给列名加上``关键字转义符  
- 跳过已经存在的文件，fileOverride  
- 支持单表/全库表生成配置，include  

# 其他
有什么问题，或者有什么其他的功能需求，欢迎提出
