<!-- TOC -->

- [第一章、Spring之旅](#第一章spring之旅)
    - [1、Spring的核心特性：依赖注入（DI会将所依赖的关系自动交给目标对象，而不是让对象自己取获取）、面向切面编程（AOP可以帮助应用将散落在各处的逻辑汇集于切面）](#1spring的核心特性依赖注入di会将所依赖的关系自动交给目标对象而不是让对象自己取获取面向切面编程aop可以帮助应用将散落在各处的逻辑汇集于切面)
        - [1.1 依赖注入能让相互协作的软件保持松散耦合，而面向切面编程能把遍布应用各处的功能分离出来形成可重用的组件](#11-依赖注入能让相互协作的软件保持松散耦合而面向切面编程能把遍布应用各处的功能分离出来形成可重用的组件)
    - [2、spring通过以下四种策略简化了Java开发：](#2spring通过以下四种策略简化了java开发)
        - [2.1 基于POJO 的轻量级和最小侵入性编程](#21-基于pojo-的轻量级和最小侵入性编程)
        - [2.2 通过依赖注入和面向接口实现松耦合【让相互协作的软件组件保持松散的耦合】](#22-通过依赖注入和面向接口实现松耦合让相互协作的软件组件保持松散的耦合)
        - [2.3 基于切面的声明式编程：AOP能够让系统服务模块化（日志这些），并以声明的方式将它们应用到它们需要影响的组件中去。AOP能够确保POJO的简单性](#23-基于切面的声明式编程aop能够让系统服务模块化日志这些并以声明的方式将它们应用到它们需要影响的组件中去aop能够确保pojo的简单性)
            - [1）切点：被切业务中中具体被切的地方（spring支持粒度到方法级别）](#1切点被切业务中中具体被切的地方spring支持粒度到方法级别)
            - [2）切面：横切于业务组件的组件（日志、事务、安全等）](#2切面横切于业务组件的组件日志事务安全等)
            - [3）通知：包含了需要用于多个应用对象的横切行为](#3通知包含了需要用于多个应用对象的横切行为)
        - [2.4 通过切面和模板减少样板式代码：spring通过模板封装来消除样板式代码。比如spring的 JdbcTemplate 使得进行数据库操作时，避免了传统的 JDBC 样板代码。【和mybatis有点像】](#24-通过切面和模板减少样板式代码spring通过模板封装来消除样板式代码比如spring的-jdbctemplate-使得进行数据库操作时避免了传统的-jdbc-样板代码和mybatis有点像)
    - [3、装配（wiring）：创建应用组件之间协作关系的行为叫装配。spring可以使用自动化配置、基于XML和基于Java的配置，来装配bean。](#3装配wiring创建应用组件之间协作关系的行为叫装配spring可以使用自动化配置基于xml和基于java的配置来装配bean)
    - [4、spring容器：在spring应用中，对象由spring容器创建和装配，并存在容器之中。spring自带多个容器实现，主要是两大类](#4spring容器在spring应用中对象由spring容器创建和装配并存在容器之中spring自带多个容器实现主要是两大类)
        - [1）bean 工厂：是最简单的容器，提供基本的DI支持（org.springframework.beans.factory.BeanFactory）](#1bean-工厂是最简单的容器提供基本的di支持orgspringframeworkbeansfactorybeanfactory)
        - [2) 应用上下文（Application Context）【用这个】：基于BeanFactory构建，并提供应用框架级别的服务。spring自带了多种应用上下文的实现，它们之间的主要区别仅仅在于如何加载配置](#2-应用上下文application-context用这个基于beanfactory构建并提供应用框架级别的服务spring自带了多种应用上下文的实现它们之间的主要区别仅仅在于如何加载配置)
            - [1）classPathXmlApllicationContext ：在所有的类路径下（包含jar文件）查找xml配置文件表加载上下文定义](#1classpathxmlapllicationcontext-在所有的类路径下包含jar文件查找xml配置文件表加载上下文定义)
            - [2）AnnotationConfigApplicationContex：从一个或多个基于Java的配置类中加载spring应用上下文](#2annotationconfigapplicationcontex从一个或多个基于java的配置类中加载spring应用上下文)
            - [3）AnnotationConfigWebApplicationContext:从一个或多个基于java的配置类中加载spring Web上下文。](#3annotationconfigwebapplicationcontext从一个或多个基于java的配置类中加载spring-web上下文)
            - [4）XmlWebApplicationContext：从web应用下的一个或者多个xml配置文件中加载上下文定义](#4xmlwebapplicationcontext从web应用下的一个或者多个xml配置文件中加载上下文定义)
    - [5、spring 致力于简化Java开发，springboot致力于让spring本身更加简单。](#5spring-致力于简化java开发springboot致力于让spring本身更加简单)
        - [1、springboot应用 Spring Boot starter 和 自动配置来消除 spring 项目中的样板式配置。](#1springboot应用-spring-boot-starter-和-自动配置来消除-spring-项目中的样板式配置)
            - [1）一个简单的spring boot starter 依赖能够替换掉maven构建中多个通用的依赖。例如在项目中添加spring-boot-starter-web依赖后，将会引入spring web和spring MVC模块，以及jackson2数据绑定模块](#1一个简单的spring-boot-starter-依赖能够替换掉maven构建中多个通用的依赖例如在项目中添加spring-boot-starter-web依赖后将会引入spring-web和spring-mvc模块以及jackson2数据绑定模块)
            - [2）自动配置充分利用了spring 4.0的条件化配置特性，能够自助配置特定的spring bean，用来启动某项特性。例如：springboot能够在应用的类路径中探测到thymeleaf，然后将thymeleaf模板配置为springMVC视图的bean。](#2自动配置充分利用了spring-40的条件化配置特性能够自助配置特定的spring-bean用来启动某项特性例如springboot能够在应用的类路径中探测到thymeleaf然后将thymeleaf模板配置为springmvc视图的bean)
- [第二章、装配Bean](#第二章装配bean)
    - [1.1 Spring 为 bean 的装配提供了三种机制：自动化配置、基于Java的显式配置、基于XML的显式配置————这些技术都描述了spring应用中的组件以及这些组件之间的关系](#11-spring-为-bean-的装配提供了三种机制自动化配置基于java的显式配置基于xml的显式配置这些技术都描述了spring应用中的组件以及这些组件之间的关系)
    - [一、自动化配置bean：使用组件扫描和自动装配实现spring的自动化配置【推荐】](#一自动化配置bean使用组件扫描和自动装配实现spring的自动化配置推荐)
        - [1、Spring 通过两个角度来实现自动化装配：](#1spring-通过两个角度来实现自动化装配)
            - [1）组件扫描【@ComponentScan】：Spring 会去寻找带有 @component 注解的类，并为其在容器中创建 bean](#1组件扫描componentscanspring-会去寻找带有-component-注解的类并为其在容器中创建-bean)
            - [2）自动装配【@Autowired】：Spring 自动满足 bean 之间的依赖【自动从容器中找到 bean 注入进来】](#2自动装配autowiredspring-自动满足-bean-之间的依赖自动从容器中找到-bean-注入进来)
        - [2、配置步骤：](#2配置步骤)
            - [1）@Component ：创建可被发现的 bean](#1component-创建可被发现的-bean)
            - [2）启动组件扫描去寻找带有 @component 注解的类，并为其在容器中创建 bean](#2启动组件扫描去寻找带有-component-注解的类并为其在容器中创建-bean)
                - [2.1 方式一：在JavaConfig中启动组件扫描](#21-方式一在javaconfig中启动组件扫描)
                - [2.2 方式二：在 XML 中启动组件扫描](#22-方式二在-xml-中启动组件扫描)
            - [3）测试：用于创建应用上下文【Spring容器】，并加载配置【把bean加入容器，包括依赖关系弄好】](#3测试用于创建应用上下文spring容器并加载配置把bean加入容器包括依赖关系弄好)
        - [3、补充：](#3补充)
            - [1）spring 应用上下文中所有的bean都会给定一个ID](#1spring-应用上下文中所有的bean都会给定一个id)
            - [2）组件扫描](#2组件扫描)
            - [3）自动装配@Autowire注意事项](#3自动装配autowire注意事项)
    - [二、基于Java的显式配置：你无法对第三方类库进行自动话装配，所以用这个](#二基于java的显式配置你无法对第三方类库进行自动话装配所以用这个)
        - [2.1 配置步骤](#21-配置步骤)
            - [1）创建配置类：该类包含在spring应用上下文如何创建bean的细节，并装配bean【bean之间的依赖关系】——你用的时候还是用@Autowire，在应用上下文中找对应的bean](#1创建配置类该类包含在spring应用上下文如何创建bean的细节并装配beanbean之间的依赖关系你用的时候还是用autowire在应用上下文中找对应的bean)
    - [三、基于XML的显式配置(spring从xml中获取信息来创建bean)：](#三基于xml的显式配置spring从xml中获取信息来创建bean)
        - [3.1 步骤：](#31-步骤)
- [第四章、面向切面的spring](#第四章面向切面的spring)
    - [一、概述：](#一概述)
        - [1.1 横切关注点：是散布于应用中多处的功能。（日志、事务、安全）](#11-横切关注点是散布于应用中多处的功能日志事务安全)
        - [1.2 依赖注入有助于应用对象之间的解耦，而AOP可以实现横切关注点与它们所影响的对象之间的解耦](#12-依赖注入有助于应用对象之间的解耦而aop可以实现横切关注点与它们所影响的对象之间的解耦)
    - [二、术语：](#二术语)
        - [1.1 通知：通知包含了需要应用于多个应用对象的横切行为。](#11-通知通知包含了需要应用于多个应用对象的横切行为)
        - [](#)

<!-- /TOC -->

# 第一章、Spring之旅
## 1、Spring的核心特性：依赖注入（DI会将所依赖的关系自动交给目标对象，而不是让对象自己取获取）、面向切面编程（AOP可以帮助应用将散落在各处的逻辑汇集于切面） 
### 1.1 依赖注入能让相互协作的软件保持松散耦合，而面向切面编程能把遍布应用各处的功能分离出来形成可重用的组件
* 相对于EJB（企业级JavaBean），spring提供了更加轻量级和简单的编程模型。它增强了老式Java对象（Plain Old Java object ，POJO）的功能，使之具备了和其他企业级Java规范才具有的功能
## 2、spring通过以下四种策略简化了Java开发：
### 2.1 基于POJO 的轻量级和最小侵入性编程
* spring的非侵入式编程意味着这个类在spring应用和非spring应用中都可以发挥同样的作用（一个类或许会使用spring注解，但它依旧式POJO）
  * 而很多框架通过强迫应用继承它的类或实现它的接口而导致应用和框架绑死
  * 另：spring用bean或者JavaBean来表示应用组件，一个spring组件可以是任何形式的POJO【JavaBean = POJO】
### 2.2 通过依赖注入和面向接口实现松耦合【让相互协作的软件组件保持松散的耦合】
* 1） 为啥要松耦合，因为耦合具有两面性：
    * 一方面，紧密耦合的代码难以测试，难以复用，难以理解，并且修复一个bug，经常会出现一个甚至更多的bug
    * 另一方面，一定程度的耦合是必须的，完全没有耦合的代码什么也做不了。
* 2）依赖注入会将所依赖的关系自动交给目标对象，而不是让对象自己取获取，从而实现松耦合。
  * 通过 DI ，对象的依赖关系将由系统中负责协调各对象的第三方组件在创建对象的时候进行设定。对象无需自行创建或管理它们的依赖关系。
* 3）如果一个对象只通过一个接口（而不是具体的实现或初始化过程）来表明依赖关系，那么这种依赖就能够在对象本身毫不知情的情况下，用不同的具体实现进行替换，从而实现松耦合。

### 2.3 基于切面的声明式编程：AOP能够让系统服务模块化（日志这些），并以声明的方式将它们应用到它们需要影响的组件中去。AOP能够确保POJO的简单性
#### 1）切点：被切业务中中具体被切的地方（spring支持粒度到方法级别）
#### 2）切面：横切于业务组件的组件（日志、事务、安全等）
#### 3）通知：包含了需要用于多个应用对象的横切行为
### 2.4 通过切面和模板减少样板式代码：spring通过模板封装来消除样板式代码。比如spring的 JdbcTemplate 使得进行数据库操作时，避免了传统的 JDBC 样板代码。【和mybatis有点像】

## 3、装配（wiring）：创建应用组件之间协作关系的行为叫装配。spring可以使用自动化配置、基于XML和基于Java的配置，来装配bean。
## 4、spring容器：在spring应用中，对象由spring容器创建和装配，并存在容器之中。spring自带多个容器实现，主要是两大类
### 1）bean 工厂：是最简单的容器，提供基本的DI支持（org.springframework.beans.factory.BeanFactory）
### 2) 应用上下文（Application Context）【用这个】：基于BeanFactory构建，并提供应用框架级别的服务。spring自带了多种应用上下文的实现，它们之间的主要区别仅仅在于如何加载配置
#### 1）classPathXmlApllicationContext ：在所有的类路径下（包含jar文件）查找xml配置文件表加载上下文定义
```javaiao
import org.springframework.context.support.
                   ClassPathXmlApplicationContext;

public class KnightMain {

  public static void main(String[] args) throws Exception {
    ClassPathXmlApplicationContext context = 
        new ClassPathXmlApplicationContext(
            "META-INF/spring/knight.xml");  //基于knights.xml文件创建了spring应用上下文，
    Knight knight = context.getBean(Knight.class);//调用应用上下文获取一个ID为knight的bean
    knight.embarkOnQuest();
    context.close();
  }

}
```

#### 2）AnnotationConfigApplicationContex：从一个或多个基于Java的配置类中加载spring应用上下文
#### 3）AnnotationConfigWebApplicationContext:从一个或多个基于java的配置类中加载spring Web上下文。
#### 4）XmlWebApplicationContext：从web应用下的一个或者多个xml配置文件中加载上下文定义
## 5、spring 致力于简化Java开发，springboot致力于让spring本身更加简单。
### 1、springboot应用 Spring Boot starter 和 自动配置来消除 spring 项目中的样板式配置。
#### 1）一个简单的spring boot starter 依赖能够替换掉maven构建中多个通用的依赖。例如在项目中添加spring-boot-starter-web依赖后，将会引入spring web和spring MVC模块，以及jackson2数据绑定模块
#### 2）自动配置充分利用了spring 4.0的条件化配置特性，能够自助配置特定的spring bean，用来启动某项特性。例如：springboot能够在应用的类路径中探测到thymeleaf，然后将thymeleaf模板配置为springMVC视图的bean。


# 第二章、装配Bean
## 1.1 Spring 为 bean 的装配提供了三种机制：自动化配置、基于Java的显式配置、基于XML的显式配置————这些技术都描述了spring应用中的组件以及这些组件之间的关系
## 一、自动化配置bean：使用组件扫描和自动装配实现spring的自动化配置【推荐】

### 1、Spring 通过两个角度来实现自动化装配：
#### 1）组件扫描【@ComponentScan】：Spring 会去寻找带有 @component 注解的类，并为其在容器中创建 bean
#### 2）自动装配【@Autowired】：Spring 自动满足 bean 之间的依赖【自动从容器中找到 bean 注入进来】

### 2、配置步骤：
#### 1）@Component ：创建可被发现的 bean

```java
//SgtPeppers是一张具体的CD

@Component  //此注解表明该类回作为组件类，并告知spring要为这个类创建bean
public class SgtPeppers implements CompactDisc {

  private String title = "Sgt. Pepper's Lonely Hearts Club Band";  
  private String artist = "The Beatles";
  
  public void play() {
    System.out.println("Playing " + title + " by " + artist);
  }
  
}
```

#### 2）启动组件扫描去寻找带有 @component 注解的类，并为其在容器中创建 bean

##### 2.1 方式一：在JavaConfig中启动组件扫描
```java
//这是一个CD播放机的配置类javaConfig
package soundsystem;

@Configuration //表明这是一个配置类，基于Java的配置类
@ComponentScan //***在spring中启动组件扫描，寻找带有 @component 注解的类，并为其在容器中创建 bean。【默认扫描此类相同包下的所有子包】
public class CDPlayerConfig { 
}

```
##### 2.2 方式二：在 XML 中启动组件扫描
```xml
  <context:component-scan base-package="soundsystem" />  <!--基础包是soundsystem，于是扫描它及其子包-->
```

#### 3）测试：用于创建应用上下文【Spring容器】，并加载配置【把bean加入容器，包括依赖关系弄好】
```java
@RunWith(SpringJUnit4ClassRunner.class)  //使用JUnit4进行单元测试
@ContextConfiguration(classes=CDPlayerConfig.class)
 //告诉spring需要在CDPlayerConfig类中加载配置【因为它有@componentScan ，会启动组件扫描把相关带了@Component的组件加入应用上下文】
public class CDPlayerTest {

  @Rule
  public final StandardOutputStreamLog log = new StandardOutputStreamLog();

  @Autowired  //会在应用上下文中寻找匹配我的bean，装配进来
  private MediaPlayer player;
  
  @Autowired
  private CompactDisc cd;
  
  @Test
  public void cdShouldNotBeNull() {
    assertNotNull(cd);
  }

  @Test
  public void play() {
    player.play();
    assertEquals(
        "Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles\n", 
        log.getLog());
  }

}

```

### 3、补充：
#### 1）spring 应用上下文中所有的bean都会给定一个ID
```java
//@component 
//@Named  //等价于component 
@Component("lonelyclub")//自定义名称ID,否则默认是sgtPeppers
public class SgtPeppers implements CompactDisc
```
#### 2）组件扫描
```java
@Configuration
//@ComponentScan  //默认扫描配置类所在的包及其子包
//@ComponentScan（basePackages = {"soundsystem","video"}）//指定扫描这些基础包及其子包
@ComponentScan（basePackageClasses = {CDPlayer.class,DVDplayer.class}）//这些类所在的包作为组件扫描的基础包
public class CDPlayerConfig { }
```
#### 3）自动装配@Autowire注意事项
* 3.1 如果只有一个bean匹配依赖需求的话，那么这个bean将会被装配起来
* 3.2 如果没有匹配的bean，那么在应用上下文创建的时候spring会抛出异常
* 3.3 如果有多个bean都满足依赖关系，spring会抛出异常，表明没有明确指定要选择哪个bean进行自动装配

## 二、基于Java的显式配置：你无法对第三方类库进行自动话装配，所以用这个
* javaConfig是配置代码，不应该侵入任何业务逻辑代码中
### 2.1 配置步骤
#### 1）创建配置类：该类包含在spring应用上下文如何创建bean的细节，并装配bean【bean之间的依赖关系】——你用的时候还是用@Autowire，在应用上下文中找对应的bean
```java
@Configuration //表明这是一个配置类，该类包含在spring应用上下文如何创建bean的细节
@ComponentScan  //默认扫描配置类所在的包及其子包
public class CDPlayerConfig {
  
  @Bean  //该注解会告诉spring这个方法将返回一个对象，该对象要注册为spring应用上下文中的bean。
  //方法体中包含了最终产生bean实例的逻辑。
  //默认情况下，bean的ID和带有bean注解的方法名是一样的，此处为compactDisc
  public CompactDisc compactDisc() {  
    return new SgtPeppers();
  }

//当这些bean之间存在依赖关系时
//方法一、在javaConfig中注入bean的最简单的方式就是引用创建bean的方法
@Bean
public CDPlayer cdplayer(){
  return new CDPlayer(compactDisc()); //通过调用compactDisc()方法来得到bean
  //默认情况下spring中的bean都是单例的，即spring会拦截对compactDisc（）的调用并确保返回的是spring所创建的bean，
  //也就是大家得到的是相同的compactDisc实例。
}

//方法二、cdplayer方法请求一个CompactDisc作为参数  ***最佳
//不管CompactDisc使用什么方式创建出来的，（扫描还是xml还是配置类），spring都会将其传入到配置方法中，并用来创建cdplayer baan
@Bean
public CDPlayer cdplayer(CompactDisc compactDisc){
  return new CDPlayer(compactDisc); 
}

}
```



## 三、基于XML的显式配置(spring从xml中获取信息来创建bean)：
### 3.1 步骤：



