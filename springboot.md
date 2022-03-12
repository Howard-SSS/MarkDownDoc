# AOP

面相切面编程，也是面向方法编程，通过预编译方式和运行期动态代理的方式实现不修改源代码的情况下给程序动态统一添加功能的技术

# IOC

通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递给它

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
| @JsonIgnore               | 不会进行格式化                                               |
| @JsonFormat               | 格式化                                                       |
| @JsonInclude              | 匹配时才格式化                                               |
| @JsonProperty             | 别名                                                         |
| @RequestParam             | 请求头参数注入                                               |
| @PathVariable             | 路径参数注入                                                 |
| @RequestBody              | 请求体参数注入                                               |

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

File->Setting->Compiler-Build Project automatically

ctrl+shift+alt+/，选择Registry，勾上Compiler automake allow when app running

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

| 依赖名                     | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| slf4j-jdk.jar              | java.util.logging的桥接器                                    |
| slf4j-log4j12.jar          | log4j1.2版本的桥接器                                         |
| spring-boot-starter-log4j2 | log4j2的桥接器                                               |
| logback-classic.jar        | slf4j的原生实现，Logback直接实现了slf4j的接口                |
| slf4j-jcl.jar              | Jakarta Commons Logging的桥接器，这个桥接器将slf4j所有日志委派给jcl |

#### slf4j与其他组件适配器说明

| jar包名            | 说明            |
| ------------------ | --------------- |
| jcl-over-slf4j.jar | jcl(门面)适配器 |
| jul-to-slf4j.jar   | jul(实现)适配器 |

# 测试

MockMVC

```java
@Test
void getTest() throws Exception {
    mockMvc.perform(
        MockMvcRequestBuilders.get("/g").param("id", "12345")
    ).andExpect(MockMvcResultMatchers.status().isOk())
        .andDo(MockMvcResultHandlers.print());
}
```

```java
@Test
void multiTest() throws Exception {
    File file = new File("C:\\Users\\心里的潇洒情\\Desktop\\0c3c28bfd9fbe87b952ce334fd430ae.jpg");
    MockMultipartFile mockFile = new MockMultipartFile("file", "jenny.jpg", MediaType.MULTIPART_FORM_DATA_VALUE, new FileInputStream(file));
    mockMvc.perform(
        MockMvcRequestBuilders.
        multipart("/m").
        file(mockFile).
        content(MediaType.MULTIPART_FORM_DATA_VALUE)
    ).andExpect(MockMvcResultMatchers.status().isOk())
        .andDo(MockMvcResultHandlers.print());
}
```

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
    // 进入controller前执行（先添加先调用）
    @Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception { return true; }
    @Override
    // 进入controller后执行（后添加先调用）
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {}
    @Override
    // 整个请求结束执行（后添加先调用）
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

#### json

- Gson
- Jackson
- Json-B

默认Jackson

```java
@JsonComponent
class UserJsonCustom {
    public static class Serializer extends JsonSerializer<User> {
        @Override
        public void serialize(User value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
        }
    }
    public static class Deserializer extends JsonDeserializer<User> {
        @Override
        public User deserialize(JsonParser p, DeserializationContext ctxt) throws IOException, JsonProcessingException {
            return null;
        }
    }
}
```

```java
@JsonComponent
class UserJsonCustom {
    public static class Serializer extends JsonObjectSerializer {
        @Override
        protected void serializeObject(Object value, JsonGenerator jgen, SerializerProvider provider) throws IOException {
        }
    }
    public static class Deserializer extends JsonObjectDeserializer<User> {
        @Override
        protected User deserializeObject(JsonParser jsonParser, DeserializationContext context, ObjectCodec codec, JsonNode tree) throws IOException {
            return null;
        }
    }
}
```

```java
public abstract class JsonObjectSerializer<T> extends JsonSerializer<T> { ... }
public abstract class JsonObjectDeserializer<T> extends JsonDeserializer<T> { ... }
```

