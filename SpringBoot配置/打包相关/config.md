### 打包的项目名与类型

1. build中配置finalName属性
2. packaging为打包类型（不写默认是jar ），springboot也建议我们打成jar包

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ruoyi</artifactId>
        <groupId>com.ruoyi</groupId>
        <version>3.7.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <packaging>jar</packaging>
    <artifactId>ruoyi-admin</artifactId>
    ......
    
    <build>
        <plugins>
            
            <plugin>   
                <groupId>org.apache.maven.plugins</groupId>   
                <artifactId>maven-war-plugin</artifactId>   
                <version>3.1.0</version>   
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                    <warName>${project.artifactId}</warName>
                </configuration>   
           </plugin>   
        </plugins>
        <finalName>${project.artifactId}</finalName>
    </build>
    
```



### 打war包步骤

#### 1.  在pom.xml文件中,修改打包形式

```xml
<packaging>war</packaging>
```



#### 2. 排除内嵌tomcat和jsp依赖
在pom.xml文件中,移除嵌入式tomcat插件,或将原来的内置tomcat依赖scope改为provided；；而内嵌的tomcat依赖（spring-boot-starter-tomcat）在spring-boot-starter-web依赖的子依赖中；一旦排除后，刷新以来，内嵌的tomcat依赖（spring-boot-starter-tomcat）会变成灰色。变成灰色的意思是在打包时，不会打到包里面。

如果用jsp了，还是排除jsp的解析依赖;因为springboot如果要用jsp，肯定要引入jsp依赖，但是tomcat能解析jsp,不需要以来，但是在idea中（springbbot）中，又需要这个以来；所以如果打成war包，就需要排除jsp依赖;当然即使项目没有用Jsp，加上这个排除jsp依赖也不会有问题。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- 移除嵌入式tomcat插件 -->
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!--或者像下面的写法-->

<!-- 将原来的内置tomcat依赖scope改为provided-->
<!-- 打war包时加入此项， 告诉spring-boot tomcat相关jar包用外部的（也就是用置tomcat,war肯定要放到外部的tomcat上），不要打进去 -->
<!-- 内置tomcat -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>


```

```xml
<!--去掉使用内嵌tomcat解析jsp-->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
	<scope>provided</scope>
</dependency>
```



##### 2.1  关于springboot 打包过程中提示没有web.xml文件的处理

使用Maven打包项目的时候，出现错误:
webxml attribute is required (or pre-existing WEB-INF/web.xml if executing in update)

原因分析
web项目下缺少WEB-INF/web.xml

当是在servlet 3.0之后，对于web.xml文件本身是可选的

##### 2.1.1 第一解决方案

在pom.xml文件中定义一个参数配置failOnMissingWebXml

```xml

<!-- 按照SpringBoot项目结构是没有Web目录的,maven插件需要增加这行:failOnMissingWebXml-->
<!--servlet3.0之后可以没有web.xml配置，而用类来配置-->
    <build>
        <!-- 打包出的war包名字-->
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-war-plugin</artifactId>
				<configuration>
					<failOnMissingWebXml>false</failOnMissingWebXml>
				</configuration>
			</plugin>
        </plugins>
    </build>
```

##### 2.1.2 第二种解决方案

更新maven-war-plugin的版本

```xml
<plugin>
  <artifactId>maven-war-plugin</artifactId>
  <version>3.0.0</version>
</plugin>
```

##### 2.1.3 第三种解决方案

在webapp目录下创建WEB-INF/web.xml

![img](https://img-blog.csdnimg.cn/2019081214560991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5OTU2Mg==,size_16,color_FFFFFF,t_70)

一般springboot没有webapp，所以一般用第一种方案；

当然可以手动建立Web Resource Directory，一般名字都叫webbapp，当然名字随便。并设置为Web Resource Directory（步骤 建webapp文件夹，位置如下 \src\main\webapp；project structure->Modules->找到自己的module并展开->Web->Web Resource Directories下面点加号->Web resource directory path加上自己建的webapp,Relative path in deployment directory默认就好，也就是 /）

然后再新建WEB-INF/web.xml



#### 3.  添加javax.servlet-api或者tomcat-servlet-api依赖

既然不用内置`tocmat`了，那么就需要添加`servlet`依赖

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
</dependency>

<!-- 或tomcat-servlet-api-->
<dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-servlet-api</artifactId>
    <version>8.0.36</version>
    <scope>provided</scope>
</dependency>
```

#### 4. 配置入口类
修改项目默认启动方式，启动类继承SpringBootServletInitializer类重写configure()方法，或新建类继承SpringBootServletInitializer类重写configure()方法

项目启动类，原来的入口不能直接用了，需要另外一种外部`tomcat`启动的方式，要告诉tomcat 从哪启动

如果不配置入口类，那么访问时会报404

```java
package com.single.zxblog;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;


@SpringBootApplication
public class ZxblogApplication extends SpringBootServletInitializer {
    
    public static void main(String[] args) {
        SpringApplication.run(ZxblogApplication.class, args);
    }
    
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(ZxblogApplication.class);
    }
}
```


或在原启动类同级下新建ZxblogStartApplication(任意名称)启动类,重写configure()方法并指向ZxblogApplication类

```java
package com.single.zxblog;


import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;


