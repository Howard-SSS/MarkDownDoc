# 注解

| 名字                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| @SpringBootApplication    | 启动类                                                       |
| @RestController           | @Controller + @ResponseBody                                  |
| @ResponseBody             | 数据响应类                                                   |
| @RequestMapping           | 路径导向                                                     |
| @ComponentScan            | Bean扫描，若无配置basePackages属性，则将当前配置类所在的包当作扫描包 |
| @Autowired                | 属于spring，按类型装配，若找不到或找到多个会抛出异常，用于字段和setter |
| @Resource                 | 属于J2EE，按名称装配，若找不到会抛出错误，用于字段和setter   |
| @ConfigurationProperties  | prefix指定配置文件的某节点                                   |
| @Validate                 | 数据校验-JSR303                                              |
| @PropertiesSource         | properties文件配置bean                                       |
| @Target                   | 定义注解可以用在哪方面                                       |
| @Retention                | 定义注解用什么方式保留                                       |
| @Documented               | 定义java doc会生成注解信息                                   |
| @Inherited                | 定义注解是否会被继承                                         |
| @SpringBootConfiguration  | 标注这是一个SpringBoot配置类                                 |
| @EnableAutoConfiguration  | 开启自动配置                                                 |
| @ConditionalOnMissingBean | 仅当BeanFactory中不包含指定的bean class或name时条件匹配      |
| @ConditionalOnBean        | 与@ConditionalOnMissingBean相反                              |
|                           |                                                              |

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

# 自动配置

#### 视图解析

ContentNegotiatingViewResolver不会解析视图、而是委派给其他视图解析器进行解析

ViewResolver是SpringMVC内置的视图解析器

```java
public class ContentNegotiatingViewResolver extends WebApplicationObjectSupport implements ViewResolver, Ordered, InitializingBean{
    // 视图解析器列表
    @Nullable
	private List<ViewResolver> viewResolvers;
    // 扫描所有Bean初始视图解析器列表
    @Override
	protected void initServletContext(ServletContext servletContext) { ... }
	...
    // 根据返回视图的名称解析视图
    @Override
	@Nullable
	public View resolveViewName(String viewName, Locale locale) throws Exception {
        ...
        // 获得所有匹配的视图
        List<View> candidateViews = getCandidateViews(viewName, locale, requestedMediaTypes);
        // 获取最终那个
        View bestView = getBestView(candidateViews, requestedMediaTypes, attrs);
        if (bestView != null) {
            return bestView;
        }
    	...
	}
}
```

```java
public interface ViewResolver {
    @Nullable
    View resolveViewName(String var1, Locale var2) throws Exception;
}
```

BeanNameViewResolver会根据handler方法返回的视图名称，去ioc容器中找到实现View接口并且同名的Bean

```java
@Controller
@RequestMapping("/test")
public class Controller {
	@GetMapping("abc")
	public String to() {
		return "xyz";
	} 
}
```

```java
@Component
class Xyz implements View {
    @Override
    public String getContentType() {
        return "text/html";
    }
    @Override
    public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
        response.getWriter().print("Welcome to");
    }
}
```

#### 提供静态资源访问

条件：放在指定位置

```java
public class WebMvcAutoConfiguration {
	@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        ...
       	if (!registry.hasMappingForPattern("/webjars/**")) {
            // 访问/webjars/**时会去classpath:/META-INF/resources/webjars/**进行映射
            customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
            .addResourceLocations("classpath:/META-INF/resources/webjars/")
            .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl)
            .setUseLastModified(this.resourceProperties.getCache().isUseLastModified()));
        }
        String staticPathPattern = this.mvcProperties.getStaticPathPattern();
        if (!registry.hasMappingForPattern(staticPathPattern)) {
            // 获得静态资源地址{"classpath:/META-INF/resources/","classpath:/resources/","classpath:/static/","classpath:/public/"}， 在地址里比对
            customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
            .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
            .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl)
            .setUseLastModified(this.resourceProperties.getCache().isUseLastModified()));
        }
    }
}
```

#### 格式转换

#### HttpMessageConverters

负责http请求和响应的报文处理

### WebMvcConfigurer

#### 请求映射

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    // url匹配规则配置
    @Override
	public void configurePathMatch(PathMatchConfigurer configurer) {}
}
```

```java
public class PathMatchConfigurer {
    // 设置是否匹配尾部斜杠 /abc/ 映射到 /abc
	public PathMatchConfigurer setUseTrailingSlashMatch(Boolean trailingSlashMatch) { ... }
    ...
}
```

#### 拦截器配置

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor()); // 添加拦截器，返回InterceptorRegistration类型
    }
}
```

```java
public class MyInterceptor implements HandlerInterceptor {
    // 处理前调用（先添加先调用）
    @Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception { return true; }
    @Override
    // 处理后调用（后添加先调用）
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {}
    @Override
    // 处理后回调（后添加先调用）
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {}
}
```

```java
public class InterceptorRegistration {
    // 添加需要拦截映射
	public InterceptorRegistration addPathPatterns(String... patterns) { ... }
    // 排除不需要拦截映射
    public InterceptorRegistration excludePathPatterns(String... patterns) { ... }
    ...
}
```

#### 跨域配置

跨域是指协议、域名、端口其中有不同

- 全局配置

```java
 @Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
		registry.addMapping("/user/getAll"); // 需要跨域的映射,返回CorsRegistration类型
    }
}
```

```java
public class CorsRegistration {
    // 允许的跨域源头
	public CorsRegistration allowedOrigins(String... origins) { ... }
    // 允许的跨域方法
    public CorsRegistration allowedMethods(String... methods) { ... }
    ...
}
```

- 注解配置

@CrossOrigin

#### WebMvcConfigurer原理

```java
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
public class WebMvcAutoConfiguration {
    @Import(EnableWebMvcConfiguration.class)
    public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer { ... }
    
	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
        ...
    }
}
```

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
    
    // 通过自动注入找到所有Bean
    @Autowired(required = false)
	public void setConfigurers(List<WebMvcConfigurer> configurers) {
		if (!CollectionUtils.isEmpty(configurers)) {
			this.configurers.addWebMvcConfigurers(configurers);
		}
	}
    ...
}
```

```java
class WebMvcConfigurerComposite implements WebMvcConfigurer {
    private final List<WebMvcConfigurer> delegates = new ArrayList<>();
    
    public void addWebMvcConfigurers(List<WebMvcConfigurer> configurers) {
		if (!CollectionUtils.isEmpty(configurers)) {
			this.delegates.addAll(configurers);
		}
	}
    ...
}
```

当添加了@EnableWebMvc就不会使用springMVC自动配置类的默认配置

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
}
```

```java
@Configuration(proxyBeanMethods = false)
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport { ... }
```

原因是WebMvcAutoConfiguration类有@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)注解