# 国际化

- 添加本地化文件

- 配置messageResource 设置本地化资源

  - 资源放在resource/messages或在配置文件spring.messages.basename=xxx

- 解析请求头中的accept-language或者解析url中的locale

  ```java
  public enum LocaleResolver {
      // 配置文件spring.mvc.locale指定语言
      FIXED,
      // 请求头获得语言
      ACCEPT_HEADER
  }
  ```

  ```java
  @ConfigurationProperties("spring.web")
  public class WebProperties {
  	private LocaleResolver localeResolver = LocaleResolver.ACCEPT_HEADER;
  	...
  }
  ```

  通过配置文件spring.mvc.locale-resolver=accept_header指定使用类型

  ```java
  @Bean
  @ConditionalOnMissingBean
  @ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
  public class WebMvcAutoConfiguration {
  	public LocaleResolver localeResolver() {
          // 当配置spring.mvc.locale-resolver=fixed
          if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
              // 就使用spring.mvc.locale的值
              return new FixedLocaleResolver(this.mvcProperties.getLocale());
          }
          AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
          Locale locale = (this.webProperties.getLocale() != null) ? this.webProperties.getLocale()
              : this.mvcProperties.getLocale();
          // spring.mvc.locale作为默认语言
          localeResolver.setDefaultLocale(locale);
          return localeResolver;
      }
      ...
  }
  ```

  ```java
  public class AcceptHeaderLocaleResolver implements LocaleResolver {
  	@Override
  	public Locale resolveLocale(HttpServletRequest request) {
  		Locale defaultLocale = getDefaultLocale();
          // 当没有Accept-Language才用默认Locale
  		if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
  			return defaultLocale;
  		}
  		Locale requestLocale = request.getLocale();
  		List<Locale> supportedLocales = getSupportedLocales();
  		if (supportedLocales.isEmpty() || supportedLocales.contains(requestLocale)) {
  			return requestLocale;
  		}
  		Locale supportedLocale = findSupportedLocale(request, supportedLocales);
  		if (supportedLocale != null) {
  			return supportedLocale;
  		}
  		return (defaultLocale != null ? defaultLocale : requestLocale);
  	}
      ...
  }
  ```

- 语言切换

  ```
  @Configuration
  public class WebMvcConfig implements WebMvcConfigurer {
  	@Override
      public void addInterceptors(InterceptorRegistry registry) {
          // url语言拦截器
          registry.addInterceptor(new LocaleChangeInterceptor()).addPathPatterns("/**");
      }
  }
  ```

  ```java
  public class LocaleChangeInterceptor implements HandlerInterceptor {
  	@Override
  	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws ServletException {
          // 获取url中locale的值
          String newLocale = request.getParameter(getParamName());
          if (newLocale != null) {
              if (checkHttpMethod(request.getMethod())) {
                  LocaleResolver localeResolver = RequestContextUtils.getLocaleResolver(request);
                  if (localeResolver == null) {
                      throw new IllegalStateException(
                          "No LocaleResolver found: not in a DispatcherServlet request?");
                  }
                  try {
                      // 将值设置进localeResolver
                      localeResolver.setLocale(request, response, parseLocaleValue(newLocale));
                  }
                  ...
              }
          }
      }
      ...
  }
  ```

  ```java
  LocaleContextHolder是一个Locale持久器，springmvc底层会将localeResolver设置进去
  ```

# 统一异常处理

ErrorMvcAutoConfiguration

```java
public class ErrorMvcAutoConfiguration {
    
	@Bean
	@ConditionalOnMissingBean(value = ErrorAttributes.class, search = SearchStrategy.CURRENT)
	public DefaultErrorAttributes errorAttributes() { ... }
    
    @Bean
	@ConditionalOnMissingBean(value = ErrorController.class, search = SearchStrategy.CURRENT)
	public BasicErrorController basicErrorController(ErrorAttributes errorAttributes, ObjectProvider<ErrorViewResolver> errorViewResolvers) { ... }
    // 解析错误视图
    @Bean
    @ConditionalOnBean(DispatcherServlet.class)
    @ConditionalOnMissingBean(ErrorViewResolver.class)
    DefaultErrorViewResolver conventionErrorViewResolver() { ... }
}
```

