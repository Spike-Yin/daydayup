# Springboot 是什么

springboot 是一个高度简化的spring的程序

* ==约定大于配置==

意思是对springboot而言, 代码的规范比配置重要

* 在springboot里,几乎所有的配置都是自动设置,这大大减少了开发难度和时间

任何一个程序的发展都是从繁琐走向简单, 越先进的程序总是**约定>配置**

# Springboot Starter

Starter: 启动器

springboot将研发中应用的各个场景都抽取了出来,做成了一个个starter

* starter整合了该场景下各种可能用到的依赖
* 用户只需要在maven中引入starter依赖,springboot就可以自动扫描需要加载的信息并启动相应配置
* 自动配置, 所有的starter都遵循约定成俗的默认配置

以web-stater为例(spring-boot-starter-web)

* 它能够提供web开发场景中所需要的一切依赖

==pom.xml==: 想要应用依赖都要在这里写入

以下是maven中需要引入的依赖(如果是用maven启动的话)

```java
<!--SpringBoot父项目依赖管理-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.5</version>
        <relativePath/>
    </parent>
    ....
    <dependencies>
        <!--导入 spring-boot-starter-web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        ...
    </dependencies>
```

通过mvn命令查看web启动器的依赖数

```java
mvn dependency:tree
```

*  Spring Boot 导入了 springframework、logging、jackson 以及 Tomcat 等依赖

在pom.xml的配置中,web等依赖并没有写明版本,但依赖里都有具体的版本信息.

* 这些版本信息都是由spring-boot-stater-parent(版本仲裁中心)统一控制

```java
<!--SpringBoot父项目依赖管理-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.5</version>
    <relativePath/>
</parent>
```

* 啊对,就是上面那个,加入用maven启动需要导入的parent依赖

Spring Boot 项目通过继承 spring-boot-starter-parent 来获得一些合理的默认配置，它主要提供了以下特性：

+ 默认 JDK 版本（Java 8）
+ 默认字符集（UTF-8）
+ 依赖管理功能
+ 资源过滤
+ 默认插件配置
+ ==识别 application.properties 和 application.yml 类型的配置文件==

查看spring-boot-starter-parent的底层代码

* 对着<parent> 按下**ctrl**,点击鼠标左键进入

代码结果略,  大概知道这里面有很多配置的具体信息,比如配置的版本

==这里面还是有需要注意的事项的,对以下在底层代码出现元素重点注意==

+ dependencyManagement ：负责管理依赖；
+ pluginManagement：负责管理插件；
+ properties：负责定义依赖或插件的版本号。

Spring-boot-dependencies 就是通过**上面的三个元素**对一些常用的技术框架的**依赖和插件**进行了**统一的版本管理**

* 比如Activemq, Spring, ==Tomcat==
* 时至今日,我还是不清楚tomcat是个啥玩意儿



## 本章存在疑惑和问题

* springboot为啥能通过在pom.xml文件中写入一串代码就能导入那么多依赖?
* 是maven的自身特性吗?  
* 这些依赖的配置一开始是存在哪里的?
* springboot是基于spring框架优化的简约开发,   是否说明在spring里面这些依赖都是人工导入的?
* 这么多依赖是怎么统合的?
* tomcat是啥 ?

# YAML

Springboot默认使用两种全局的配置文件

* 在遇到默认的配置不符合需求时,就要对配置进行修改,因此需要一种可以修改配置的统一格式

两种配置文件的文件名是固定的

* application.properties
* application.yml /yaml    (后置格式多样,==建议使用yml后置格式==)

yml是一种使用YAML语言编写的文件,他与properties一样,可以在springboot启动时被自动读取,修改自动配置的默认值

## YAML简介

是一种以数据为中心的标记语言,比**XML**和**JSON**更适合做配置文件

想要使用yaml作为属性配置文件,需要将SnakeYAML库添加到==classpath==下

* starter-web和starter两个启动器都对SnakeYAML库做了集成
* 项目只要**引用这两个中随便一个**,springboot会**自动**将库添加到==classpath==下

## YAML语法

语法很EZ:

* 用缩进表示层级关系
* **缩进不能用tab,只能用空格**
* 缩进的空格数不重要,  **主要如果上下元素同级,缩进也要相同  [就和py一样]**
* 大小写敏感

```java
spring:
  profiles: dev
  datasource:
    url: jdbc:mysql://127.0.01/banchengbang_springboot
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
```

由上述配置例子可以看出

* spring是主元素,profiles和datasource是两个子元素,根据缩进
* url,username,password是datasource的子元素

## YAML常用写法

基本上就是常用对三种的专用写法

* 对象:键值对的集合(这应该针对的事html文件中的写法)
* 数组:一组按次序的值(???合着,在给我解释名字是吧,联想的我简直像个小丑)
* 字面量(?): 单个的,不可拆分的值  (说实话,看字面,不懂)

## YAML 字面量写法

* 字面量,例如: 数字,字符串,布尔值,日期等

字面量常用于给对象赋值,懂吧

对象就像上面那个例子一样,那些个元素,就是对象

* YAML写对象,==注意!  一定要在冒号:后面加空格!!!!!!==

```java
name: bianchengbang
```

对字符串有个注意的点

* 就是单引号和双引号分别注释的字符串中, 会有些差别

如果是单引号注释的字符串,  会转义特殊字符 (我还纳闷了半天,就是把原来什么样的符号就以什么样的符号打出来,不会产生什么特殊格式,就比如md的语法)

```java
name: 'zhangsan \n lisi'
```

```
输出结果:
zhangsan \n lisi
```

如果是双引号的特殊字符,就会显示格式

```
name: "zhangsan \n lisi"
```

```
输出结果:
zhangsan 
lisi
```

## YAML 对象写法

普通写法,就是每一个元素一行咯

子元素用缩进区分咯

行内写法:

```
website: {name: bianchengbang,url: www.biancheng.net}
```

用  =={}== 进行包含

==**一定要有空格!!!!!!!!!!!!!!!!!!!**==

## YAML数组写法

用 ==-== 这个来区分对象还是数组

```
pets:
  -dog
  -cat
  -pig
```

行内写法

```
pets: [dog,cat,pig]
```

==[]== 包含数组

## 复合结构写法大包含

```
person:
  name: zhangsan
  age: 30
  pets:
    -dog
    -cat
    -pig
  car:
    name: QQ
  child:
    name: zhangxiaosan
    age: 2
```

看上面多整洁

看起来方便

写的也很EZ

## YAML组织结构

一个yml文件可以由一个或多个文档组成

* 就是把所有配置文件需要更新的配置写在一个或多个yml文档里
* 当然这里让我疑惑地是,多个的yml文件到底是存在哪里的
* yml文件是存在优先级的

如果YAML 文件包含多个文档可以用=="---"==作为分割符

```
---
website:
  name: bianchengbang
  url: www.biancheng.net
---
website: {name: bianchengbang,url: www.biancheng.net}
pets:
  -dog
  -cat
  -pig
---
pets: [dog,cat,pig]
name: "zhangsan \n lisi"
---
name: 'zhangsan \n lisi'
```



## 本章存在的疑惑和问题

* yml语法还是很简单的, 不会写的问题是纯粹的不熟练.
* 对于properties配置文件的语法还是需要重视的, properties的写法应该才是最被大多数人习惯的
* 这是根据下面的学习经验总结而来的







# HelloWorld的建立

因为下面的配置绑定用的都是helloworld的例子

然后又各种搞不懂,这里我要重新写一遍helloworld

下面我们开始







# Spring Boot配置文件

## ==Java Bean!==

* 有关java bean的内容需要到MVC课程里才能具体学习                                            

这里只能简单介绍一下(说实话,只会用,真不懂)

* javabean是一个类
* 这个类是具体的和公共的,并且具有无参数的**构造器**,所有属性可以通过getset方法获取
* Javabean的属性是以方法定义的形式出现的
* 下面几点我自己都不懂,就不写了



**@Bean 这个注解实际上是一个添加组件到容器的方法**

**用了@Bean   就意味被他注解的类, 将作为一个组件被添加到容器中去**

**容器就是Component**

**而这个组件,    在springboot启动中,  他会返回组件名(默认情况下组件名是方法名)**

**返回的类型也是组件被定义的类型**

**返回的值也是组件中定义的实例值**

![image-20211023100519550](C:\Users\VLINi\AppData\Roaming\Typora\typora-user-images\image-20211023100519550.png)

**如上图所示,  他会返回User和Pet   这两个是组件的类**



* 是否可以理解, Bean只是一个提供将组件注入容器的一种形式
* 只有这么写, 那些组件才能注入容器
* 容器又是啥?
* 容器是指IOC容器, 但IOC容器又是啥呢?



## @Configuration

真的是缺了不止一点半点儿

