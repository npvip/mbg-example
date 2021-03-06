# mbg-example
MyBatis generator example  
* Maven
* IntelliJ IDEA
* MySQL
* JDK1.8

# 使用步骤  
## 1.引入依赖jar包和插件到`pom.xml`
```
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.3.7</version>
</dependency>

<build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <configuration>
                    <!-- 可以在此配置配置文件的路径,默认路径${basedir}/src/main/resources/generatorConfig.xml
                    <configurationFile></configurationFile>
                    -->
                    <overwrite>true</overwrite>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>8.0.12</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
```

## 2.编写generatoeConfig.xml文件  
 默认路径是在${basedir}/src/main/resources/generatorConfig.xml，也可以自己定义名称和路径在pom.xml中配置。  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!-- 引入mysql-connector-java-xxxx.jar位置,项目中已引入该jar,可以不配置该选项
    <classPathEntry location="/usr/local/mvnrepo/mysql/mysql-connector-java/8.0.12/mysql-connector-java-8.0.12.jar"></classPathEntry>
    -->
    <context id="MySQLTables" targetRuntime="MyBatis3Simple">
        <!-- 生产Java文件的编码  -->
        <property name="javaFileEncoding" value="UTF-8"></property>

        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter" />

        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter" />

        <!-- 生成的pojo，将implements Serializable,为生成的Java模型类添加序列化接口-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>

        <!-- 为生成的Java模型创建一个toString方法 -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />

        <!-- 关闭注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"></property>
        </commentGenerator>

        <!-- 数据库连接 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis_test"
                        userId="test"
                        password="test_1234">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- 指定java bean的生产策略 -->
        <javaModelGenerator targetPackage="cn.mbg.example.model" targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- sql映射生产策略 -->
        <sqlMapGenerator targetPackage="cn.mbg.example.mapper"  targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <!-- mapper接口所在位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="cn.mbg.example.dao"  targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <!-- 指定表 -->
        <table tableName="tbl_dept" domainObjectName="Department"></table>
        <table tableName="employee" domainObjectName="Employee"></table>

    </context>
</generatorConfiguration>
```

## 3.配置maven运行
![maven配置](https://github.com/npvip/mbg-example/blob/master/img/idea-mbg.png)

# 注意
* 注意JDK版本：如果JDK版本低于1.8，则`mybatis-generator-maven-plugin`,`mybatis-generator-core`需选择靠前的版本(JDK1.7->1.3.2有效)

# 参考  
* MyBatis Generator:http://www.mybatis.org/generator/index.html