ErrorMvcAutoConfiguration包含3个主要Bean，DefaultErrorAttributes、BasicErrorController、DefaultErrorViewResolver

```java
public class BasicErrorController extends AbstractErrorController {
    // 网页请求
	@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        ...
        // 使用了DefaultErrorViewResolver解析
        odelAndView modelAndView = resolveErrorView(request, response, status, model);
        ...
    }
    // ajax请求
    @RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) { ... }
}
```

BasicErrorController包含两个映射接口，分别处理网页和ajax请求异常

```java
public abstract class AbstractErrorController implements ErrorController {
	protected ModelAndView resolveErrorView(HttpServletRequest request, HttpServletResponse response, HttpStatus status, Map<String, Object> model) {
        ...
    	ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
        ...
	}
}
```

```java
/*
 * For example, an {@code HTTP 404} will search (in the specific order):
 * <ul>
 * <li>{@code '/<templates>/error/404.<ext>'}</li>
 * <li>{@code '/<static>/error/404.html'}</li>
 * <li>{@code '/<templates>/error/4xx.<ext>'}</li>
 * <li>{@code '/<static>/error/4xx.html'}</li>
 * </ul>
 */
public class DefaultErrorViewResolver implements ErrorViewResolver, Ordered {
	@Override
	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
		ModelAndView modelAndView = resolve(String.valueOf(status.value()), model);
        ...
	}
    
    private ModelAndView resolve(String viewName, Map<String, Object> model) {
		String errorViewName = "error/" + viewName;
        // 在templates里找
		TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName,
				this.applicationContext);
		if (provider != null) {
			return new ModelAndView(errorViewName, model);
		}
        // 在static里找
		return resolveResource(errorViewName, model);
	}
	...
}
```

# Servlet容器

#### Servlet容器配置修改

- 全局配置文件修改

- WebServerFactoryCustomizer修改

  ```java
  @Component
  public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
  
      @Override
      public void customize(ConfigurableWebServerFactory server) {
          server.setPort(8000);
      }
  }
  ```

#### 注册servlet三大容器

servlet、filter、listener

以servlet为例

```java
@WebServlet(urlPatterns = "/ScanServlet")
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.println("hello ScanServlet");
    }
}
@SpringBootApplication
@ServletComponentScan// 加上才会扫描到
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

或

```java
public class BeanServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.println("hello BeanServlet");
    }
}
@Configuration
public class MyWebMvcConfigurer {
    @Bean
    public ServletRegistrationBean myServlet() {
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean();
        servletRegistrationBean.setServlet(new BeanServlet());
        servletRegistrationBean.addUrlMappings("/BeanServlet");
        return servletRegistrationBean;
    }
}
```

#### 三大容器

tomcat(默认)、jetty、undertow

#### Servlet自动配置

```java
@Configuration(proxyBeanMethods = false)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
// 依赖任意servlet容器
@ConditionalOnClass(ServletRequest.class)
@ConditionalOnWebApplication(type = Type.SERVLET)
// 启用server.xxx自动注入
@EnableConfigurationProperties(ServerProperties.class)
// 让所有WebServerFactoryCustomizer一一调用
@Import({ ServletWebServerFactoryAutoConfiguration.BeanPostProcessorsRegistrar.class,
		ServletWebServerFactoryConfiguration.EmbeddedTomcat.class,
		ServletWebServerFactoryConfiguration.EmbeddedJetty.class,
		ServletWebServerFactoryConfiguration.EmbeddedUndertow.class })
