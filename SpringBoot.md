 

# 第一个SpringBoot程序

环境：

+ jdk
+ maven
+ SpringBoot
+ idea

```java
//程序的主入口
@SpringBootApplication
public class HelloApplication {

	public static void main(String[] args) {
		SpringApplication.run(HelloApplication.class, args);
	}
//开盖即食
}
```

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String Hello(){
        return "hello,world!";
    }
}
```



# 自动装配原理

**自动配置**：



pom.xml

+ spring-boot-dependencies:依赖在父工程中
+ 在写或引入SpringBoot的依赖时，不需要指定版本



**启动器**

+ ```xml
  <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter</artifactId>
     <version>2.4.5</version>
  </dependency>
  ```



**主程序**

```java
//程序的主入口
//@SpringBootApplication:标注这个类是一个SpringBoot的应用
@SpringBootApplication
public class HelloApplication {

	public static void main(String[] args) {
        //将SpringBoot应用启动
		SpringApplication.run(HelloApplication.class, args);
	}
}
```

+ 注解

  ```java
  @SpringBootConfiguration:springboot配置
      @Configuration：spring配置类
      @Component：spring的组件
  @EnableAutoConfiguration：自动配置
      @AutoConfigurationPackage：自动配置包
  	@Import({AutoConfigurationImportSelector.class})：导入选择器
      	@Import({Registrar.class})
  
  
  ```

# SpringBoot配置

```yaml
# server.port=8080(properties后缀名下）
#server:
#  port: 8080(yaml后缀名下）

#key-value
name: Volunteer

#对象
student:
  name: nameless
  age: 3

#行内写法
human: {name: The_One,age: xxx}

#数组
pets:
  - cat
  - dog
  - fox

animals: [wolf,cat,dragon]
```

yaml可以直接给实体类赋值

```yaml
human:
  name: Volunteer
  age: 19
  happy: true
  birth: 2000/01/01
  map: {k1: v1,k2: v2}
  list:
    - code
    - game
    - loli
  dog:
    name: dog
    age: 3
```

```java
@ConfigurationProperties(prefix = "human")
public class Human {
    private String name;
    private Dog dog;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> map;
    private List<Object> list;
```

SpringBoot的多环境配置：可以选择激活哪一个配置文件

```yaml
spring:
  profiles:
    active: dev
server:
  port: 8081
---

server:
  port: 8082
spring:
  profiles: dev
---

server:
  port: 8083
spring:
  profiles: test
```

# SpringBoot web开发

jar：webapp

自动装配

+ xxxxautoconfiguration：向容器中自动配置组件
+ xxxxProperties：自动配置类，装配配置文件中自定义的一些内容

要解决问题

+ 导入静态资源......
+ 首页
+ jsp，模板引擎Thymeleaf
+ 装配扩展SpringMVC
+ 增删改查
+ 拦截器
+ 国际化（i18n）

## 静态资源

1. 在SpringBoot中，我们可以使用一下方式处理静态资源

   + webjars	localhost：8080/webjars/
+ public，static，/**，resources	localhost：8080/
2. 优先级：resources>static(默认)>public

## 模板引擎

若要使用thymeleaf，只需要导入对应的依赖

```html
<body>
<!--所有的HTML元素都可以被thymeleaf替换接管-->
<div th:text="${msg}">THIS IS A TEST</div>
<div th:utext="${msg}">THIS IS A TEST</div>
<hr>
<div th:each="user:${users}" th:text="${user}"></div>
</body>
```

```java
public String test(Model model){
    model.addAttribute("msg","<h1>hello,springboot</h1>");

    model.addAttribute("users", Arrays.asList("Volunteer","nameless"));
```

## 扩展MVC

```java
//如果想要diy一些定制化的功能，只要写这个组件，然后将它交给SpringBoot，SpringBoot会自动装配
//扩展SpringMVC
@Configuration
public class MyMvcConfigurer implements WebMvcConfigurer {
    //ViewResolver 实现了视图解析器接口的类，我们可以把它看做视图解析器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }
    //自定义一个自己的视图解析器MyViewResolver
    public static class MyViewResolver implements ViewResolver{
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}
```

```java
@EnableWebMvc   //导入了一个类：DelegatingWebMvcConfiguration：从容其中获取所有的webmvcconfig；
```

# TEST



## Index

首页实现

+ 使用注解

```java
@RequestMapping(value = {"/","/index.html"})
public String index(){
    return "index";
}
```

+ MVCConfig

```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/").setViewName("index");
    registry.addViewController("/index.html").setViewName("index");
}
```

`Notice`：所有静态资源都需要使用Thymeleaf接管，使用@{}

`页面国际化`：

1. 配置i18n文件
2. 如果需要在项目中用按钮自动切换，需要自定义一个组件LocaleResolver
3. 将自己写的组件配置到spring容器，@Bean
4. #{}

## 登录+拦截器

登录

```java
@Controller
public class LoginController {
    @RequestMapping("/login")
    public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model, HttpSession session){
        if (!StringUtils.isEmpty(username)&&"123456".equals(password)){
            session.setAttribute("loginUser",username);
            return "redirect:/main.html";
        }else {
            model.addAttribute("msg","用户名或密码错误");
            return "index";
        }
    }
}
```

拦截器

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        Object loginUser = request.getSession().getAttribute("loginUser");

        if (loginUser==null){
            request.setAttribute("msg","没有权限，请登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else {
            return true;
        }
    }
}
```



拦截器设置

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("/index.html","/","/login","/css/*","/js/*","/img/*");
}
```



