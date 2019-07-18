[TOC]

# 一、 Dubbo 概述

`Dubbo`是一个`RPC`框架，是一套分布式系统架构的解决方案。

## 1、 什么是分布式系统

分布式系统（`Distributed System`）是建立在网络上的若干个独立计算机服务的集合，每个计算机服务对于用户而言都是一个独立的计算机服务。



## 2、 软件结构演变历程

### （1）  单一应用架构

软件的整体架构模式为单体架构模式，单机部署的方式实现的软件应用模式。

该模式下软件中所有功能集中开发和部署，如下图所示：

<img src="assets/image-20190718114652198.png" style="width:400px;">

| 优点           | 需要补充              |
| -------------- | --------------------- |
| 开发技术栈简单 | 多人协同困难          |
| 功能稳定       | 功能扩展困难          |
| 性能稳定       | 性能提升 垂直扩展有限 |



### （2）  垂直应用架构

软件使用垂直架构模式，集群不是的方式实现的软件应用模式。  

<img src="assets/image-20190718115610498.png" style="width: 800px;height:auto;">



| 优点                 | 需要补充               |
| -------------------- | ---------------------- |
| 多人协同开发容易     | 模块之间互相调用不便   |
| 功能扩展方便         | 功能维护难度较高       |
| 模块化划分责任明确   | 上下层代码之间耦合度高 |
| 单体业务性能扩展方便 |                        |

### （3） 前后端分离架构

软件架构时将前端交互部分独立出来作为独立的项目进行开发，和服务端程序进行解耦合操作，在垂直架构的模式上扩展实现。

<img src="assets/image-20190718120653416.png" style="width: 800px;">

| 优点                 | 需要补充         |
| -------------------- | ---------------- |
| 多人协同方便         | 远程调用困难     |
| 前后端业务解耦合     | 前端技术难度提升 |
| 服务扩展功能更加方便 | 服务粒度不好控制 |
| 单体业务性能扩展方便 |                  |



### （4）  分布式架构

软件在垂直应用和前后端分离架构的基础上，将软件的后端业务受理服务进行细粒度拆分，服务之间解耦合完成高可用部署。

<img src="assets/image-20190718122525861.png" style="width: 800px;">

### （5） 分布式集群架构

在分布式的基础上，水平扩展多态服务器，根据业务的处理压力进行服务器的分配部署和调优。



### （6） 面向服务架构

在分布式集群的基础上，对基础服务进行服务治理，完善服务分配及调用模式，将最优的性能分配到压力最大的服务，支撑软件系统的高可用性。



## 3、 `RPC`远程过程调用

软件开发从垂直架构模式开始，远程调用就变得非常重要，`RPC（remote procedure call）`就是为此而出现的一种编程思想，语序一个服务可以在不知道远程服务底层实现的情况下，请求调用远程服务器暴露的接口获取需要的数据。

<img src="assets/image-20190718131927470.png" style="width:800px;">

处理过程中通信网络和参数的处理过程，直接决定了`RPC`框架的处理效率，市场上目前比较成熟可用的技术选型有很多种

-   `Apache Dubbo`
-   `Google gRPC`
-   `Facebook Thrift`
-   `Alibaba HSF`
-   `more...`



## 4、 `Apache Dubbo`

官方网站：`http://dubbo.apache.org/en-us/`

<img src="assets/image-20190718132611567.png" style="width: 600px;">

### （1） `Dubbo`概述

阿里的一个开源框架，在2011年托管`apache`开源发布，但是在2014年停止了更新

2018年1月`Alibaba`将`Dubbo`和`DubboX`合并发布2.6.x版本，并于2月份贡献给`Apache`成为了一个成熟的子项目

<img src="assets/image-20190718132659954.png" style="width: 700px;">

`Dubbo`通过`服务治理`的`服务注册中心`，可以将软件服务进行合理的整合和调用，并就所有服务器的处理压力进行负载均衡，在服务处理过程中通过`流量调度`完成灰度发布的实现，同时提供了可视化服务能让项目很友好的使用`Dubbo`完成整体项目的信息监控。

### （2）  `Dubbo`架构

官方提供的`Dubbo`基本架构图，提供了基本的几个构建组件：

![image-20190718133412692](assets/image-20190718133412692.png)

架构中的组件：

-   `Container`：服务容器
-   `Provider`：服务提供者
-   `Consumer`：服务消费者
-   `Registry`：注册中心
-   `Monitor`：监控

运行流程：

0.  `Container`容器启动，加载服务提供者`Provider`
1.  将服务提供者注册到注册中心`Registry`
2.  服务消费者从注册中心订阅`(subscribe)`提供者服务`Provider Service`
3.  注册中心`Registry`和消费者`Consumer`之间就服务的变动完成通信
4.  消费者`Consumer`根据注册中心反馈的服务信息调用提供者`Provider`服务
5.  监控中心`Monitor`就服务的提供和调用完成细节的监控处理

### （3） 注册中心

`Dubbo`官方支持多种注册中心服务组件，如`Multicast`、`Zookeeper`、`Nacos`、`Redis`以及自身提供的`Simple`注册中心，但是官方推荐使用`Zookeeper`作为自身的注册中心。

