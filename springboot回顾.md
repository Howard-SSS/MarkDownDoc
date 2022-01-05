# 注解

| 名字                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| @SpringBootApplication   | 启动类                                                       |
| @RestController          | @Controller + @ResponseBody                                  |
| @ResponseBody            | 数据响应类                                                   |
| @RequestMapping          | 路径导向                                                     |
| @ComponentScan           | Bean扫描，若无配置basePackages属性，则将当前配置类所在的包当作扫描包 |
| @Autowired               | 属于spring，按类型装配，若找不到或找到多个会抛出异常，用于字段和setter |
| @Resource                | 属于J2EE，按名称装配，若找不到会抛出错误，用于字段和setter   |
| @ConfigurationProperties | prefix指定配置文件的某节点                                   |
| @Validate                | 数据校验-JSR303                                              |
| @PropertiesSource        | properties文件配置bean                                       |
| @Target                  | 定义注解可以用在哪方面                                       |
| @Retention               | 定义注解用什么方式保留                                       |
| @Documented              | 定义java doc会生成注解信息                                   |
| @Inherited               | 定义注解是否会被继承                                         |
| @SpringBootConfiguration | 标注这是一个SpringBoot配置类                                 |
| @EnableAutoConfiguration | 开启自动配置                                                 |
|                          |                                                              |
|                          |                                                              |
|                          |                                                              |

# 配置文件

application.properties

扁平的k/v格式

application.yml

树形结构

共存时，优先读取的属性投入使用

#### 配置文件位置优先级

classpath(resource)根目录 < classpath(resource)根目录config/ < 项目根目录 < 项目根目录config/ < jar包外 < 直接子目录(电脑某目录)config/

# SPEL（Spring Express Language）

- @Value

  可以加在Class的成员变量和形参上

  - 字符串

    直接赋值

  - #{}

    bean赋值

  - ${}

    properties赋值

- ​	XML
- Expression
  
  - 

# JSR303

![img](https://upload-images.jianshu.io/upload_images/3145530-8ae74d19e6c65b4c?imageMogr2/auto-orient/strip|imageView2/2/w/654/format/webp)![img](https://upload-images.jianshu.io/upload_images/3145530-10035c6af8e90a7c?imageMogr2/auto-orient/strip|imageView2/2/w/432/format/webp)

# 热部署

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

# 日志

spring boot 默认用的是Logback + slf4j

#### 格式

```xml
2022-01-04 15:03:22.665 TRACE 10072 --- [   main] com.example.demo  : text
日期		  时间		 级别   进程号     线程名	  类名				 信息
%clr(%d{${LOG_DATEFORMAT_PATTERN: - yyyy-MM-dd HH:mm:ss.SSS}})(faint)
%clr(${LOG_LEVEL_PATTERN: -%5p})
%clr(${PID: - }){magenta}
%clr(---)(faint)
%clr([%15.15t])(faint)
%clr(%-40.40logger{39}){cyan}
%clr(:)(faint)
%m%n${LOG_EXCEPTION_CONVERSION_WORD: -%wEx}
/* 注释
%clr()() 颜色
${} 占位符
LOG_DATEFORMAT_PATTERN 环境变量
*/
```

#### 文件输出

| logging.file.name                                            | logging.file.path                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 写入指定的日志文件。名称可以是确切的位置，也可以是相对于当前目录的名称。 | 写入指定的目录。名称可以是确切的位置，也可以是相对于当前目录的名称。`spring.log` |

#### 归档

```xml
logging.logback.rollingpolicy.file-name.pattern=${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz
/* 注释
LOG_FILE 对应配置文件属性logging.file.name
%i 序列号
*/
```

#### slf4j依赖

slf4j-api

#### slf4j与其他各种日志组件桥接器说明

| jar包名              | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| slf4j-jdk.jar        | java.util.logging的桥接器                                    |
| slf4j-log4j12.jar    | log4j1.2版本的桥接器                                         |
| log4j-slf4j-impl.jar | log4j2的桥接器                                               |
| logback-classic.jar  | slf4j的原生实现，Logback直接实现了slf4j的接口                |
| slf4j-jcl.jar        | Jakarta Commons Logging的桥接器，这个桥接器将slf4j所有日志委派给jcl |

#### slf4j与其他组件适配器说明

| jar包名            | 说明            |
| ------------------ | --------------- |
| jcl-over-slf4j.jar | jcl(门面)适配器 |
| jul-to-slf4j.jar   | jul(实现)适配器 |