public class ServletWebServerFactoryAutoConfiguration {
    @Bean
	public ServletWebServerFactoryCustomizer servletWebServerFactoryCustomizer(ServerProperties serverProperties, ObjectProvider<WebListenerRegistrar> webListenerRegistrars) {
        ...
	}
}
```

```java
public class ServletWebServerFactoryCustomizer implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>, Ordered {
}
```

WebServerFactoryCustomizer

```java
public static class BeanPostProcessorsRegistrar implements ImportBeanDefinitionRegistrar, BeanFactoryAware {
    // 使用registerBeanDefinitions注册Bean
	@Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        if (this.beanFactory == null) {
            return;
        }
        registerSyntheticBeanIfMissing(registry, "webServerFactoryCustomizerBeanPostProcessor",
                                       WebServerFactoryCustomizerBeanPostProcessor.class,
                                       WebServerFactoryCustomizerBeanPostProcessor::new);
        registerSyntheticBeanIfMissing(registry, "errorPageRegistrarBeanPostProcessor",
                                       ErrorPageRegistrarBeanPostProcessor.class,
                                       ErrorPageRegistrarBeanPostProcessor::new);
    }
}
```

```java
public class WebServerFactoryCustomizerBeanPostProcessor implements BeanPostProcessor, BeanFactoryAware {
    @Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if (bean instanceof WebServerFactory) {
			postProcessBeforeInitialization((WebServerFactory) bean);
		}
		return bean;
	}
}
```

# spring boot启动原理

调用SpringApplication.run启动springboot应用

```java
SpringApplication.run(DemoApplication.class, args);
```

```java
public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
    return new SpringApplication(primarySources).run(args);
}
```

**创建SpringApplication对象**

```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
    this.resourceLoader = resourceLoader;
    Assert.notNull(primarySources, "PrimarySources must not be null");
    // 将启动类放入primarySources
    this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
    // 根据classpath下的类，推断当前web应用类型(webFlux,servlet)
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.bootstrappers = new ArrayList<>(getSpringFactoriesInstances(Bootstrapper.class));
    // 去spring.factories中去获取所有key为org.springframework.context.ApplicationContextInitializer的值
    setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    // 去spring.factories中去获取所有key为org.springframework.context.ApplicationListener的值
    setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    // 根据main方法推算出mainApplicationClass
    this.mainApplicationClass = deduceMainApplicationClass();
}
```

总结：

1. 获取启动类
2. 获取web应用类型
3. 读取对外扩展的ApplicationContextInitialier，ApplicationListener
4. 根据main推算出所在的类

**运行run方法**

```java
public ConfigurableApplicationContext run(String... args) {
    // 记录springboot启动耗时
    StopWatch stopWatch = new StopWatch();
    stopWatch.start();
    // 它是任何spring上下文的接口，所以可以接受任何ApplicationContext实现
    DefaultBootstrapContext bootstrapContext = createBootstrapContext();
    ConfigurableApplicationContext context = null;
    configureHeadlessProperty();
    // 去spring.factories中去获取SpringApplicationRunListener的组件，用来发布事件
    SpringApplicationRunListeners listeners = getRunListeners(args);
    // 1.发布ApplicationStartingEvent事件
    listeners.starting(bootstrapContext, this.mainApplicationClass);
    try {
        // 根据命令行参数实例化ApplicationArguments
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        // 预初始化环境，读取环境变量，读取配置文件信息
        ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        // 忽略实现BeanInfo的Bean
        configureIgnoreBeanInfo(environment);
        // 打印Banner横幅
        Banner printedBanner = printBanner(environment);
        // 根据webApplicationType创建Spring上下文
        context = createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        // 预初始化上下文
        prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        // 加载spring ioc容器，由于是使用
        refreshContext(context);
        afterRefresh(context, applicationArguments);
        stopWatch.stop();
        if (this.logStartupInfo) {
            new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
        }
        listeners.started(context);
        callRunners(context, applicationArguments);
    }
    catch (Throwable ex) {
        handleRunFailure(context, ex, listeners);
        throw new IllegalStateException(ex);
    }

    try {
        listeners.running(context);
    }
    catch (Throwable ex) {
        handleRunFailure(context, ex, null);
        throw new IllegalStateException(ex);
    }
    return context;
}
```

```java
private ConfigurableEnvironment prepareEnvironment(SpringApplicationRunListeners listeners, DefaultBootstrapContext bootstrapContext, ApplicationArguments applicationArguments) {
   	// 根据webApplicationType创建环境，创建完就会读取java环境变量和系统环境变量
   	ConfigurableEnvironment environment = getOrCreateEnvironment();
   	// 将命令行参数读取到环境变量
   	configureEnvironment(environment, applicationArguments.getSourceArgs());
   	// 将@PropertieSource的配置信息放在第一位，因为优先级最低
   	ConfigurationPropertySources.attach(environment);
    // 发布了ApplicationEnvironmentPreparedEvent的监听器，读取了全局配置文件
   	listeners.environmentPrepared(bootstrapContext, environment);
   	DefaultPropertiesPropertySource.moveToEnd(environment);
   	configureAdditionalProfiles(environment);
    // 将所有spring.main开头的配置信息绑定到SpringApplication
   	bindToSpringApplication(environment);
   	if (!this.isCustomEnvironment) {
      	environment = new EnvironmentConverter(getClassLoader()).convertEnvironmentIfNecessary(environment,
            deduceEnvironmentClass());
   	}
    // 更新PropertySources
   	ConfigurationPropertySources.attach(environment);
   	return environment;
}
```

```java
private void prepareContext(DefaultBootstrapContext bootstrapContext, ConfigurableApplicationContext context, ConfigurableEnvironment environment, SpringApplicationRunListeners listeners, ApplicationArguments applicationArguments, Banner printedBanner) {
   context.setEnvironment(environment);
   postProcessApplicationContext(context);
    // 拿到之前读取到所有ApplicationContextInitializer的组件，循环调用initialize方法
   applyInitializers(context);
    // 发布了ApplicationContextInitializedEvent
   listeners.contextPrepared(context);
   bootstrapContext.close(context);
   if (this.logStartupInfo) {
      logStartupInfo(context.getParent() == null);
      logStartupProfileInfo(context);
   }
   // 获取当前spring上下文beanFactory(负责创建bean)
   ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
   beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
   if (printedBanner != null) {
      beanFactory.registerSingleton("springBootBanner", printedBanner);
   }
    // 在spring下如果出现两个重名的bean，则后读取到的会覆盖前面的
    // 在springboot在这里设置了不允许覆盖，当出现2个重名的bean会抛出异常
   if (beanFactory instanceof DefaultListableBeanFactory) {
      ((DefaultListableBeanFactory) beanFactory)
            .setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
   }
    // 设置当前所有的bean是否懒加载
   if (this.lazyInitialization) {
      context.addBeanFactoryPostProcessor(new LazyInitializationBeanFactoryPostProcessor());
   }
   // Load the sources
   Set<Object> sources = getAllSources();
   Assert.notEmpty(sources, "Sources must not be empty");
    // 读取主启动类，因为后续要根据配置类解析配置的所有bean
   load(context, sources.toArray(new Object[0]));
    // 发布ApplicationPreparedEvent
   listeners.contextLoaded(context);
}
```

总结：

1. 初始化SpringApplication从spring.factories读取listener ApplicationContextInitializer
2. 运行run方法
3. 读取环境变量 配置信息
4. 创建springApplication上下文：ServletWebServerApplicationContext
5. 预初始化上下文：读取启动类
6. 调用refresh加载ioc容器
   1. 加载所有的自动配置类
   2. 创建servlet容器
7. 在这个过程中springboot会调用很多监听器对外进行拓展

# Spring MVC和Spring Boot

Spring MVC提供了web开发工具，但配置复杂，是基于Spring的一个MVC框架

Spring Boot遵循约定大于配置，提供了默认配置，是基于Spring的一套快速开发整合包