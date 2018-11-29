

# 第一章、Spring之旅
Spring的核心特性：依赖注入（dependency injection，DI）、面向切面编程（aspect-oriented-programming ，AOP） 
## 1.1



# 第二章、装配Bean
## 1.1 概述
### 1）装配：创建应用对象之间的协作关系的行为叫装配（配置），这也是依赖注入的本质
### 2）Spring 容器：是Spring框架的核心，负责管理应用中组件的生命周期，它会创建这些组件并保证他们的依赖能得到满足
## 1.2 Spring 为 bean 的装配提供了三种机制：自动化配置、基于Java的显式配置、基于XML的显式配置————这些技术都描述了spring应用中的组件以及这些组件之间的关系
## 一、自动化配置bean【推荐】

### 1、Spring 通过两个角度来实现自动化装配：
#### 1）组件扫描【@ComponentScan】：Spring 会去寻找带有 @component 注解的类，并为其在容器中创建 bean
#### 2）自动装配【@Autowired】：Spring 自动满足 bean 之间的依赖【自动从容器中找到 bean 注入进来】

### 2、配置步骤：
#### 1）@Component ：创建可被发现的 bean

```JAVA
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
```JAVA
//这是一个CD播放机的配置类javaConfig


@Configuration //
@ComponentScan //***在spring中启动组件扫描，寻找带有 @component 注解的类，并为其在容器中创建 bean。【默认扫描此类相同包下的所有子包】
public class CDPlayerConfig { 
}

```
##### 2.2 方式二：在 XML 中启动组件扫描
```XML
  <context:component-scan base-package="soundsystem" />  <!--基础包是soundsystem，于是扫描它及其子包-->
```

#### 3）测试：用于创建应用上下文【Spring容器】，并加载配置【把bean加入容器，包括依赖关系弄好】
## 二、基于Java的显式配置


## 三、基于XML的显式配置