**@Configuration  的作用是:   告诉并提醒, 这是一个配置类**

![img](file:///C:\Users\VLINi\AppData\Roaming\Tencent\Users\519371237\QQ\WinTemp\RichOle\~G[]PC2VP`KJVCOU{8YMKVX.png)

@Configuration这个注解写的文件就是放在config文件夹下

这个MyConfig   也是容器中的一个组件, 

## @ConfigurationProperties

springboot实际上提供了两种方式进行配置绑定

* 用@ConfigureProperties
* 用@Value

通过@C... 可以将==全局配置文件==中的配置数据绑定到javabean中

下面就用最简单的helloword为例,演示下怎么通过@C..进行配置

1. 首先就是在yml文件中添加自定义属性

```
person:
  lastName: 张三
  age: 18
  boss: false
  birth: 1990/12/12
  maps: { k1: v1,k2: 12 }
  lists:
    ‐ lisi
    ‐ zhaoliu
  dog:
    name: 迪迪
    age: 5
```

2. 在helloword项目里的main里的java下,可能是source?  教程写的是net.biancheng.www.bean  这是它自己写的helloword, 纯自己创建的springboot里是没有的  

   反正就是在这个下面,要创建一个Person 的实体类

   * 不会不会吧,不会吧,不会吧?

```
package net.biancheng.www.bean;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import java.util.Date;
import java.util.List;
import java.util.Map;
/**
* 将配置文件中配置的每一个属性的值，映射到这个组件中
*
* @ConfigurationProperties：告诉 SpringBoot 将本类中的所有属性和配置文件中相关的配置进行绑定；
* ==prefix = "person"：配置文件中哪个下面的所有属性进行一一映射==
*这句话我不懂是什么意思
*
* 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能；
*/
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
    public Person() {
    }
    public String getLastName() {
        return lastName;
    }
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    public Boolean getBoss() {
        return boss;
    }
    public void setBoss(Boolean boss) {
        this.boss = boss;
    }
    public Date getBirth() {
        return birth;
    }
    public void setBirth(Date birth) {
        this.birth = birth;
    }
    public Map<String, Object> getMaps() {
        return maps;
    }
    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }
    public List<Object> getLists() {
        return lists;
    }
    public void setLists(List<Object> lists) {
        this.lists = lists;
    }
    public Dog getDog() {
        return dog;
    }
    public void setDog(Dog dog) {
        this.dog = dog;
    }
    public Person(String lastName, Integer age, Boolean boss, Date birth, Map<String, Object> maps, List<Object> lists, Dog dog) {
        this.lastName = lastName;
        this.age = age;
        this.boss = boss;
        this.birth = birth;
        this.maps = maps;
        this.lists = lists;
        this.dog = dog;
    }
    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }
}
```

我自己的纯理解就是,只要在这个类前面加@C..  就会将yml中定义的属性与类中的定义的属性一一配对

==perfix是前缀的意思, 这里面那句对perfix的解释我看不懂==

下面的组件意思也没整明白

**这串代码很ez, getset可以用快捷键全部打出, 构造方法差点没认出来,点名批评,要加强复习**

3. 继续在这个目录下,创建一个名为Dog的javaBea

```
package net.biancheng.www.bean;
public class Dog {
    private String name;
    private String age;
    public Dog() {
    }
    public Dog(String name, String age) {
        this.name = name;
        this.age = age;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setAge(String age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public String getAge() {
        return age;
    }
}
```

从上面我们可以一窥javabean的写法

* 这里面又两个私有字段,两个构造方法,剩下全是getset

4. 修改HelloController的代码,  使其在浏览器中展示配置文件中的各个属性值

```
package net.biancheng.www.controller;
import net.biancheng.www.bean.Person;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
@Controller
public class HelloController {
    @Autowired
    private Person person;
    @ResponseBody
    @RequestMapping("/hello")
    public Person hello() {
        return person;
    }
}
```

这个hellocontroller在哪里, 要看上一章,我写了一遍helloword的流程

* 这个controller还是在java下那个大目录下
* 是要自己写的,包括那个XXXXApplication也是自己写的(如果是用maven打开的话)
* 用springinitial 就不用自己写,它自己配好了

这里是可以不用那么复杂写@ResponseBody

简化的方法还是看上一章

5. 重启项目,在浏览器输入:**localhost:8081/hello**

浏览器会自动输出javabean里 方法,加上yml文件中写的属性

## @Value

@Value适用于对读取配置文件中的某一个配置事,可以通过@Value注解获取

修改Person类中的代码,使用@Value注解做配置绑定

```
@Component
public class Person {
    @Value("${person.lastName}")
    private String lastName;
    @Value("${person.age}")
    private Integer age;
    @Value("${person.boss}")
    private Boolean boss;
    @Value("${person.birth}")
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
    
```

在@ConfigureProperties中重复的代码全部被我删去

* 这里很值得注意就是@Value后面的那串特殊格式

* 暂时我也不是很懂什么意思,当我写完那个helloworld我再来补完它

***

***

***

***

***

***

## 两者的对比

1. **使用的位置不同**

* @C.. 标注在javabean的类名上;
* @Value:标注在javabean的属性上.

2. **功能不同**

* @C.. :用于**批量绑定**配置文件中的配置
* @V..: 用于**一个一个的指定**需要绑定的配置

3. 松散绑定的支持不同

* 我看不懂,不写

4. SqEL支持不同

* 与3同理

5. 复杂类型封装

* @C..:支持所有类型数据的封装
* @V...: 只支持基本数据类型的封装,例如字符串,布尔值,整数等类型

6. 应用场景不同

* 就是整体和单个的优劣呗,不写了

## PropertySource

就是如果所有的配置都集中yml或properties中, 这个配置文件有可能会十分庞大, 难以维护  

所以我们会将与Springboot无关的配置(如自定义配置)提取出来,写在一个单独的配置文件中  

* 并在对应的javabean上使用@PropertySource 注解指向盖配置文件
* 就是分离那些可以分离的配置,单独写一个配置文件,用一个注释指向用到它的javabean

1. 还是以helloworld为例子, 将那些自定义配置剥离出来, 在yml一样的位置里写一个properties==(yml应该也行)[注解的名字应该会不一样]==

   如:  Person.properties    (用在person这个Javabean的配置属性)

```
person.last-name=李四
person.age=12
person.birth=2000/12/15
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
person.dog.age=2
```

properties  就用properties 的语法写咯

2. 然后就是在Person这个javabean用@PropertySource注解指向person.properties

```
@PropertySource(value = "classpath:person.properties")//指向对应的配置文件
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String, Object>
```

老样子,那些个重复代码我就删掉了,这些就够了

@PropertSource 就是在最前面用的,  ==指向的这个classpath的语法学一下==

* 啧啧,不会啊,超

3. 启动浏览器看一下:   **localhost:8081/hello**   

* 成功就是自动匹配了呗,还能咋地

# 导入Spring配置

在Springboot里,是不包含spring文件的,即使手动添加spring配置文件到项目中,也不会被识别

* 意思是springboot需要使用特别的方式才能导入spring配置

springboot提供两种方式来导入spring设置

* 使用@ImportResource注解加载spring配置文件
* 使用全注解加载spring配置

## @ImportResource导入spring配置文件

在主启动类上用@ImportResource注解

* 主启动类就是java下,直接创建文件包即可
* net.biancheng.www.service**( 这个包不用我多数了吧)** 包下创建一个名为 PersonService 的==接口==，代码如下。

```java
package net.biancheng.www.service;

import net.biancheng.www.bean.Person;

public interface PersonService {
    public Person getPersonInfo();
}
```

那个包好像就是自己创建的包,接口是在包建好后再建

2. 在 net.biancheng.www.service.impl 包下创建一个名为 PersonServiceImpl 的类，并实现 PersonService 接口，代码如下。

```java
package net.biancheng.www.service.impl;

import net.biancheng.www.bean.Person;
import net.biancheng.www.service.PersonService;
import org.springframework.beans.factory.annotation.Autowired;

public class PersonServiceImpl implements PersonService {
    @Autowired
    private Person person;
    @Override
    public Person getPersonInfo() {
        return person;
    }
}
```

那个包其实就和第一步的那个包一样

这个接口里有一个覆盖方法, 覆盖就是第一个接口的方法,getPersonInfo()

```java
public Person getPersonInfo(){
             return "person";
}
```

很ez的一个java性质

只要定义了类,就可以当做前置用

**这个前置叫啥来着?**

3. 在该项目的 resources 下添加一个名为 beans.xml 的 Spring 配置文件，配置代码如下。
   * **啊对,一个xml文件, 典型的spring配置文件,建议好好学习MVC**

```javascript
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://www.springframework.org/schema/beans 
           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="persionService" class="net.biancheng.www.service.impl.PersonServiceImpl"></bean>
<beans>
```

4. 修改项目的单元测试类 HelloworldApplicationTests ，==校验 IOC 容器==是否已经 **personService**，代码如下。

````
package net.biancheng.www;

import net.biancheng.www.bean.Person;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
@SpringBootTest
class HelloworldApplicationTests {
    @Autowired
    Person person;
    //IOC 容器
    @Autowired
    ApplicationContext ioc;
    @Test
    public void testHelloService() {
        //校验 IOC 容器中是否包含组件 personService
        boolean b = ioc.containsBean("personService");
        if (b) {
            System.out.println("personService 已经添加到 IOC 容器中");
        } else {
            System.out.println("personService 没添加到 IOC 容器中");
        }
    }
 //这个test应该是test的原生代码, 上下文加载,输出person方法?
    @Test
    void contextLoads() {
        System.out.println(person);
    }
}
````

tests应该就是在test文件夹里,==这个测试类的作用是用来改写spring配置的吗?==

* **IOC容器又是个什么鸡毛玩意儿**
* **personService 又是一个什么状态?**
* ==所以为啥要写两个接口?????==

5. 运行单元测试代码

* 结果就是personService已经添加到IOC容器

6. 在主启动程序类使用@ImportResource 注解,  将那个beans.xml的spring配置文件加载到项目中

* 就是@ImportResource 加个classpath

```
package net.biancheng.www;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;
//将 beans.xml 加载到项目中
@ImportResource(locations = {"classpath:/beans.xml"})
@SpringBootApplication
public class HelloworldApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloworldApplication.class, args);
    }
}
```

* ==//将 beans.xml 加载到项目中
  @ImportResource(locations = {"classpath:/beans.xml"})==

7. 再次执行测试代码

* **personService已经添加到IOC容器中**
* ==所以为啥要检测两次添加到IOC容器==

这就是@ImportResouce的注解使用方法,写两个接口,检测两次IOC

整一个xml配置文件,两个接口的名字不一样, 以及test的写一个检测IOC的方法



**==这个方法是真的烦哈!!!  建议是用全注解方式加载!!==**

**==好吧,  全注解方式依然需要两个接口,写一个test的IOC容器方法==**

## 全注解方式加载Spring配置

Spring Boot 推荐我们使用全注解的方式加载 Spring 配置，其实现方式如下：

1. 使用 @Configuration 注解定义配置类，替换 Spring 的配置文件；
2. 配置类内部可以包含有一个或多个被 @Bean 注解的方法，这些方法会被 AnnotationConfigApplicationContext 或 AnnotationConfigWebApplicationContext 类扫描，构建 bean 定义（相当于 Spring 配置文件中的<bean></bean>标签），方法的返回值会以组件的形式添加到容器中，组件的 id 就是方法名。

**以 helloworld 为例，删除主启动类的 @ImportResource 注解，在 net.biancheng.www.config 包下添加一个名为 MyAppConfig 的配置类，代码如下。**

```java
package net.biancheng.www.config;
import net.biancheng.www.service.PersonService;
import net.biancheng.www.service.impl.PersonServiceImpl;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
/**
* @Configuration 注解用于定义一个配置类，相当于 Spring 的配置文件
* 配置类中包含一个或多个被 @Bean 注解的方法，该方法相当于 Spring 配置文件中的 <bean> 标签定义的组件。
*/
@Configuration
public class MyAppConfig {
    /**
     * 与 <bean id="personService" class="PersonServiceImpl"></bean> 等价
     * 该方法返回值以组件的形式添加到容器中
     * 方法名是组件 id（相当于 <bean> 标签的属性 id)
     */
    @Bean
    public PersonService personService() {
        System.out.println("在容器中添加了一个组件:peronService");
        return new PersonServiceImpl();
    }
}

```

执行测试代码

* 会显示两行字
* 在容器中添加了一个组件:personService
* personService 已经添加到IOC容器中



# Springboot Profile

在实际的开发中,一个项目通常会存在多个环境

* 比如:开发环境,测试环境和生产环境
* 不同的环境配置也不同,  比如开发环境就用的开发数据库,测试用的就是测试的

profile为在不同环境下使用不同的配置提供了支持

* 我们可以通过激活,指定参数等, 实现快速切换环境

## 多profile文件方式

springboot的配置文件只有两种形式: .properties和.yml文件

* 这两种文件可以通过命名来区分不同环境的配置

```
application-{profile}.properties/yml
```

这里的profile    就是指各个环境的名称或简称, 如dev,test,prod等

### properties配置

在helloworld 的src/main/resources下添加4个配置文件

* **啊对,就是4个环境的配置文件**
* application.properties: ==主配置文件==
* application-dev.properties: 开发环境配置文件
* application-test.properties: 测试环境配置文件
* application-prod.properties: 生产环境配置文件



在application.properties文件中,指定的默认服务器端口号为8080

在主配置文件中通过以下的一行代码**运用指定环境端口**

```
#默认端口号
server.port=8080
#激活指定的profile
spring.profiles.active=prod
```

spring.profiles.active=prod

意思就是用生产环境的端口

在application-dev.properties中, 配置如下

```
# 开发环境
server.port=8081
```

在 application-test.properties 中，指定测试环境端口号为 8082

```
# 测试环境
server.port=8082
```

在 application-prod.properties 中，指定生产环境端口号为 8083

```
# 生产环境
server.port=8083
```

运行后,显示代码结果

```java
The following profiles are active:prod
Tomcat initialized with prot(s):8083(http)
```

指定的生产环境生效了,就是8083

### yml配置

与properties一样,也可以添加四个配置文件

在application.yml文件中这么写

```
#默认配置
server:
  port: 8080
#切换配置
spring:
  profiles:
    active: dev #激活开发环境配置
```

在 application-dev.yml 中指定开发环境端口号为 8081，配置如下。

```
#开发环境
server:
  port: 8081
```

下面两个配置文件都一样,不写了

运行结果, 控制台输出与properties输出一样,tomcat initialized port(s):8083

## 多profile文档块模式

同时在yml文件中,  可以只写一个yml文件, 用==**---**== 进行分隔

就是配置多个文档块,以下是示例代码

```
#默认配置
server:
  port: 8080
#切换配置
spring:
  profiles:
    active: test
---
#开发环境
server:
  port: 8081
spring:
  config:
    activate:
      on-profile: dev
---
#测试环境
server:
  port: 8082
spring:
  config:
    activate:
      on-profile: test
---
#生产环境
server:
  port: 8083
spring:
  config:
    activate:
      on-profile: prod
```

运行后, 结果显示和上面一样

## 激活profile

除了在配置文件中用代码==激活指定的profile==, 还有两种方式激活profile

* 命令行激活
* 虚拟机参数激活

### 命令行激活

在把springboot项目打包成jar文件, 在通过命令行运行时, 配置命令行参数, 激活指定的profile

还是以helloworld为例,执行以下mvn命令将项目打包

```
mvn clean package
```

**这个mvn命令是啥, 第一章讲启动器就提到了, mvn命令行到底怎么用?**

打包结果,显示 打成包哈,  这层代码就不写了



打开命令行窗口,跳转到jar文件所在目录,执行以下命令,启动该项目,并激发开发环境(dev)的profile

```
java -jar helloworld-0.0.1-SNAPSHOT.jar  --spring.profiles.active=dev
```

以上命令中，--spring.profiles.active=dev 为激活开发环境（dev）Profile 的命令行参数。

执行,显示结果

### 虚拟机参数激活

在springboot 项目运行时,**指定虚拟机参数**激活指定的profile

以 helloworld 为例，将该项目打包成 JAR 文件后，打开命令行窗口跳转到 JAR 所在目录，执行以下命令，激活生产环境（prod）Profile。

```
java -Dspring.profiles.active=prod -jar helloworld-0.0.1-SNAPSHOT.jar
```

以上命令中，-Dspring.profiles.active=prod 为激活生产环境（prod）Profile 的虚拟机参数。

* ==可说是虚拟机?  这不还是在命令行启动吗? 是我理解有问题?==



# Springboot默认配置文件

就是---不是只能存在一个properties或yml文件

## 默认配置文件

Spring Boot 可以存在多个 application.properties 或 apllication.yml。

Spring Boot 启动时会扫描以下 5 个位置的 application.properties 或 apllication.yml 文件，并将它们作为 Spring boot 的默认配置文件。

1. file:./config/
2. file:./config/*/
3. file:./
4. classpath:/config/
5. classpath:/

file: 指当前项目根目录;   classpath: 指当前的项目的类路径, 即resouces目录

根目录就是与pon.xml一个位置, classpath就是resouces目录

上述的文件排序,其实也是**优先级排序**,   位于**相同位置的properties的优先级高于yml**

所有位置的文件都会被加载,**高优先级就会覆盖低优先级**

## 示例

在根目录下, 就是在pom.xml上创建一个application.yml

在resources下创建一个application.yml

在resources下创建一个config目录,在里面创建一个application.yml

项目根路径下的yml配置如下:

```
#项目根目录下
#上下文路径为 /abc
server:
  servlet:
    context-path: /abc
```

* context: 上下文, 所以==上下文路径是个啥玩意儿???==

在config文件里的yml配置如下:

```
#类路径下的 config 目录下
#端口号为8084
#上下文路径为 /helloWorld
server:
  port: 8084
  servlet:
    context-path: /helloworld
```

在resources下的yml配置如下:

```
#默认配置
server:
  port: 8080
```



在controller包下创建一个名为MyController的类,代码如下:

```
package net.biancheng.www.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {
    @ResponseBody
    @RequestMapping("/test")
    public String hello() {
        return "hello Spring Boot!";
    }
}
```

==那些个import 就是添加了注解自动添加的==

启动,输出为:  Tomcat:8084(http) with context path: /abc

这里分析一波为什么是这样显示的

+ 该项目中存在多个默认配置文件，其中根目录下 /config 目录下的配置文件优先级最高，因此项目的上下文路径为 “/abc”；
+ ==但为啥没有config目录捏==?
+ 类路径（classpath）下 config 目录下的配置文件优先级高于类路径下的配置文件，因此该项目的端口号为 “8084”；
+ 以上所有配置项形成互补，所以访问路径为“http://localhost:8084/abc”。



# Springboot外部配置文件

除了项目内部的配置文件,springboot 本身还是可以加载项目外的配置文件

我们可以通过以下2个参数,指定外部配置文件路径

* spring.config.location
* spring.config.additional-location

## spring.config.location

讲项目打包成jar文件,在命令行命令中启动,使用命令行参数

```
spring.config.location
```

 指定外部配置文件的路径

```
java -jar {JAR}  --spring.config.location={外部配置文件全路径}
```

* 值得注意的,使用该参数指定配置文件后,会使项目默认配置文件(就是application.properties或application.yml)失效
* springboot将只加载指定的外部配置文件

### 示例

在本地目录下,如d盘:  D:\myConfig , 创建一个my-application.yml.

配置如下:

```
#指定配置文件
server:
  port: 8088
```

执行以下mvn命令,将springbootdemo 项目打包成JAR

```
mvn clean package
```

打包成功

代码显示结果为:building jar: ...

打开命令行窗口输入命令

```
java -jar springbootdemo-0.0.1-SNAPSHOT.jar  --spring.config.location=D:\myConfig\my-application.yml
```

项目成功运行后结果显示: 端口8088

​                                             8088(http) with  context path' '

从控制台输出可以看出：

+ 服务器端口号从“8084”被修改为“8088”，表示外部配置文件已生效；
+ 上下文路径则从“/abc”被修改为默认值（‘ ’），表示项目内部的默认配置文件已失效。



## spring.config.additional-location

我们还能用一个命令参数**执行加载外部配置文件**

````
java -jar {JAR}  --spring.config.additional-location={外部配置文件全路径}
````

但与上面的spring.config.location 不同, spring.config.additional-location不会使项目默认的配置文件失效

* 使用该命令行参数添加的外部配置文件会与默认的配置文件共同生效,形成互补配置
* 外部配置的优先级还是最高, 比所有的默认配置文件的优先级都高

### 示例2

和示例1的前几个步骤一样,打包成jar包, 用以下命令启动项目

```
java -jar springbootdemo-0.0.1-SNAPSHOT.jar  --spring.config.additional-location=D:\myConfig\my-application.yml
```

注意：Maven 对项目进行打包时，位于**项目根目录下的配置文件是无法被打包进项目的 JAR 包的**，因此位于根目录下的默认配置文件无法在 JAR 中生效，即该项目将**只加载指定的外部配置文件和项目类路径（classpath）下的默认配置文件**，它们的加载优先级顺序为：

1. spring.config.additional-location 指定的外部配置文件 my-application.yml
2. classpath:/config/application.yml
3. classpath:/application.yml

* 说实话搞不懂, 为什么到这里maven对项目进行打包时, 根目录下的配置文件就无法被打包进jar包了
* 这里就是外部配置优先级高于内部呗



通过上面的示例，我们看到将 Spring Boot 项目打包后，然后在命令行启动命令中添加 spring.config.additional-location 参数指定外部配置文件，会导致项目根目录下的配置文件无法被加载，我们可以通过以下 3 种方式解决这个问题。

+ 在 IDEA 的运行配置（Run/Debug Configuration）中，添加虚拟机参数 -Dspring.config.additional-location=D:\myConfig\my-application.yml，指定外部配置文件；

+ 在 IDEA 的运行配置（Run/Debug Configuration）中，添加程序运行参数 --spring.config.additional-location=D:\myConfig\my-application.yml，指定外部配置文件；
+ 在主启动类中调用 System.setProperty（）方法添加系统属性 spring.config.additional-location，指定外部配置文件。

==也就是说在先打包后, 在命令行输入这个additional后, 项目根目录下的配置文件才不能被读取==



**好像不对,jar包雀食是不把pom.xml上的配置文件读取的**

* 找机会弄懂

也就是说其实maven配置没问题, 是命令行参数打了后才这样的



# springboot配置配置文件时的加载顺序

Spring Boot 不仅可以通过配置文件进行配置，还可以通过环境变量、命令行参数等多种形式进行配置。这些配置都可以让开发人员在不修改任何代码的前提下，直接将一套 Spring Boot 应用程序在不同的环境中运行。

* 就是说不仅可以通过配置文件进行配置, 还可以通过别的方式进行配置
* 配置形式是多样的

## springboot 配置优先级

以下是常用的 Spring Boot 配置形式及其加载顺序（优先级由高到低）：

1. 命令行参数
2. 来自 java:comp/env 的 JNDI 属性
3. Java 系统属性（System.getProperties()）
4. 操作系统环境变量
5. RandomValuePropertySource 配置的 random.* 属性值
6. 配置文件（YAML 文件、Properties 文件）
7. @Configuration 注解类上的 @PropertySource 指定的配置文件
8. 通过 SpringApplication.setDefaultProperties 指定的默认属性

以上所有形式的配置都会被加载，当存在相同配置内容时，高优先级的配置会覆盖低优先级的配置；存在不同的配置内容时，高优先级和低优先级的配置内容取并集，共同生效，形成互补配置。



## 命令行参数

springboot中所有的配置,都可以通过命令行参数指定,其配置形式如下.

```
java -jar {Jar文件名} --{参数1}={参数值1} --{参数2}={参数值2}
```

### 示例1

在springboot 项目启动时,使用以下命令

```
java -jar springbootdemo-0.0.1-SNAPSHOT.jar --server.port=8081 --server.servlet.context-path=/bcb
```

* 所以这个context path: 上下文路径, 实际上就是项目的访问路径



## 配置文件

springboot启动, 加载jar包外, 再加载jar包内, 包内外都以config 目录下的配置文件优先

同一位置下,properties优先级高于yml文件



1. 先加载 JAR 包外的配置文件，再加载 JAR 包内的配置文件；
2. 先加载 config 目录内的配置文件，再加载 config 目录外的配置文件；
3. 先加载 config 子目录下的配置文件，再加载 config 目录下的配置文件；
4. 先加载 appliction-{profile}.properties/yml，再加载 application.properties/yml；
5. 先加载 .properties 文件，再加载 .yml 文件。



### 示例2

不说了, 就是在各个地方创建各种配置, 生产, 测试, 开发, 默认

看符不符合上面的规则呗

符合, 啊对, 符合



# springboot 自动配置原理

这些在配置文件中输入的配置属性, 到底是用什么方式导入了项目中

这就要追究到自动配置原理.

## spring factories机制

* springboot自动配置是基于spring factories机制实现的
* spring factories机制是springboot中的一种服务发现机制
* 这种扩展机制与javaSPI 机制 十分相似.
* ==springboot会自动扫描所有JAR包类路径下META-INF/spring.factories文件==
* 读取其中内容,进行实例化, 这种机制也是springboot starter的基础

### spring factories

* spring factories 文件本质与properties文件相似
* 它包含了一组或多组键值对(key=value)
* key的取值  是 **接口的完全限定名**     ??????????
* value的取值  为  **接口实现类的完全限定名**   ???????????
* 一个接口可以设置多个实现类, 不同实现类用","隔开

```
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnBeanCondition,\
org.springframework.boot.autoconfigure.condition.OnClassCondition,\
org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition
```

这个  \   就是换行  用来方便阅读的

### spring factories 实现原理

 **spring-core 包里定义了 SpringFactoriesLoader 类，这个类会扫描所有 Jar 包类路径下的 META-INF/spring.factories 文件**，并获取指定接口的配置。在 SpringFactoriesLoader 类中定义了两个对外的方法

| 返回值       | 方法                                                         | 描述                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| <T> List<T>  | loadFactories(Class<T> factoryType, @Nullable ClassLoader classLoader) | 静态方法；<br/>根据接口获取其实现类的实例；<br/>该方法返回的是实现类对象列表。 |
| List<String> | loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) | 公共静态方法；<br/>根据接口获取其实现类的名称；<br/>该方法返回的是实现类的类名的列表 |

以上两个方法的关键都是从指定的 ClassLoader 中获取 spring.factories 文件，并解析得到类名列表，具体代码如下。

* loadFactories() 方法能够获取指定接口的实现类对象.
* loadFactoryNames() 方法能够根据接口获取其实现类类名的集合.
* loadSpringFactories() 方法能够读取该项目中所有 Jar 包类路径下 META-INF/spring.factories 文件的配置内容，并以 Map 集合的形式返回.



## 自动配置加载

Spring Boot 自动化配置基于 Spring Factories 机制实现的，在 spring-boot-autoconfigure-xxx.jar 类路径下的 META-INF/spring.factories 中设置了 Spring Boot 自动配置的内容

* 自己找代码  ==这玩意儿总而言之就是用factories的机制, 将这些xxAutoConfiguration  实例化,  并作为组件加入容器中==



## springboot application注解

所有 Spring Boot 项目的主启动程序类上都使用了一个 @SpringBootApplication 注解，该注解是 Spring Boot 中最重要的注解之一 ，也是 Spring Boot 实现自动化配置的关键。 

* ==啊对, 应该就是那个 application, 就是那个run的那个文件==

***

@SpringBootApplication 是一个组合元注解，其主要包含两个注解：@SpringBootConfiguration 和 @EnableAutoConfiguration，其中 @EnableAutoConfiguration 注解是 SpringBoot 自动化配置的核心所在。

### @EnableAutoConfiguration注解

**@EnableAutoConfiguration 注解用于开启 Spring Boot 的自动配置功能**， 它使用 Spring 框架提供的 @Import 注解通过 **AutoConfigurationImportSelector类（选择器）**给容器中导入自动配置组件。

### AutoConfigurationImportSelector类

==这里是一点都没搞懂, 我暂且蒙古, 下面他介绍的三个方法搞懂了再写==

## 自动配置的生效和修改

spring.factories 文件中的所有自动配置类（xxxAutoConfiguration），都是必须在一定的条件下才会作为组件添加到容器中，配置的内容才会生效。这些限制条件在 Spring Boot 中以 @Conditional 派生注解的形式体现，如下表。

* ==没看懂,==

### ServeletWebSeverFactoryAutoConfiguration

????????????

### ServerProperties

????????????

两个都没看懂



# Spring Boot统一日志框架

日志: 用来记录运行情况和定位线上问题.

* java提供了多种日志框架: JCL, SLF4J,Jboss-logging,jUL,log4j, log4j2,logback等



## 日志框架选择

有两种日志框架

* 日志门面(日志抽象层)
* 日志实现

| 日志分类               | 描述                                                         | 举例                                                         |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 日志门面（日志抽象层） | 为 Java 日志访问提供一套标准和规范的 API 框架，其主要意义在于提供接口。 | JCL（Jakarta Commons Logging）、SLF4j（Simple Logging Facade for Java）、jboss-logging |
| 日志实现               | 日志门面的具体的实现                                         | Log4j、JUL（java.util.logging）、Log4j2、Logback             |

日志: **一个日志门面和一个日志实现组合而成**

springboot用  SLF4J+Logback的组合来搭建日志系统

SLF4J很好用, 也是市场上最流行的日志

Logback是SLF4J的原生实现框架,   用来代替log4j

## SLF4J的使用

在项目开发中

* 记录日志, 不直接调用日志实现层的方法.
* 应该调用日志门面(日志抽象层)的方法.

在使用SLF4J记录日志时,  需要在应用中导入SLF4J及日志实现.

**并在记录日志时,调用SLF4J的方法**

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
// 调用SLF4J

public class HelloWorld {
    public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
       //调用 sl4j 的 info() 方法，而非调用 logback 的方法
        logger.info("Hello World");
    }
}
```

+ Logback 作为 Slf4j 的原生实现框架，应用使用 SLF4J+Logback 的组合记录日志时，只需要引入 SLF4J 和 Logback 的 Jar 包即可；
+ 当应用使用 SLF4J+Log4j 的组合记录日志时，不但需要引入 SLF4J 和 Log4j 的 Jar 包，还必须引入它们之间的适配层（Adaptation layer）slf4j-log4j12.jar，该适配层可谓“上有老下有小”，它既要实现 SLF4J 的方法，还有调用 Log4j 的方法，以达到承上启下的作用；
+ 使用 SLF4J+JUL 记录日志时，与 SLF4J+Log4j 一样，不但需要引入 SLF4J 和 JUL 的对应的 Jar 包，还要引入适配层 slf4j-jdk14.jar。

==每一个日志的实现框架都有自己的配置文件。使用 slf4j 记录日志时，配置文件应该使用日志实现框架（例如 logback、log4j 和 JUL 等等）自己本身的配置文件。==

## 统一日志框架(通用)

一个完整的应用会依赖于多种不同的框架, 记录日志使用的日志框架也不相同

* 那么如何统一日志框架的使用呢?

大概需要三步

1. 排除应用中的原来的日志框架；
2. 引入替换包替换被排除的日志框架；
3. 导入 SLF4J 实现。

==SLF4J会提供一个替换包替换原来的日志框架==

## 统一日志框架(Spring Boot)

统一日志框架的使用一共分为 3 步，Soring Boot 作为一款优秀的开箱即用的框架，已经为用户完成了其中 2 步：

* 引入替换包和导入 SLF4J 实现。

* Spring Boot 的核心启动器 spring-boot-starter 引入了 spring-boot-starter-logging

spring-boot-starter-logging 的 Maven 依赖不但引入了 logback-classic （包含了日志框架 SLF4J 的实现），

还引入了 log4j-to-slf4j（log4j 的替换包），jul-to-slf4j （JUL 的替换包），

即 Spring Boot 已经为我们完成了统一日志框架的 3 个步骤中的 2 步。

***

SpringBoot 底层使用 slf4j+logback 的方式记录日志，当我们引入了依赖了其他日志框架的第三方框架（例如 Hibernate）时，只需要把这个框架所依赖的日志框架排除，即可实现日志框架的统一，示例代码如下。

```
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-console</artifactId>
    <version>${activemq.version}</version>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



# Spring-boot日志配置及输出

## 默认配置

Spring Boot 默认使用 SLF4J+Logback 记录日志，并提供了默认配置

常见的日志配置包括日志级别、日志的输入出格式等内容。

### 日志的输出级别分类

当一条日志信息的级别大于或等于配置文件的级别时,就对这条日志进行记录.

常见的日志级别如下

| 序号 | 日志级别 | 说明                                                        |
| :--: | -------- | ----------------------------------------------------------- |
|  1   | trace    | 追踪，指明程序运行轨迹。                                    |
|  2   | debug    | 调试，实际应用中一般将其作为最低级别，而 trace 则很少使用。 |
|  3   | info     | 输出重要的信息，使用较多。                                  |
|  4   | warn     | 警告，使用较多。                                            |
|  5   | error    | 错误信息，使用较多。                                        |

#### 输出格式

* 说实话, 感觉用不到, 这种用不到还看不懂的东西, 暂时不记

## 修改默认日志配置

* 就是可以通过全局配置文件(就是properties和yml文件)修改springboot日志级别和显示格式等默认配置
* 需要和上面的输出格式==一起使用==

## 自定义日志配置

就是上面的修改日志配置, 只是修改个别的配置, 想要修改更多配置或更高级功能,需要日志实现框架自己的配置文件进行配置

* Spring 官方提供了各个日志实现框架所需的配置文件，用户只要将指定的配置文件放置到项目的类路径下即可。

**日志框架的配置文件基本分为两类**

* 普通日志配置文件，即不带 srping 标识的配置文件，例如 logback.xml；
* 带有 spring 表示的日志配置文件，例如 logback-spring.xml。

#### 普通日志配置文件

logback.xml、log4j2.xml 等不带 spring 标识的普通日志配置文件，

放在项目的类路径下后，这些配置文件会跳过 Spring Boot，

直接被日志框架加载。

通过这些配置文件，我们就可以达到自定义日志配置的目的

#### 带有spring标识的日志配置文件

Spring Boot 推荐用户使用 logback-spring.xml、log4j2-spring.xml 等这种带有 spring 标识的配置文件。

这种配置文件被放在项目类路径后，不会直接被日志框架加载，

而是由 Spring Boot 对它们进行解析，这样就可以使用 Spring Boot 的高级功能 Profile，

实现在不同的环境中使用不同的日志配置。

***

上面的几个日志配置实际上是提供示例, 不过yysy, 根本就看不懂, 我这也不提供示例了

***



# Spring-boot-starter-web(web启动器)

IDEA是提供直接用web服务启动的springboot的

这个知道就行

这里提供几个概念

* ==spring MVC==,  是spring提供的一个基于MVC设计模式的轻量级web开发框架
* 它本身是spring框架的一部分
* 是当今最主流的web开发框架之一

## Spring Boot Web 快速开发

Spring Boot 为 Spring MVC 提供了自动配置

并在 Spring MVC 默认功能的基础上添加了以下特性：

***



+ 引入了 ContentNegotiatingViewResolver 和 BeanNameViewResolver（**视图解析器**）
+ 对包括 WebJars 在内的**静态资源的支持**
+ 自动注册 Converter、GenericConverter 和 Formatter （**转换器和格式化器**）
+ 对 HttpMessageConverters 的支持（**Spring MVC 中用于转换 HTTP 请求和响应的消息转换器**）
+ 自动注册 MessageCodesResolver（**用于定义错误代码生成规则**）
+ 支持**对静态首页（index.html）的访问**
+ 自动使用 **ConfigurableWebBindingInitializer**
+ ==所以index.html实际上是静态首页的访问==


只要在 Spring Boot 项目中的 pom.xml 中引入了 spring-boot-starter-web ，即使不进行任何配置，也可以直接使用 Spring MVC 进行 Web 开发。



# Springboot 静态资源映射

在web应用中, 会涉及到大量的**静态资源**, 就比如: JS, CSS ,HTML

Spring MVC在导入静态资源文件时, 需要**配置静态资源的映射**

Springboot已经默认完成了这项工作

springboot提供三种静态资源映射规则:

* WebJars映射  (Jar包, 各种jar形式)
* 默认资源映射
* 静态首页( 欢迎业)映射

## WebJars映射

为让页面美观, web应用会大量使用 JS和CSS , 例如:jQuery, Backbone.js和Bootstrap

通常情况下, 这些web前端资源会被拷贝到Java Web项目的webapp根目录下进行管理

但springboot是以jar包形式进行部署的,   并不存在webapp目录

**所以我们需要用到webjars**

* 他就是以jar形式为web 提供资源文件

webjars将web 前端资源打包成一个个的jar包,  将这些jar包部署到maven中央仓库中进行统一管理

当springboot项目中需要引入web前端资源时, 只需访问webjars官网, 找到需要的pom依赖, 导入项目即可.

* 也就是和启动器依赖dependency的方式一样, 将依赖导入pom.xml文件即可

==所有通过webjars引入的前端资源都放在当前项目类路径(classpath)下的"/META-INF/resources/webjars"==

* **所以这个"META-INF"是个啥玩意儿**

springboot通过MVC的自动配置类: WebMvcAutoConfiguration

为这些webjars前端资源提供默认映射规则

```
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
    } else {
        //WebJars 映射规则
        this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
        this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
            registration.addResourceLocations(this.resourceProperties.getStaticLocations());
            if (this.servletContext != null) {
                ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                registration.addResourceLocations(new Resource[]{resource});
            }
        });
    }
}
```

* 上面是部分源码, 就是写的映射规则的方法, 指定到META-INF  classppath

==通过以上源码可知，WebJars 的映射路径为“/webjars/**”，即所有访问“/webjars/**”的请求，都会去“classpath:/META-INF/resources/webjars/”查找 WebJars 前端资源。==

### 示例1

1. 在 Spring Boot 项目 spring-boot-springmvc-demo1 的 pom.xml 中添加以下依赖，将 jquery 引入到该项目中。

```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.6.0</version>
</dependency>
```

2. Spring Boot 项目中引入的 jquery 的 Jar 包结构如下图。

![WebJars JQUERY](http://c.biancheng.net/uploads/allimg/210726/1555551O8-1.png)

3. 启动 Spring Boot，浏览器访问**“http://localhost:8080/webjars/jquery/3.6.0/jquery.js”**访问 jquery.js，

## 默认静态资源映射

访问项目中的任意资源,springboot会从几个默认路径查找资源文件

以下是优先级依次降低

* classpath:/META-INF/resources/
* classpath:/resources/
* classpath:/static/
* classpath:/public/

这些路径也被称为**静态资源文件夹**

当请求某个静态资源(就是以**".html"**结尾的请求),  springboot就会按照优先级由上往下找

### 示例2

在main下的resources下的static目录中创建一个hello.html

```
<!DOCTYPE html>
<html lang="en">

<head> </head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
<body>
<h1>欢迎您来到编程帮（www.biancheng.net）</h1>
</body>
</html>
```

## 静态首页(欢迎页)映射

静态资源文件夹下的 **index.html**被称为==静态首页或者欢迎页==, 它们会被/**映射

当我们访问"/"或者"/index.html"时, 都会跳转到静态首页(欢迎页)

就是那个在localhost里句尾那个/hello

### 示例3

在springboot的main里的resources的public目录中创建一个index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
</body>
</html>
```



# Springboot定制Spring MVC

Springboot是通过@Configuration(配置类)以JavaBean形式进行相关配置的

springboot对mvc的自动配置可以满足绝大部分需求

但也可以通过自定义配置类(@Configuration)**来实现webmvcconfigurer接口**来定制spring mvc配置

* 就比如: 拦截器, 格式化程序, 视图控制器

==WebMvcConfigurer== 是一个基于java8的接口, 该接口定义了很多 与springmvc相关的方法, 其中大部分方法是default类的

只需要定义一个配置类实现 webMVCconfigurer接口,

* 接口连接的相关方法会定制springmvc的配置

方法略

* 就他妈没看懂过, 不写这方法的文档

***

以下两种形式定制springmvc

* 扩展springmvc
* 全面接管springmvc



## 扩展Spring MVC

***

***

****

****

****



## 全面接管spring mvc

****

****

****

****

****



# springboot国际化

国际化就是软件开发时应该具备支持多种语言和地区的功能

在springboot实现国际化, 只需要三步

* 编写国际化资源(配置)文件
* 使用==ResourcesBundleMessageSource==管理 国际化资源文件
* 在**页面获取国际化内容==(???????????==)**

## 编写国际化资源文件

在 Spring Boot 的类路径下创建国际化资源文件

文件名格式为：基本名_语言代码_国家或地区代码

* 例如: login_en_US.properties
* ​          login_zh_CN.properties。

以 spring-boot-springmvc-demo1为例，在 src/main/resources 下创建一个 i18n 的目录

并在该目录中按照国际化资源文件命名格式分别创建以下三个文件

+ login.properties：无语言设置时生效
+ login_en_US.properties ：英语时生效
+ login_zh_CN.properties：中文时生效

以上三个国际化资源文件创建完成后, idea会自动识别它们, 并转换成如下的模式

![img](http://c.biancheng.net/uploads/allimg/210726/1601333623-0.png)

打开任意一个国际化资源文件, 并切换为Resource Bundle模式, 点击"+"

创建所需的国际化属性

![国际化配置文件](http://c.biancheng.net/uploads/allimg/210726/1601333350-1.png)



***



## 使用ResourcesBundleMessageSource管理国际化资源文件

springboot 已经对ResourceBundleMessageSource 提供了默认的自动配置

springboot**通过 MessageSourceAutoConfigura对**

**ResourceBundleMessageSource提供了默认配置**

本来是提供了源码的, 但, 

* 提供源码又看不懂有啥用呢

***

* springboot 将 MessageSourceProperties 以组件的形式添加到容器中
* MessageSourceProperties的**属性和配置文件**中以=="spring.messages"==开头的配置进行了绑定
* springboot从容器中获取MessageSourceProperties组件, 并从中读取国际化资源文件的basename---文件基本名, encoding(编码)信息,   并将他们封装
* springboot将ResourceBundleMessageSource以组件的形式添加到容器中, 进而实现对国际化资源文件的管理

查看MessageSourceProperties类

提供部分源码如下

: 略

***

+ MessageSourceProperties 为 basename、encoding 等属性提供了默认值；
+ **basename** 表示国际化资源文件的基本名，其==默认取值为“message”==，即 Spring Boot 默认会获取类路径下的 message.properties 以及 message_XXX.properties 作为国际化资源文件；
+ 在 application.porperties/yml 等配置文件中，使用==配置参数“spring.messages.basename”==即可**重新指定国际化资源文件的基本名**。

springboot已经对国际化资源文件的管理提供了默认自动配置

这里只需要在springboot的全局配置文件中, 使用配置参数"spring.message.basename"指定自定义的国际资源文件的基本名即可

```java
spring.messages.basename=i18n.login
```

## 获取国际化内容

由于页面使用的Tymeleaf模板引擎

因此我们可以通过表达式#(...)获取国际化内容

以spring-boot-adminex为例, 在login.html中获取国际化内容

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <meta name="description" content="">
    <meta name="author" content="ThemeBucket">
    <link rel="shortcut icon" href="#" type="image/png">
    <title>Login</title>
    <!--将js css 等静态资源的引用修改为 绝对路径-->
    <link href="css/style.css" th:href="@{/css/style.css}" rel="stylesheet">
    <link href="css/style-responsive.css" th:href="@{/css/style-responsive.css}" rel="stylesheet">
    
    <!-- HTML5 shim and Respond.js IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
    
    <script src="js/html5shiv.js" th:src="@{/js/html5shiv.js}"></script>
    <script src="js/respond.min.js" th:src="@{/js/respond.min.js}"></script>
    <![endif]-->
</head>

<body class="login-body">
<div class="container">
   
   <form class="form-signin" th:action="@{/user/login}" method="post">
        <div class="form-signin-heading text-center">
            <h1 class="sign-title" th:text="#{login.btn}">Sign In</h1>
            <img src="/images/login-logo.png" th:src="@{/images/login-logo.png}" alt=""/>
        </div>
        <div class="login-wrap">
            <p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
            <input type="text" class="form-control" name="username" placeholder="User ID" autofocus
                   th:placeholder="#{login.username}"/>
            <input type="password" class="form-control" name="password" placeholder="Password"
                   th:placeholder="#{login.password}"/>
            <label class="checkbox">
                <input type="checkbox" value="remember-me" th:text="#{login.remember}">
                <span class="pull-right">
                    <a data-toggle="modal" href="#myModal" th:text="#{login.forgot}"> </a>
                </span>
            </label>
            <button class="btn btn-lg btn-login btn-block" type="submit">
                <i class="fa fa-check"></i>
            </button>
            
            <div class="registration">
                <!--Thymeleaf 行内写法-->
                [[#{login.not-a-member}]]
                <a class="" href="/registration.html" th:href="@{/registration.html}">
                    [[#{login.signup}]]
                </a>
                <!--thymeleaf 模板引擎的参数用（）代替 ？-->
                <br/>
                <a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>|
                <a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
            </div>
        </div>
        
        <!-- Modal -->
        <div aria-hidden="true" aria-labelledby="myModalLabel" role="dialog" tabindex="-1" id="myModal"
             class="modal fade">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                        <h4 class="modal-title">Forgot Password ?</h4>
                    
                    </div>
                    <div class="modal-body">
                        <p>Enter your e-mail address below to reset your password.</p>
                        <input type="text" name="email" placeholder="Email" autocomplete="off"
                               class="form-control placeholder-no-fix">
                    </div>
                    <div class="modal-footer">
                        <button data-dismiss="modal" class="btn btn-default" type="button">Cancel</button>
                        <button class="btn btn-primary" type="button">Submit</button>
                    </div>
                </div>
            </div>
        </div>
        <!-- modal -->
    </form>
</div>
<!-- Placed js at the end of the document so the pages load faster -->
<!-- Placed js at the end of the document so the pages load faster -->
<script src="js/jquery-1.10.2.min.js" th:src="@{/js/jquery-1.10.2.min.js}"></script>
<script src="js/bootstrap.min.js" th:src="@{/js/bootstrap.min.js}"></script>
<script src="js/modernizr.min.js" th:src="@{/js/modernizr.min.js}"></script>
</body>
</html>
```

## 验`证

启动springboot, 浏览器访问登录页

## 手动切换语言

在登录页(login.html)最下方有两个切换语言的链接, 想要通过点击它们来切换进行国际化的语言

该怎么做?

![国际化切换按钮](http://c.biancheng.net/uploads/allimg/210726/16013322A-4.png)

### 区域信息解析器自动配置

spring MVC 国际化有2个十分重要的对象

* Locale: 区域信息对象
* LocaleResolver: 区域信息解析器, 容器中的组件, 负责获取区域信息对象

==可以通过以上两个对象区域信息的切换, 以达到切换语言的目的==

springboot在WebMvcAutoConfiguration中为区域信息解析器(LocaleResolver)进行了自动配置

源码略

***

+ 该方法**默认向容器中添加了一个区域信息解析器**（LocaleResolver）组件，它会**根据请求头中携带的“Accept-Language”参数**，获取**相应区域信息（Locale）对象**。
+ 该方法上使用了 **@ConditionalOnMissingBean 注解**，其参数 name 的取值为 localeResolver（与该方法注入到容器中的组件名称一致），该注解的含义为：**当容器中不存在名称为 localResolver 组件时，该方法才会生效**。换句话说，当我们**手动向容器中添加一个名为“localeResolver”的组件**时，**Spring Boot 自动配置的区域信息解析器会失效**，而我们定义的区域信息解析器则会生效。

#### 手动切换语言

1. 修改login.html切换语言链接, 在请求中携带国际化区域信息,代码如下

````
<!--thymeleaf 模板引擎的参数用（）代替 ？-->
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>|
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
````

2. 在 net.biancheng.www 下创建一个 component 包，并在该包中创建一个区域信息解析器 MyLocalResolver，代码如下。

```
package net.biancheng.www.componet;

import org.springframework.util.StringUtils;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

//自定义区域信息解析器
public class MyLocalResolver implements LocaleResolver {
    
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        //获取请求中参数
        String l = request.getParameter("l");
        //获取默认的区域信息解析器
        Locale locale = Locale.getDefault();
        //根据请求中的参数重新构造区域信息对象
        if (StringUtils.hasText(l)) {
            String[] s = l.split("_");
            locale = new Locale(s[0], s[1]);
        }
        return locale;
    }
    
    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
    
    }
}
```

3. 在 net.biancheng.www.config 的 MyMvcConfig 中添加以下方法，将自定义的区域信息解析器以组件的形式添加到容器中，代码如下。

```
//将自定义的区域信息解析器以组件的形式添加到容器中
@Bean
public LocaleResolver localeResolver(){
    return new MyLocalResolver();
}
```

4. 启动springboot, 进入浏览器页面, 点击切换语言链接



# springboot 拦截器

拦截器, spring MVC也提供了拦截器功能, 它可以根据URL对请求进行拦截

主要用于登录校检, 权限验证, 乱码解决, 性能监控和异常处理

springboot同样提供了拦截器功能

在 Spring Boot 项目中，使用拦截器功能通常需要以下 3 步：

1. 定义拦截器；
2. 注册拦截器；
3. 指定拦截规则（如果是拦截所有，静态资源也会被拦截）。



## 定义拦截器

在springboot中定义拦截器十分简单, 只需要创建一个拦截器类

并实现HandleInterceptor接口即可

HandleInterceptor 接口中定义以下三个方法

| 返回值类型 | 方法声明                                                     | 描述                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| boolean    | preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) | 该方法在控制器处理请求方法前执行，其返回值表示是否中断后续操作，返回 true 表示继续向下执行，返回 false 表示中断后续操作。 |
| void       | postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) | 该方法在控制器处理请求方法调用之后、解析视图之前执行，可以通过此方法对请求域中的模型和视图做进一步修改。 |
| void       | afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) | 该方法在视图渲染结束后执行，可以通过此方法实现资源清理、记录日志信息等工作。 |

## 示例1

以spring-boot-adminex项目为例,  在net.biancheng.www.componet中创建一个名为LoginInterceptor的拦截器类

* 这是对登录进行拦截

代码略, 什么时候看懂什么时候再写

***

## 注册拦截器

创建一个实现了WebMvcConfigurer 接口的配置类(使用了@Configuration注解的类), 

重写addInterceptors()方法,  

并在该方法中调用registry.addInterceptor()方法

将自定义的拦截器注册到容器中



**在配置类MyMvcConfig中,添加以下方法注册拦截器**

```
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    ......
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor());
    }
```

## 指定拦截规则

在使用 registry.addInterceptor() 方法将拦截器注册到容器中后，我们便可以继续指定拦截器的拦截规则了，代码如下

````
@Slf4j
@Configuration
public class MyConfig implements WebMvcConfigurer {
    ......
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        log.info("注册拦截器");
        registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**") //拦截所有请求，包括静态资源文件
                .excludePathPatterns("/", "/login", "/index.html", "/user/login", "/css/**", "/images/**", "/js/**", "/fonts/**"); //放行登录页，登陆操作，静态资源
    }
}
````

* 这是写在哪里的?  还是那个配置文件里面吗?



在指定拦截器拦截规则时，调用了两个方法，这两个方法的说明如下：

+ addPathPatterns：**该方法用于指定拦截路径**，例如拦截路径为“/**”，表示拦截所有请求，包括对静态资源的请求。
+ excludePathPatterns：该方法**用于排除拦截路径，即指定不需要被拦截器拦截的请求**。


至此，拦截器的基本功能已经完成，接下来，我们先实现 spring-boot-adminex 的登陆功能，为验证登陆拦截做准备。

## 实现登录功能

1. 将AdminEx模板中的main.html移动到src/main/resources/templates中
2. 在 net.bianheng.www.controller 中创建一个 LoginController， 并在其中添加处理登陆请求的方法 doLogin()，代码略.
3. 在配置类 MyMvcConfig 中添加视图映射，代码如下。

```
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //当访问 “/” 或 “/index.html” 时，都直接跳转到登陆页面
        registry.addViewController("/").setViewName("login");
        registry.addViewController("/index.html").setViewName("login");
        //添加视图映射 main.html 指向  dashboard.html
        registry.addViewController("/main.html").setViewName("main");
    
    }
    ......
}
```

4. 在 login.html 适当位置添加以下代码，显示错误信息。

```
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```



* 下面就是验证了
* ez



# Springboot默认异常处理

在日常的web开发中, 会经常遇到各种异常,此时就需要一个统一的异常处理机制了.

这个机制要用来保证客户端接受较为友好的提示.

springboot本身也提供了一套默认的异常处理机制.

# springboot默认异常处理机制

一旦程序出现异常. springboot会自动识别客户端的类型(浏览器客户端/机器客户端)

* 根据客户端的不同, 以不同的形式展示异常信息

1. 对于浏览器客户端,  springboot会响应一个"whitelabel"的错误视图, 以html格式呈现错误信息

![img](http://c.biancheng.net/uploads/allimg/210726/1605202Q1-0.png)

2. 对于机器客户端而言,  springboot将生成JSON响应,  来展示异常信息.

```
{
    "timestamp": "2021-07-12T07:05:29.885+00:00",
    "status": 404,
    "error": "Not Found",
    "message": "No message available",
    "path": "/m1ain.html"
}
```

## springboot 异常处理自动配置原理

springboot通过**配置类ErrorMvcAutoConfiguration**对异常处理了自动配置

该配置向容器中注入以下4个组件

+ ErrorPageCustomizer：该组件会在在系统发生异常后，默认将请求转发到“/error”上。
+ BasicErrorController：处理默认的“/error”请求。

+ DefaultErrorViewResolver：默认的错误视图解析器，将异常信息解析到相应的错误视图上。

+ DefaultErrorAttributes：用于页面上共享异常信息。

### ErrorPageCustomizer

ErrorPageCustomizer 向容器中注入了一个名为ErrorPageCustomizer的组件,

它主要用来定制错误页面的响应规则.

````
@Bean
public ErrorPageCustomizer errorPageCustomizer(DispatcherServletPath dispatcherServletPath) {
    return new ErrorPageCustomizer(this.serverProperties, dispatcherServletPath);
}
````

ErrorPageCustomizer 通过 registerErrorPages() 方法来注册错误页面的响应规则。

当系统中发生异常后，ErrorPageCustomizer  组件会自动生效，

并将请求转发到 “/error”上，

交给 BasicErrorController 进行处理，其部分代码如下。

```
@Override
public void registerErrorPages(ErrorPageRegistry errorPageRegistry) {
    //将请求转发到 /errror（this.properties.getError().getPath()）上
    ErrorPage errorPage = new ErrorPage(this.dispatcherServletPath.getRelativePath(this.properties.getError().getPath()));
    // 注册错误页面
    errorPageRegistry.addErrorPages(errorPage);
}
```

### BasicErrorController

ErrorMvcAutoConfiguration还向容器中注入了一个错误控制器组件 BasicErrorController

```
@Bean
@ConditionalOnMissingBean(value = ErrorController.class, search = SearchStrategy.CURRENT)
public BasicErrorController basicErrorController(ErrorAttributes errorAttributes,
                                                 ObjectProvider<ErrorViewResolver> errorViewResolvers) {
    return new BasicErrorController(errorAttributes, this.serverProperties.getError(),
            
            errorViewResolvers.orderedStream().collect(Collectors.toList()));
}
```

BasicErrorController的源码如下

* 略略略

Spring Boot 通过 BasicErrorController 进行统一的错误处理（例如默认的“/error”请求）。

Spring Boot 会自动识别发出请求的客户端的类型（浏览器客户端或机器客户端），

并根据客户端类型，将请求分别交给 errorHtml() 和 error() 方法进行处理。



方法返回值不记, 属于是完全看不懂了

* ==换句话说，当使用浏览器访问出现异常时，会进入 BasicErrorController 控制器中的 errorHtml() 方法进行处理，当使用安卓、IOS、Postman 等机器客户端访问出现异常时，就进入error() 方法处理。==

源码略略

***

### DefaultErrorViewResolver

ErrorMvcAutoConfiguration 还向容器中注入了一个默认的错误视图解析器组件

啊对,   就是DefaultErrorViewResolver

代码如下:

````
@Bean
@ConditionalOnBean(DispatcherServlet.class)
@ConditionalOnMissingBean(ErrorViewResolver.class)
DefaultErrorViewResolver conventionErrorViewResolver() {
    return new DefaultErrorViewResolver(this.applicationContext, this.resources);
}
````

当发出请求的客户端为浏览器时，

Spring Boot 会获取容器中所有的 ErrorViewResolver 对象（错误视图解析器），

并分别调用它们的 resolveErrorView() 方法对异常信息进行解析，

其中自然也包括 DefaultErrorViewResolver（默认错误信息解析器）。



源码略

***

DefaultErrorViewResolver 解析异常信息的步骤如下：

1. 根据**错误状态码**（例如 404、500、400 等），生成一个**错误视图 error/status**，例如 error/404、error/500、error/400。
2. 尝试使用**模板引擎解析 error/status 视图，即尝试**从 classpath 类路径下的 templates 目录**下，******查找 error/status.html**，例如 error/404.html、error/500.html、error/400.html。
3. 若模板引擎能够解析到 error/status 视图，则将视图和数据封装成 ModelAndView 返回并结束整个解析流程，否则跳转到第 4 步。
4. 依次从**各个静态资源文件夹中查找 error/status.html**，若在静态文件夹中找到了该错误页面，则返回并结束整个解析流程，否则跳转到第 5 步。
5. 将错误状态码（例如 404、500、400 等）转换为 4xx 或 5xx，然后重复前 4 个步骤，若解析成功则返回并结束整个解析流程，否则跳转第 6 步。 
6. 处理默认的 “/error ”请求，使用 Spring Boot 默认的错误页面（Whitelabel Error Page）。



### DefaultErrorAttributes

ErrorMvcAutoConfiguration 还向容器中注入了一个组件默认错误属性处理工具**DefaultErrorAttributes**

代码如下:

```
@Bean
@ConditionalOnMissingBean(value = ErrorAttributes.class, search = SearchStrategy.CURRENT)
public DefaultErrorAttributes errorAttributes() {
    return new DefaultErrorAttributes();
}
```

DefaultErrorAttributes 是 Spring Boot 的默认错误属性处理工具，它可以从请求中获取异常或错误信息，并将其封装为一个 Map 对象返回，

在 Spring Boot 默认的 Error 控制器（BasicErrorController）处理错误时，会调用 DefaultErrorAttributes 的 getErrorAttributes() 方法获取错误或异常信息，并封装成 model 数据（Map 对象），返回到页面或 JSON 数据中。该 model 数据主要包含以下属性：

+ timestamp：时间戳；
+ status：错误状态码
+ error：错误的提示
+ exception：导致请求处理失败的异常对象
+ message：错误/异常消息
+ trace： 错误/异常栈信息
+ path:错误/异常抛出时所请求的URL路径

```
所有通过 DefaultErrorAttributes 封装到 model 数据中的属性，都可以直接在页面或 JSON 中获取。
```



# Springboot 全局异常处理

springboot提供的默认异常处理机制并不一定适合实际的业务场景

因此, 通常会根据自身的需求对springboot的全局异常进行统一定制

例如定制错误页面, 定制错误数据

## 定制错误页面

可以通过以下三种方式定制springboot的错误页面

* 自定义error.html
* 自定义动态错误页面
* 自定义静态错误页面

## 自定义error.html

可以直接在模板引擎文件夹(/resources/templates)下创建error.html, 覆盖springboot默认的错误视图页面(Whitelabel Error Page)

### 示例1

1. 在spring-boot-adminex的模板引擎文件夹()



