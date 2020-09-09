## JavaWEB

- 浏览器：
  - html（结构）
  - css （表现）
  - javaScript （行为）
    - 封装的jquery方法$("#idName")
- 异步请求:
  - ajax
    - $.get(url, data, function(data){})
    - $.post(url, data, function(data){})
  - 以json字符串进行传输，可以利用gson等工具，将json字符串比变为java对象或者将java对象变为json格式的字符串
- 服务器：
  - servlet：
    - ·作为一个将html静态页面和服务器端的java代码连接起来的对象，相当于一个小程序的作用
    - HttpRequest， HttpResponse
    - 四个域对象：
      - pageContext
      - request
      - session
      - application、
    - 转发和重定向的区别
  - jsp：
    - el表达式
    - jstl自定义标签
  - 监听器
  - 过滤器

## Maven:

- Maven相当于一个仓库，管理着项目中的相关jar包
- pom,xml中配置项目依赖
- maven的四个过程
  - clean
  - compile
  - test-compoile
  - package
  - target
  - test
  - install

## Spring:

- IOC容器：
  - pom.xml中配置spring的依赖
  - 在resource中创建applicarion.xml文件，在该文件中添加scan的包文件，在该包下的所有类都将做为IOC容器中的组件
  - 在扫描的包下面的类上添加@Component、@Controller、@Service、@Repository'的注解，使其能被扫描器所注意到
  - 在三层架构中要采用接口和其实现类的方法，可以使用@Autowired进行自动的注入

## SpringMVC

- pom.xml中添加相关的配置文件
- 在resources中创建SpringMvc的全局配置文件，在配置文件中相当于是创建了一个全局的servlet，统一管理所有的请求
- 配置类名和包名以及文件名均相同的mapper配置文件，并在springmvc的配置文件中引入该，mapper配置文件
- @RequestMapping("/hello")