`Zookeeper`的使用方式请参考 [大牧絮叨系列-Zookeeper](https://laomu.github.io/大牧絮叨系列-Zookeeper/Zookeeper%20Tutorial)

### （4） 监控中心

目前的管理控制台已经发布0.1版本，结构上采取了前后端分离的方式，前端使用Vue和Vuetify分别作为Javascript框架和UI框架，后端采用Spring Boot框架。既可以按照标准的Maven方式进行打包，部署，也可以采用前后端分离的部署方式，方便开发，功能上，目前具备了服务查询，服务治理(包括Dubbo2.7中新增的治理规则)以及服务测试三部分内容。【引用官方内容】

（1）  `maven`方式部署安装

```shell
# 通过git方式下载远程仓库项目文件
$ git clone https://github.com/apache/dubbo-admin.git

# 使用`maven`命令进行清理并重新打包
$ cd dubbo-admin
$ mvn clean package

# 运行打包后的jar文件
$ cd dubbo-admin-distribution/target
$ java -jar dubbo-admin-0.1.jar
```



（2） 前后端分离方式安装

前端：

```shell
# 进入下载的源代码-ui文件夹
$ cd dubbo-admin-ui 
# 使用node包管理命令安装前端依赖库文件
$ npm install 
# 启动服务
$ npm run dev 
```

后端：

```shell
# 进入下载的源代码-server目录
$ cd dubbo-admin-server
# 清理并重新打包
$ mvn clean package 
# 运行打包后的jar文件
$ cd target
$ java -jar dubbo-admin-server-0.1.jar
```

（3） 自定义配置

远程下载的监控模块的配置主要包含在`dubbo-admin-server/src/main/resources/application.properties`中，通常获取到本地之后需要修改的核心配置：

```properties
# 配置中心地址
admin.config-center=zookeeper://127.0.0.1:2181
# 注册中心地址
admin.registry.address=zookeeper://127.0.0.1:2181
# 元数据中心地址
admin.metadata-report.address=zookeeper://127.0.0.1:2181
```

（4） 访问测试

分别启动前后端服务应用

```shell
# 启动前端应用
$ cd dubbo-admin-ui
$ npm run dev

# 启动后端应用
$ cd dubbo-admin-server
$ java -jar dubbo-admin-server-0.1.jar
```

打开浏览器访问`http://localhost:8080`

<img src="assets/image-20190718230730658.png"  style="width: 800px;">





## 5、`Dubbo Xml`实现

首先构建一个基于`Xml`配置方式的`Dubbo`架构的软件应用，快速了解`Dubbo`中的各个功能模块和组件之间的依赖及调用关系。

### （1）  创建`api`模块应用

基于`Dubbo最佳应用实践`的指导，我们将 `数据模型`、`功能接口`等都定义在`api`模块中，方便其他模块的使用。

创建`maven`项目：`dubbo2019-api`

#### a. `pom.xml`添加依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dubbo2019</artifactId>
        <groupId>dubbo2019</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>dubbo2019-api</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.8</version>
        </dependency>
    </dependencies>
</project>
```

#### b. 创建实体处理类

部门类型和职员类型【附录1中提供了测试用数据】，包含在`com.damu.entity`包中：

需要注意~这里定义的数据模型类，因为后续设计到`RPC`调用，所以必须实现序列化接口。

-   `Department.java`
-   `Employee.java`

```java
package com.damu.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.experimental.Accessors;

/**
 * <p>项目文档： 部门数据</p>
 *
 * @author 大牧
 * @version V1.0
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
@Accessors(chain = true)
public class Department implements Serializable {
    private Long        deptNo;     //  部门编号
    private String      deptName;   //  部门名称
    private String      location;   //  部门所在地区
}
```

```java
package com.damu.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.experimental.Accessors;

import java.util.Date;

/**
 * <p>项目文档： 职员数据</p>
 *
 * @author 大牧
 * @version V1.0
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
@Accessors(chain = true)
public class Employee implements Serializable {
    private Long            empNo;      // 职员编号
    private String          empName;    // 职员名称
    private String          nickname;   // 职员昵称
    private String          job;        // 职员岗位
    private Long            mgr;        // 上级编号
    private Date            hirdate;    // 入伙时间
    private Integer         salary;     // 薪水待遇
    private Integer         comm;       // 奖金福利
    private Long            deptNo;     // 所属部分
}
```

#### c. 构建业务受理通用接口

包含在`com.damu.service`包中

-   `IService.java`
-   `DepartmentService.java`
-   `EmployeeService.java`

```java
package com.damu.service;

import java.util.List;

/**
 * <p>项目文档： 通用服务受理接口</p>
 *
 * @author 大牧
 * @version V1.0
 */
public interface IService<T> {

    /**
     * 增加数据
     * @param t 要增加的数据
     * @return 影响的记录数
     */
    Integer add(T t);

    /**
     * 根据编号删除数据
     * @param id 要删除的数据编号
     * @return 影响的记录数
     */
    Integer delete(String id);

    /**
     * 更新数据
     * @param t 要更新的数据
     * @return 影响的记录数
     */
    Integer update(T t);

    /**
     * 查询指定的所有数据
     * @return 数据列表
     */
    List<T> getAll();

    /**
     * 根据编号获取对应数据
     * @param id 数据编号
     * @return 数据
     */
    T getById(String id);
}
```

```java
package com.damu.service;

import com.damu.entity.Department;

/**
 * <p>项目文档： 部门业务受理接口</p>
 *
 * @author 大牧
 * @version V1.0
 */
public interface DepartmentService extends IService<Department>{

}
```

```java
package com.damu.service;

import com.damu.entity.Employee;

/**
 * <p>项目文档： 职员类型业务接口</p>
 *
 * @author 大牧
 * @version V1.0
 */
public interface EmployeeService extends IService<Employee>{

}
```

### （2） 创建服务模块

这里的服务模块是抽象的形式表示项目中的一个`功能提供模块`，在业务受理过程中可能会被其他模块`调用`

<img src="assets/image-20190719001809200.png" style="width: 600px;">

#### a. 构建`maven`项目：`dubbo2019-provider-8001`

这一部分我们的技术选型如下：

-   数据库连接池：`druid`[复习、回顾、整理]
-   数据`ORM`：`mybatis xml`[复习、回顾、整理]

修改`pom.xml`添加依赖关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dubbo2019</artifactId>
        <groupId>dubbo2019</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>dubbo2019-provider-8001</artifactId>

    <dependencies>
        <!-- 基本依赖 -->
        <dependency>
            <groupId>dubbo2019</groupId>
            <artifactId>dubbo2019-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!-- 数据访问依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.1</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.18</version>
        </dependency>
        <!-- mybatis-spring适配 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <!-- spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
</project>
```



#### b. 构建`mapper`接口

-   `DepartmentMapper.java`
-   `EmployeeMapper.java`

```java
package com.damu.mapper;

import com.damu.entity.Department;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * <p>项目文档： 部门关系映射</p>
 *
 * @author 大牧
 * @version V1.0
 */
@Repository
public interface DepartmentMapper {

    /**
     * 增加数据
     * @param department 要增加的数据
     * @return 影响的记录数
     */
    Integer add(Department department);

    /**
     * 根据编号删除数据
     * @param id 要删除的数据编号
     * @return 影响的记录数
     */
    Integer delete(String id);

    /**
     * 更新数据
     * @param department 要更新的数据
     * @return 影响的记录数
     */
    Integer update(Department department);

    /**
     * 查询指定的所有数据
     * @return 数据列表
     */
    List<Department> findAll();

    /**
     * 根据编号获取对应数据
     * @param id 数据编号
     * @return 数据
     */
    Department findById(String id);
}
```

```java
package com.damu.mapper;

import com.damu.entity.Department;
import com.damu.entity.Employee;

import java.util.List;

/**
 * <p>项目文档： 职员关系映射</p>
 *
 * @author 大牧
 * @version V1.0
 */
public interface EmployeeMapper {

    /**
     * 增加数据
     * @param employee 要增加的数据
     * @return 影响的记录数
     */
    Integer add(Employee employee);

    /**
     * 根据编号删除数据
     * @param id 要删除的数据编号
     * @return 影响的记录数
     */
    Integer delete(String id);

    /**
     * 更新数据
     * @param employee 要更新的数据
     * @return 影响的记录数
     */
    Integer update(Employee employee);

    /**
     * 查询指定的所有数据
     * @return 数据列表
     */
    List<Employee> findAll();

    /**
     * 根据编号获取对应数据
     * @param id 数据编号
     * @return 数据
     */
    Employee findById(String id);
}
```

#### c. 创建对应的`Mapper映射`配置文件

-   `resources/mapper/DeptMapper.xml`
-   `resources/mapper/EmpMapper.xml`

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--命名空间应该是对应接口的包名+接口名 -->
<mapper namespace="com.damu.mapper.DepartmentMapper">

    <insert id="add" parameterType="com.damu.entity.Department">
        insert into department(deptname, location)
            values(#{deptname}, #{location})
    </insert>

    <delete id="delete">
        delete from department where deptno = #{id}
    </delete>

    <update id="update" parameterType="com.damu.entity.Department">
        update department
            <trim prefix="set" suffix=",">
                <if test="deptname != null">deptname = #{deptname}</if>
                <if test="location != null">location = #{location}</if>
            </trim>
        where deptno = #{deptno}
    </update>

    <select id="findById" resultType="com.damu.entity.Department">
        select * from department where deptno = #{id}
    </select>

    <select id="findAll" resultType="com.damu.entity.Department">
        select * from department
    </select>
</mapper>
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--命名空间应该是对应接口的包名+接口名 -->
<mapper namespace="com.damu.mapper.EmployeeMapper">

    <insert id="add" parameterType="com.damu.entity.Employee">
        insert into Employee(empname, nickname, job, mgr, hirdate, salary, comm, deptno)
        values(#{empname}, #{nickname}, #{job}, #{mgr}, #{hirdate}, #{salary}, #{comm}, #{deptno})
    </insert>

    <delete id="delete">
        delete from employee where empno = #{id}
    </delete>

    <update id="update" parameterType="com.damu.entity.Employee">
        update employee
        <trim prefix="set" suffix=",">
            <if test="empname != null">empname = #{empname}</if>
            <if test="nickname != null">nickname = #{nickname}</if>
            <if test="job != null">job = #{job}</if>
            <if test="mgr != null">mgr = #{mgr}</if>
            <if test="hirdate != null">hirdate = #{hirdate}</if>
            <if test="salary != null">salary = #{salary}</if>
            <if test="comm != null">comm = #{comm}</if>
            <if test="deptno != null">deptno = #{deptno}</if>
        </trim>
        where empno = #{empno}
    </update>

    <select id="findById" resultMap="employee">
        select * from department where deptno = #{id}
    </select>

    <select id="findAll" resultMap="employee">
        select * from department
    </select>
    
    <resultMap id="employee" type="com.damu.entity.Employee">
        <id property="empno" column="empno"/>
        <result property="nickname" column="nickname"/>
        <result property="job" column="job"/>
        <result property="mgr" column="mgr"/>
        <result property="hirdate" column="hirdate"/>
        <result property="salary" column="salary"/>
        <result property="comm" column="comm"/>
        <association property="deptno" javaType="com.damu.entity.Department">
            <id property="deptno" column="deptno"/>
            <result property="deptname" column="deptname"/>
            <result property="location" column="location"/>
        </association>
    </resultMap>
</mapper>
```

#### d. 配置数据库连接信息映射

使用简单的`properties`配置基本连接信息：`resources/db.properties`

```properties
#mysql jdbc
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/dubbo2019?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=Root2019
```

#### e. 配置`spring.xml`完成`spring项目`全局配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
    <context:component-scan base-package="com.damu"/>
    <!-- 配置dao层扫描 -->
    <context:property-placeholder location="classpath:db.properties" />
    <!-- 通过JDBC模版获取数据库连接 -->
    <bean id="jdbcTemplate"
          class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!-- 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <property name="driverClassName" value="${jdbc.driverClassName}" />
    </bean>

    <!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 加载mybatis的全局配置文件 -->
        <property name="mapperLocations" value="classpath:mapper/*.xml" />
    </bean>

    <!-- 配置mapper扫描包 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.damu.mapper" />
    </bean>
</beans>
```

#### d. 测试服务端项目

我们通过`Junit`完成上述项目的单元测试，通过类`com.damu.service.DepartmentServiceImplTest.java`完成整体的项目功能访问测试即可。

```java
package com.damu.service;

import com.damu.entity.Department;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.List;

import static org.junit.Assert.*;

public class DepartmentServiceImplTest {

    ClassPathXmlApplicationContext ioc;
    DepartmentServiceImpl departmentService;

    @Before
    public void setUp() throws Exception {
        ioc = new ClassPathXmlApplicationContext("spring.xml");
        departmentService = ioc.getBean(DepartmentServiceImpl.class);
    }

    @Test
    public void getAll() {
        List<Department> depts = departmentService.getAll();
        for (Department dept : depts) {
            System.out.println(dept);
        }
    }

    @Test
    public void getById() {
        Department department = departmentService.getById("10");
        System.out.println(department);
    }
}
```

### （3） 创建消费模块

消费模块并不是字面意义上的`消费`，而是在服务模块化开发时，需要依赖其他模块服务的单元模块，这里我们通过消费模块模拟项目中的多个模块之间互相依赖的关系。

<img src="assets/image-20190719001809200.png" style="width: 600px;">

#### a. 构建`maven`项目：`dubbo2019-consumer-9001`

该项目主要是远程调用服务模块提供的数据，所以我们这里的代码会进行远程调用的简单模拟。

修改`pom.xml`添加项目依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dubbo2019</artifactId>
        <groupId>dubbo2019</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>dubbo2019-consumer-9001</artifactId>
    <dependencies>
        <!-- 基础依赖 -->
        <dependency>
            <groupId>dubbo2019</groupId>
            <artifactId>dubbo2019-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!-- Spring依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.1.8.RELEASE</version>
        </dependency>
        <!-- 测试相关 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
</project>
```

#### b. 基本调用代码

如果是常规项目，我们可以通过项目之间的依赖关系直接注入使用

<img src="assets/image-20190719003203693.png" style="width: 400px;">

将`dubbo2019-provider-8001`当成`dubbo2019-consumer-9001`的依赖，就可以通过下面的代码调用：

```java
package com.damu.service;

import com.damu.entity.Department;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * <p>项目文档： 消费端业务受理类</p>
 *
 * @author 大牧
 * @version V1.0
 */
@Service
public class ConsumerService {

    @Autowired
    private DepartmentService departmentService;

    /**
     * 模拟业务：查询可供参观的部门
     * @return 返回查询到符合条件的部门
     */
    public List<Department> visitDepartmentByCondition() {
        return departmentService.getAll();
    }
}
```

但是此时的项目部署场景，`dubbo2019-provider-8001`可能部署在一个服务器上，而`dubbo2019-consumer-2019`可能部署在远端的另一台服务器上，我们如果使用依赖的方式是非常不方便的，此时我们提到的`RPC`就是类似场景的非常优雅的解决方案之一了。

<img src="assets/image-20190719003607357.png" style="width: 700px;">



### （4） `Dubbo RPC`配置实现

参考`Dubbo`的基本架构图，我们可以看到如果两个服务之间要实现远程`RPC`功能，需要将服务管理在`注册中心`这样一个组件中，进行`服务注册`和`服务发现`两个环节，最终完成`远程过程调用/RPC`功能。

<img src="assets/image-20190718133412692.png" style="width: 600px;">

#### a. 注册中心

`Dubbo`的注册中心的技术选型有很多种，官方推荐使用`Zookeeper`作为其注册中心使用。

关于`Zookeeper`的基本使用方式，请移步 [大牧絮叨系列-Zookeeper](https://laomu.github.io/大牧絮叨系列-Zookeeper/Zookeeper%20Tutorial)

我们通过本机部署的`Zookeeper`进行测试，在后面的章节中再进行高可用拓展。

执行命令启动`Zookeeper`

```shell
$ zkServer.sh start
```

启动完成后查看启动状态：

```shell
$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /software/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: standalone[单机模式]
```

#### b. 服务注册

将服务提供者`dubbo2019-provider-8001`注册到`Zookeeper`中，完成服务的发布。

参考`Dubbo官方文档`的 [服务注册Zookeeper注册中心](https://dubbo.apache.org/zh-cn/docs/user/references/registry/zookeeper.html)

**第一步：增加连接注册中心的依赖，**在找一个环节中需要注意，`Zookeeper`在`2.7`版本中移除了`zkClient`的默认依赖，而是开始使用`curator`完成`Zookeeper`注册中心的访问了。

```xml
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.7.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.2.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.2.0</version>
</dependency>
```

>   注意问题1：如果你使用的`dubbo`是2.6或者以下版本，请引入`zkClient依赖`,参考下面的代码。
>
>   注意问题2：如果你使用的`dubbo2.7`版本，并且使用了`curator4`及以上版本，官方手册提示`zookeeper`必须是`3.5+`以上版本才支持，否则会出现方法实现检查异常`KeeperErrorCode = Unimplemented`
>
>   注意问题3：`zookeeper3.5`增加新特性，启动时会默认使用`8080`端口，如果你的端口被占用，请修改配置文件`zoo.cfg`增加`admin.serverPort`选项修改占用端口

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/dubbo -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.6</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.101tec/zkclient -->
<dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.11</version>
</dependency>
```

**第二步：添加服务发布配置，**参考官方文档 快速入门 部分的配置，在当前项目`resources/`目录中添加陪配置文件`provider.xml`，该配置文件中完成服务的发布处理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 服务提供方信息，用于计算依赖关系：一般是当前发布应用的名称 -->
    <dubbo:application name="dubbo2019-provider-8001"/>

    <!-- 注册中心暴露服务地址 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"/>

    <!-- 指定服务暴露的协议及端口 -->
    <dubbo:protocol name="dubbo" port="28888"/>

    <!-- 声明需要暴露服务的接口：一定要指定具体实现 -->
    <dubbo:service interface="com.damu.service.DepartmentService" ref="departmentServiceImpl"/>
    <!-- 服务接口的具体实现 -->
    <bean id="departmentServiceImpl" class="com.damu.service.DepartmentServiceImpl"/>

</beans>
```

将该配置文件，包含到`spring.xml`主配置文件中

```xml
<import resource="provider.xml"/>
```

#### c. 启动测试服务

修改启动类`com.damu.ProviderApp.java`，让程序启动后阻塞，表示程序会持续提供服务。

```shell
# 启动zookeeper
$ zkServer.sh start

# 启动监测平台服务端
$ java -jar dubbo-admin-server-0.1.jar
# 启动监测平台前端
$ npm run dev

# 启动服务提供端
$ java -jar dubbo2019-provider-8001-1.0-SNAPSHOT.jar
```

此时稍作等待，我们查看监测界面就能看到`zookeeper`注册中心发现的服务

![image-20190719021700049](assets/image-20190719021700049.png)

#### d. 消费端配置调用

`pom.xml`中添加依赖关系

```xml
<!-- dubbo依赖 -->
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.7.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.2.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.2.0</version>
</dependency>
```

消费端创建配置文件，添加`zookeeper`注册中心访问关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 服务消费方信息，用于计算依赖关系：一般是当前发布应用的名称 -->
    <dubbo:application name="dubbo2019-consumer-9001"/>

    <!-- 注册中心暴露服务地址 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"/>

    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference interface="com.damu.service.DepartmentService" id="departmentService"/>

</beans>
```

将`consumer.xml`包含到主配置文件`spring.xml`中

```xml
<import resource="consumer.xml"/>
```



构建业务处理类，引用远程`service`服务接口实现

```java
package com.damu.service;

import com.damu.entity.Department;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * <p>项目文档： 消费端业务受理类</p>
 *
 * @author 大牧
 * @version V1.0
 */
@Service
public class ConsumerService {

    @Autowired
    private DepartmentService departmentService;

    /**
     * 模拟业务：查询可供参观的部门
     * @return 返回查询到符合条件的部门
     */
    public List<Department> visitDepartmentByCondition() {
        return departmentService.getAll();
    }
}
```

测试`RPC`调用实现：

```java
package com.damu;

import com.damu.entity.Department;
import com.damu.service.ConsumerService;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.List;

/**
 * <p>项目文档： 消费端测试程序</p>
 *
 * @author 大牧
 * @version V1.0
 */
public class ConsumerApp {

    public static void main(String[] args) {

        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring.xml");

        ConsumerService service = ioc.getBean(ConsumerService.class);

        List<Department> depts = service.visitDepartmentByCondition();

        for (Department dept : depts) {
            System.out.println(dept);
        }
    }
}
```

测试结果中，消费端成功调用服务提供端的数据

```shell
Department(deptNo=10, deptName=中央, location=梁山本部)
Department(deptNo=20, deptName=近卫, location=梁山本部)
Department(deptNo=21, deptName=军委1部, location=梁山本部)
Department(deptNo=22, deptName=军委2部, location=梁山本部)
Department(deptNo=23, deptName=军委3部, location=梁山本部)
Department(deptNo=30, deptName=财务部, location=梁山本部)
Department(deptNo=40, deptName=参谋部, location=梁山本部)
Department(deptNo=50, deptName=后勤部, location=梁山本部)
Department(deptNo=60, deptName=军情部, location=梁山本部)
Department(deptNo=70, deptName=迎宾部, location=梁山本部)
Department(deptNo=80, deptName=刑罚部, location=梁山本部)
```



## 6、基于注解整合 `Spring Boot`







# 二、 Dubbo 配置



# 三、 高可用架构



# 四、 Dubbo底层原理



# 五、附录

## 1、 测试用数据，基于`MySQL`

```sql
-- 创建数据库
drop database dubbo2019;
create database dubbo2019 default charset 'utf8';
use dubbo2019;

-- 创建表与数据
CREATE TABLE employee(
    empno int auto_increment primary key comment '人物编号',
    empname VARCHAR(10) comment '人物名称',
    nickname varchar(20) comment '昵称',
    job VARCHAR(9) comment '工作岗位',
    mgr int comment '上级编号',
    hirdate DATE comment '入伙时间',
    salary int comment '薪水待遇',
    comm int comment '奖金',
    deptno int comment '所属部门'
);

truncate table employee;
INSERT INTO employee VALUES
(1, '宋江', '天魁星·及时雨', '头领', null,'2015-11-8', 800, 200000, 1),
(2, '卢俊义', '天罡星·玉麒麟','卢俊义', 1,'2018-4-6', 800, 100000, 1),
(3, '吴用', '天机星·智多星', '头领', 1,'2012-8-10', 800, 100000, 1),

(21, '关胜', '天勇星·大刀','五虎上将', 1,'2017-4-6', 20000, 2000, 20),
(22, '林冲', '天雄星 豹子头','五虎上将', 1,'2018-4-6', 21000, 1800, 20),
(23, '秦明', '天猛星 霹雳火','五虎上将', 1,'2018-4-6', 19000, 1900, 20),
(24, '呼延灼', '天威星 双鞭','五虎上将', 1,'2019-4-6', 22000, 1100, 20),
(25, '董平', '天立星·双枪将','五虎上将', 1,'2017-4-6', 18000, 1200, 20),

(2101, '花荣', '天英星 小李广','骑兵头领', 1,'2016-4-6', 16000, 2000, 21),
(2102, '徐宁', '天佑星 金枪手','骑兵都统', 2101,'2015-4-6', 12000, 1200, 21),
(2103, '杨志', '天暗星 青面兽','骑兵都统', 2101,'2016-4-6', 13000, 1200, 21),
(2104, '索超', '天空星 急先锋','骑兵都统', 2101,'2018-4-6', 16000, 1100, 21),
(2105, '张青', '地刑星 菜园子','骑兵都统', 2101,'2016-4-6', 14000, 1000, 21),
(2106, '史进', '天微星 九纹龙','骑兵都统', 2101,'2017-4-6', 15000, 1100, 21),
(2107, '穆虹', '天究星 没遮拦','骑兵都统', 2101,'2015-4-6', 14000, 1000,  21),
(2108, '朱仝', '天满星 美髯公','骑兵都统', 2101,'2016-4-6', 14000, 1000, 21),
(2109, '王英', '地微星 矮脚虎','骑兵都统', 2101,'2017-4-6', 12000, 900, 21),
(2110, '扈三娘', '地慧星 一丈青','骑兵都统', 2101,'2017-4-6', 11000, 800, 21),
(2111, '吕方', '地佐星 小温侯','骑兵都统', 2101,'2016-4-6', 12000, 900, 21),
(2112, '郭盛', '地佑星 赛仁贵','骑兵都统', 2101,'2016-4-6', 12000, 900, 21),

(2201, '鲁智深', '天孤星 花和尚','步军头领', 1,'2015-4-6', 16000, 2000, 22),
(2202, '武松', '天伤星 行者','步军都统', 2201,'2015-4-6', 16000, 1400, 22),
(2203, '刘唐', '天异星 赤髪鬼','步军都统', 2201,'2014-4-6', 13000, 1200, 22),
(2204, '雷横', '天退星 插翅虎','步军都统', 2201,'2016-4-6', 12000, 1200, 22),
(2205, '李逵', '天杀星 黑旋风','步军都统', 2201,'2018-4-6', 11000, 1200, 22),
(2206, '燕青', '天巧星 浪子','步军都统', 2201,'2016-4-6', 13000, 1200, 22),
(2207, '石秀', '天慧星·拚命三郎','步军都统', 2201,'2015-4-6', 12000, 1200, 22),
(2208, '杨雄', '天牢星 病关索','步军都统', 2201,'2014-4-6', 13000, 1200, 22),
(2209, '解珍', '天暴星 两头蛇','步军都统', 2201,'2013-4-6', 12000, 1200, 22),
(2210, '解宝', '天哭星 双尾蝎','步军都统', 2201,'2013-4-6', 12000, 1200, 22),
(2211, '孔明', '地猖星 毛头星','步军偏将', 2201,'2015-4-6', 13000, 1100, 22),
(2212, '孔亮', '地狂星 独火星','步军偏将', 2201,'2016-4-6', 13000, 1000, 22),
(2213, '樊瑞', '地默星 混世魔王','步军偏将', 1201,'2014-4-6', 12000, 1000, 22),
(2214, '鲍旭', '地暴星 丧门神','步军偏将', 1201,'2015-4-6', 12000, 900, 22),
(2215, '项充', '地飞星 八臂哪吒','步军偏将', 2201,'2016-4-6', 11000, 900, 22),
(2216, '李衮', '地走星 飞天大圣','步军偏将', 2201,'2016-4-6', 12000, 1100, 22),
(2217, '薛永', '地幽星 病大虫','步军偏将', 2201,'2014-4-6', 11000, 900, 22),
(2218, '施恩', '地伏星 金眼彪','步军偏将', 2201,'2014-4-6', 12000, 900, 22),
(2219, '穆春', '地镇星 小遮拦','步军偏将', 2201,'2015-4-6', 12800, 900, 22),
(2220, '李忠', '地僻星 打虎将','步军偏将', 2201,'2013-4-6', 11000, 1100, 22),
(2221, '郑天寿', '地异星·白面郎君','步军偏将', 2201,'2016-4-6', 12000, 1100, 22),
(2222, '宋万', '地魔星 云里金刚','步军偏将', 2201,'2016-4-6', 11000, 900, 22),
(2223, '杜迁', '地妖星 摸着天','步军偏将', 2201,'2017-4-6', 10000, 900, 22),
(2224, '邹渊', '地短星 出林龙','步军偏将', 2201,'2017-4-6', 10000, 900, 22),
(2225, '邹润', '地角星 独角龙','步军偏将', 2201,'2018-4-6', 10000, 1100, 22),
(2226, '龚旺', '地捷星 花项虎','步军偏将', 2201,'2018-4-6', 11000, 1000, 22),
(2227, '丁得孙', '地速星 中箭虎','步军偏将', 2201,'2018-4-6', 12000, 900, 22),
(2228, '焦挺', '地恶星 没面目','步军偏将', 2201,'2018-4-6', 11000, 900, 22),
(2229, '石勇', '地丑星 石将军','步军偏将', 2201,'2018-4-6', 13000, 900, 22),

(2301, '李俊', '天寿星 混江龙','水军头领', 1,'2016-4-6', 16000, 2000, 23),
(2302, '张横', '天平星 船火儿','水军都统', 2301,'2017-4-6', 13000, 1000, 23),
(2303, '张顺', '天损星 浪里白条','水军都统', 2301,'2018-4-6', 12000, 1100, 23),
(2304, '阮小二', '天剑星 立地太岁','水军都统', 2301,'2014-4-6', 14000, 1000, 23),
(2305, '阮小五', '天罪星 短命二郎','水军都统', 2301,'2014-4-6', 14000, 1200, 23),
(2306, '阮小七', '天败星 活阎罗','水军都统', 2301,'2014-4-6', 14000, 1000, 23),
(2307, '童威', '地进星 出洞蛟','水军偏将', 2301,'2018-4-6', 13000, 1100, 23),
(2308, '童猛', '地退星 翻江蜃','水军偏将', 2301,'2018-4-6', 12000, 1000, 23),

(3001, '柴进', '天贵星 小旋风','财务部长', 1,'2014-4-6', 15000, 2000, 30),
(3002, '李应', '天富星 扑天雕','财务会计', 3001,'2014-4-6', 13000, 2000, 30),
(3003, '皇甫端', '地兽星 紫髯伯','财务会计', 3001,'2015-4-6', 13000, 2000, 30),

(4001, '公孙胜', '天闲星 入云龙','参谋长', 1,'2015-8-6', 15000, 2000, 40),
(4002, '张清', '天捷星 没羽箭','参谋', 4001,'2016-9-15', 13000, 2000, 40),
(4003, '朱武', '地魁星 神机军师','参谋', 4001,'2017-6-20', 13000, 2000, 40),
(4004, '安道全', '地灵星 神医','参谋', 4001,'2015-10-18', 13000, 2000, 40),
(4005, '宋清', '地俊星 铁扇子','参谋', 4001,'2018-11-16', 13000, 2000, 40),

(5001, '金大坚', '地巧星 玉臂匠','后勤部长', 1,'2015-2-1', 9000, 3000, 50),
(5002, '蒋敬', '地会星 神算子','后勤杂事', 5001,'2015-4-20', 9000, 2000, 50),
(5003, '孟康', '地满星 玉幡竿','后勤杂事', 5001,'2016-5-10', 9000, 2000, 50),
(5004, '侯键', '地遂星 通臂猿','后勤杂事', 5001,'2016-8-16', 9000, 2000, 50),
(5005, '裴宣', '地正星 铁面孔目','后勤杂事', 5001,'2017-12-3', 9000, 2000, 50),
(5006, '汤隆', '地孤星 金钱豹子','后勤杂事', 5001,'2017-1-20', 9000, 2000, 50),
(5007, '凌阵', '地辅星 轰天雷','后勤杂事', 5001,'2018-8-20', 9000, 2000, 50),
(5008, '李云', '地察星 青眼虎','后勤杂事', 5001,'2018-8-21', 9000, 2000, 50),
(5009, '曹正', '地羁星 操刀鬼','后勤杂事', 5001,'2018-9-10', 9000, 2000, 50),
(5010, '朱富', '地藏星 笑面虎','后勤杂事', 5001,'2018-9-15', 9000, 2000, 50),
(5011, '陶宗旺', '地理星 九尾龟','后勤杂事', 5001,'2018-9-22', 9000, 2000, 50),
(5012, '郁保四', '地健星 险道神','后勤杂事', 5001,'2018-10-6', 9000, 2000, 50),

(6001, '戴宗', '天速星 神行太保','军情部长', 1,'2014-2-16', 5000, 10000, 50),
(6002, '乐和', '地乐星 铁叫子','军情都统', 6001,'2015-12-13', 2000, 8000, 50),
(6003, '时迁', '地贼星 鼓上蚤','军情都统', 6001,'2015-10-16', 2000, 8000, 50),
(6004, '段景住', '地狗星 金毛犬','军情都统', 6001,'2016-6-19', 2000, 8000, 50),
(6005, '白胜', '地耗星 白日鼠','军情都统', 6001,'2016-8-20', 2000, 8000, 50),
(6006, '黄信', '地煞星 镇三山','军情远哨', 6001,'2017-4-6', 2000, 8000, 50),
(6007, '孙立', '地勇星 病尉迟','军情远哨', 6001,'2018-12-6', 2000, 8000, 50),
(6008, '宣赞', '地杰星 丑郡马','军情远哨', 6001,'2018-9-30', 2000, 8000, 50),
(6009, '郝思文', '地雄星 井木犴','军情远哨', 6001,'2018-5-21', 2000, 8000, 50),
(6010, '韩滔', '地威星 百胜将','军情远哨', 6001,'2018-5-21', 2000, 8000, 50),
(6011, '彭屺', '地英星 天目将','军情远哨', 6001,'2017-2-16', 2000, 8000, 50),
(6012, '单廷圭', '地奇星 圣水将','军情远哨', 6001,'2016-10-6', 2000, 8000, 50),
(6013, '魏定国', '地猛星 神火将','军情远哨', 6001,'2018-12-3', 2000, 8000, 50),
(6014, '欧鹏', '地辟星 摩云金翅','军情远哨', 6001,'2017-11-4', 2000, 8000, 50),
(6015, '邓飞', '地阖星 火眼狻猊','军情远哨', 6001,'2017-11-5', 2000, 8000, 50),
(6016, '燕顺', '地强星 锦毛虎','军情远哨', 6001,'2018-10-16', 2000, 8000, 50),
(6017, '马麟', '地明星 铁笛仙','军情远哨', 6001,'2018-10-16', 2000, 8000, 50),
(6018, '陈达', '地周星 跳涧虎','军情远哨', 6001,'2018-9-20', 2000, 8000, 50),
(6019, '杨春', '地隐星 白花蛇','军情远哨', 6001,'2018-9-20', 2000, 8000, 50),
(6020, '杨林', '地暗星 锦豹子','军情远哨', 6001,'2018-9-20', 2000, 8000, 50),
(6021, '周通', '地空星 小霸王','军情远哨', 6001,'2018-5-30', 2000, 8000, 50),


(7001, '孙新', '地数星 小尉迟','东山迎宾', 3,'2014-8-6', 8000, 9000, 70),
(7002, '顾大嫂', '地阴星 母大虫','东山迎宾', 3,'2014-8-6', 8000, 9000, 70),
(7003, '张青', '地刑星 菜园子','西山迎宾', 3,'2015-6-12', 8000, 9000, 70),
(7004, '孙儿娘', '地壮星 母夜叉','西山迎宾', 3,'2015-6-12', 8000, 9000, 70),
(7005, '朱贵', '地囚星 旱地忽律','南山迎宾', 3,'2013-5-25', 8000, 9000, 70),
(7006, '杜兴', '地全星 鬼睑儿','南山迎宾', 3,'2013-5-26', 8000, 9000, 70),
(7007, '李立', '地奴星 催命判官','北山迎宾', 3,'2015-4-18', 8000, 9000, 70),
(7008, '王定六', '地劣星 活闪婆','北山迎宾', 3,'2015-4-18', 8000, 9000, 70),

(8001, '蔡福', '地平星 铁臂膊','刑罚堂主', 3,'2013-7-10', 12000, 2000, 80),
(8002, '蔡庆', '地损星 一枝花','刑罚执法', 3,'2013-7-10', 12000, 2000, 80);

select * from employee;

CREATE TABLE `department` (
    `deptno` int auto_increment primary key comment '部门编号',
    `deptname` varchar(50) comment '部门名称',
    `location` varchar(100) comment '部门地点'
);

INSERT INTO department VALUES
(10, '中央', '梁山本部'),
(20, '近卫', '梁山本部'),
(21, '军委1部', '梁山本部'),
(22, '军委2部', '梁山本部'),
(23, '军委3部', '梁山本部'),
(30, '财务部', '梁山本部'),
(40, '参谋部', '梁山本部'),
(50, '后勤部', '梁山本部'),
(60, '军情部', '梁山本部'),
(70, '迎宾部', '梁山本部'),
(80, '刑罚部', '梁山本部');   
```

