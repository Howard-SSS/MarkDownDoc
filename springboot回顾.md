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