public class ZxblogStartApplication extends SpringBootServletInitializer {
    /**
     * 默认返回return super.configure(builder);
     * 现指向ZxblogApplication.main()启动方法
     * @param builder
     * @return
     */
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(ZxblogApplication.class);
    }
}
```

或者在配置文件中

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <!--增加以下内容-->
    <configuration>
    	<fork>true</fork>
        <!--增加jvm参数-->
        <jvmArguments>Dfile.encoding=UTF-8</jvmArguments>
        <!--指定入口类-->
        <mainClass>com.XXX.XXX</mainClass>
    </configuration>
</plugin>
```



#### 5. 打包
```go
E:\java_project\zxblog>mvn clean package -Dmaven.test.skip=true
```
放入tomcat的webapps目录下即可。可以对war重命名任意名称再放入webapps中，一般重命名为有意义的项目名称

> 即使省略2、3，打成war包，入到tomcat中，也不会报错，但是在tomcat中还有一个tomcat，也就是在打包好的war中的WEB-INF/lib中有tomcat相应的jar，会让人担心以后出现不可预知的问题，但是目前没有听说过会有问题。

### 打jar包步骤

#### 1. 修改打包类型
```xml
<packaging>war</packaging>
```

#### 2. 修改插件配置

如果项目中有jsp，必须指定maven-plugin的版本为1.4.2.RELEASE，其他版本都不好使

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>1.4.2.RELEASE</version>
</plugin>
```

#### 3. 打包

```go
maven clean package
```

#### 4. 运行

把jar文件交给用户，然后运行这个jar，可以写一个bat脚本文件运行；这个Jar是一个springboot的项目；

```
java -jar xxxx.jar 
```

如果在linux中运行也是这个命令，当然也可以写一个脚本文件，名称为run.sh

```sh
#!/bin/sh
java -jar xxx.jar
```

然后赋权限 chmod 777 run.sh

然后启动shell 脚本 ./run.sh

> 也就是说如果一个springboot想打成jar包，并且没有jsp，则不用进行任何的配置直接打包即可
>
> spirngboot极力推荐我们打成jar包




### jar 与 war 区别

####  1. 

对于web项目，jar包与war包只有一个区别，就是上传文件的地方；jar包上传肯定不能上传到项目 ，因为是一个jar，不能再解压再放入上传的文件；所以要配置上传的文件为一个物理路径，然后映射到这个路径。而war不存在这个问题。

```
做法其实很简单：jar不让传文件，那就把jar中的目录映射到本地磁盘中的真实路径，然后把本地磁盘的真实路径做为tomcat中一个虚拟静态目录，去加载，给其提供web功能，可以访问，就行了
```

https://blog.csdn.net/zhangpower1993/article/details/89016503

方式一实现WebMvcConfigurer接口（推荐），方式二继承WebMvcConfigurationSupport类
这接口和类中有以下常用方法

```java
/* 拦截器配置 */
void addInterceptors(InterceptorRegistry var1);
/* 视图跳转控制器 */
void addViewControllers(ViewControllerRegistry registry);
/**
     *静态资源处理
**/
void addResourceHandlers(ResourceHandlerRegistry registry);
/* 默认静态资源处理器 */
void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer);
/**
     * 这里配置视图解析器
 **/
void configureViewResolvers(ViewResolverRegistry registry);
/* 配置内容裁决的一些选项*/
void configureContentNegotiation(ContentNegotiationConfigurer configurer);
/** 解决跨域问题 **/
public void addCorsMappings(CorsRegistry registry) ;
```

步骤如下 ：

application.yml 配置上传文件路径

```yml
cms:
  #上传文件的路径
  profile: D:/UploadFiles
  ......
  #开发环境访问路径前缀
  development-prefix-path: /dev-api
  #生产环境访问路径前缀
  productionPrefixPath: /prod-api
```

编写读取配置文件信息的配置类，当然也可以不用写这个类

```java
@Component
@ConfigurationProperties(prefix = "cms")
public class CmsConfig {
    /** 上传路径 */
    private static String profile;
    public static String getProfile()
    {
        return profile;
    }

    public void setProfile(String profile)
    {
        CmsConfig.profile = profile;
    }
    ......
        
        
}
```

实现接口或者继承类

```java
@Configuration
public class XXXXXXConfig implements WebMvcConfigurer {

    @Value("${debug}")
    private boolean debug;

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        if(this.debug){
            registry.addResourceHandler(CmsConfig.getDevelopmentPrefixPath()+RESOURCE_PREFIX +"/**")
                    .addResourceLocations("file:" + CmsConfig.getProfile() + "/")//file:指本地磁盘路径 如file:D:/UploadFiles
                	.addResourceLocations("classpath:/static/")//指项目的相对路径
            return;
        }
        registry.addResourceHandler(CmsConfig.getProductionPrefixPath()+RESOURCE_PREFIX +"/**")
                .addResourceLocations("file:" + CmsConfig.getProfile() + "/");
    }
}

```

- addResoureHandler：指的是对外暴露的访问路径
- addResourceLocations：指的是内部文件放置的目录

当我访问文件时，服务器不是在classpath找，而是需要找D:/UploadFiles，所以需要配置真实路径为服务器静态资源路径;;这一步如果不配置，上传没有问题，访问肯定会报404

```yml
spring:
  web:
    resources:
      static-locations: classpath:/static/,file:${cms.profile}
```




1. 打war包，springgboot中配置的port，context-path 都将失效。原因是项目原来由springboot内嵌的tomcat管理 ，现在由自己的tomcat管理了。
2. 