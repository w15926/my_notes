#  application.properties

```properties
# 设置端口号
server.port=8888

# 单个文件最大上传大小 默认1MB
spring.servlet.multipart.max-file-size=10MB
# 整个请求上传所有文件总共大小
spring.servlet.multipart.max-request-size=100MB



# ====================== yml ========================
#spring:
#  mvc:
# 访问路径变成 http://ip:port/res/xxx.jpg
#  static-path-pattern: /res/** # 可能会导致 index.html 不能被默认访问

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
    username: root
    password: wasd32100
    driver-class-name: com.mysql.cj.jdbc.Driver
    
server:
  port: 8081
  servlet:
  # 变成http://localhost:8081/dormitory
    context-path: /dormitory 
```



# pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>springboot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- springboot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <!-- 父项目声明版本，下面子类可以不声明版本，由父项目管控 -->
        <version>2.3.4.RELEASE</version>
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <!-- 自定义某包的版本，进入parent的org.springframework.boot里，再进入，搜索对应包，复制下来修改版本 -->
        <!--<mysql.version>5.1.43</mysql.version>-->
    </properties>

    <dependencies>
        <!-- springboot - web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```



## 安装对应包

> **包**
>
> > 进入parent的org.springframework.boot里，再进入，搜索对应包，抛开`<version>`复制下来
>
> **版本**
>
> > 自定义某包的版本，进入parent的org.springframework.boot里，再进入，搜索对应包，复制版本下来修改版本



## Build

```xml
    <!-- 简化打包部署 jar maven生命周期同时运行clean和package-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

**运行** --> target目录

```shell
java -jar xxxx.jar
```



# Lombok

- @NoArgsConstructor 无参构造器
- @AllArgsConstructor 全参构造器
- @Data 一键生成 get set toString
- @ToString 只生成toString
- @EqualsAndHashCode 重写Equals HashCode
- @Slf4j 控制台生成日志



**pom.xml**

```xml
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```



**java**

- bean

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class User {
  private String username;
  private String password;
}
```

- use

```java
    /*
     * @AllArgsConstructor
     */
    User user = new User("pd", "password");
    System.out.println(user);

    /*
     * @Slf4j
     */
    log.info("请求进来了");
    log.error("我报错咯");
```



# dev-tools

**热部署**

> 安装此插件后重启项目，每次更新代码只要选中 构建->重新编译即可完成热部署

> 注意：默认情况下也有重新编译，但是在没有插件的情况下非热部署

> 真正的热部署用付费的JReble，完全自动

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```



# yaml


**java**

- bean

```java
/*
 * @Component 将Person放置容器中
 * @ConfigurationProperties 将该类和配置文件中哪个前缀绑定
 */
@ConfigurationProperties(prefix = "person")
@Component
@Data
public class Person {
  private String userName;
  private Boolean boss;
  private Date birth;
  private Integer age;
  private Pet pet;
  private String[] interests;
  private List<String> animal;
  private Map<String, Object> score;
  private Set<Double> salarys;
  private Map<String, List<Pet>> allPets;
}
```

- interface

```java
  /*
   * yaml
   * @Autowired 自动注入容器里的内容
   */
  @Autowired
  Person person;

  @RequestMapping("/person")
  public Person person() {
    return person;
  }
```




**application.yml**

```properties
# .yml .yaml 都可以
# .properties和.yml是同时运行的
person:
  user-name: pd
  boss: true
  birth: 2003/01/01
  age: 18
  #  interests: [吃饭,睡觉,调皮] # 数组集合set 或者采用以下写法
  interests:
    - 吃饭
    - 睡觉
    - 调皮
    - 666
  animal: [ 舔狗,狼狗 ]
  #  score: {engilsh:80,math:80} # map 或者采用以下写法
  score:
    english: 80
    math: 80
  salarys:
    - 99.99
    - 99.98
    - 99.97
  pet:
    name: 狗
    weight: 99.99
  all-pets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
    health: [{name: mario,weight: 47}]
```



**pom.xml**

```xml
        <!-- configuration注解 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>

                <!-- 打包的时候忽略yaml配置处理器 -->
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>

            </plugin>
        </plugins>
    </build>
```



# Web开发

## 静态访问

只要是在（根目录resources）

`resources/META-INF/resources`

`resources/public`

`resources/resources`

`resources/static`

目录下

都可以通过`http://ip:port/xxx.jpg`访问得到

**注意：如果访问地址和接口地址一样，则接口地址优先级高**



### 自定义访问路径

application.yml

```yaml
spring:
  mvc:
    # 访问路径变成 http://ip:port/res/xxx.jpg
    static-path-pattern: /res/**
```



## 常用注解

### @GetMapping

### @PostMapping

### @PathVariable

### @RequestHeader

```java
  /*
   * @GetMapping get请求映射
   * @PostMapping post请求映射
   * @PathVariable 路径变量
   * @PathVariable Map<String, String> 获取所有请求参数的键值对（map中key有重复只显示第一个）
   * @RequestHeader 拿到某一个请求头
   */
  @GetMapping("/hello/{id}/pd/{username}")
  // http://localhost:8080/hello/111/pd/胖迪
  public Map<String, Object> getParameter(@PathVariable("id") Integer id,
                                          @PathVariable("username") String name,
                                          @PathVariable Map<String, String> allData,
                                          @RequestHeader("User-Agent") String userAgent,
                                          @RequestHeader Map<String, String> allHeader) {

    HashMap<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", name);
    map.put("allData", allData);
    map.put("User-Agent", userAgent);
    map.put("allHeader", allHeader);

    return map;
  }
```



### @RequestParam

### @CookieValue

```java
  /*
   * @RequestParam 获取请求参数中的 username 并赋值给name
   * @RequestParam("interest") List<String> 获取请求参数中所有的interest值并写入列表List
   * @RequestParam Map<String, String> 获取所有请求参数（map中key有重复只显示第一个）
   * @CookieValue 获取Cookie中某个key的value
   */
  @RequestMapping("/hello")
  // public String hello(@RequestParam("username") String name) {
  //   return name;
  // }

  // public List<String> hello(@RequestParam("interest") List<String> interest) {
  //   return interest; // ["woman", "girl"]
  // }

  public Map<String, Object> hello(@RequestParam Map<String, String> params,
                                   @CookieValue("NMTID") String NMTID,
                                   @CookieValue("NMTID") Cookie cookie) {
    HashMap<String, Object> map = new HashMap<>();
    map.put("params", params);
    map.put("NMTID", NMTID);

    System.out.println(cookie.getName() + "       " + cookie.getValue());

    return map;
  }
```



### @CrossOrigin

### @RequestBody

```java
  /*
   * @CrossOrigin 解决跨越
   * @RequestBody 获取请求体（只有post才有）
   */
  @PostMapping("/post")
  public Map postMethod(@RequestBody String content) {
    HashMap<String, Object> map = new HashMap<>();
    map.put("content", content);
    return map;
  }
```



### @RequestAttribute

```java
@CrossOrigin
@Controller
public class HelloController {

  /*
   * 请求跳转
   * 注意：必须使用 @Controller
   *      跳转的接口必须添加 @ResponseBody
   */
  @GetMapping("/goto")
  // 带参跳转
  public String goTo(HttpServletRequest request) {
    request.setAttribute("msg","请求成功");
    request.setAttribute("code","200");
    return "forward:/success";
  }

  @ResponseBody
  @GetMapping("/success")
  // 接参返回
  public Map success(@RequestAttribute("msg") String msg,
                     @RequestAttribute("code") String code,
                     HttpServletRequest request) {

    Object code1 = request.getAttribute("code"); // 方式一
    HashMap<String, Object> map = new HashMap<>(); // 方式二
    map.put("msg", msg);
    map.put("code",code1);

    return map;
  }
}
```



## 文件上传

```java
    /**
     * MultipartFile 自动封装上传过来的文件
     * @param email
     * @param username
     * @param headerImg
     * @param photos
     * @return
     */
    @PostMapping("/upload")
    public String upload(@RequestParam("email") String email,
                         @RequestParam("username") String username,
                         @RequestPart("headerImg") MultipartFile headerImg,
                         @RequestPart("photos") MultipartFile[] photos) throws IOException {

        log.info("上传的信息：email={}，username={}，headerImg={}，photos={}",
                email,username,headerImg.getSize(),photos.length);

        if(!headerImg.isEmpty()){
            //保存到 H:\\cache
            String originalFilename = headerImg.getOriginalFilename();
            headerImg.transferTo(new File("H:\\cache\\"+originalFilename));
        }

        if(photos.length > 0){
            for (MultipartFile photo : photos) {
                if(!photo.isEmpty()){
                    String originalFilename = photo.getOriginalFilename();
                    photo.transferTo(new File("H:\\cache\\"+originalFilename));
                }
            }
        }


        return "main";
    }
```



### 上传大小限制设置

```properties
# 单个文件最大上传大小 默认1MB
spring.servlet.multipart.max-file-size=10MB
# 整个请求上传所有文件总共大小
spring.servlet.multipart.max-request-size=100MB
```



# 数据访问

### JDBC

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
```



### 驱动

利用SpringBoot提供的版本就可以

```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
<!--            <version>5.1.49</version>-->
        </dependency>
```



### MyBatis

```xml
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
```



#### 注解模式

```java
@Mapper // 里面包含@Component可以在其他地方使用@Autowired
public interface CityMapper {
  @Select("select * from city where id=#{id}")
  public City getById(Long id);
  
  @Insert("insert into city (name, state, country) values (#{name},#{state},#{country}) where id=#{id}")
  @Options(useGeneratedKeys = true, keyProperty = "id")
  public void insert(City city);
}
```



### ⭐️ MyBatis-Plus ⭐️

***MyBatis-Plus 里包含 MyBatis、spring-boot-starter-jdbc，属于 MyBatis 的超集***

*不懂就去官网 🌚*

```xml
        <!-- mybatis-plus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.3.1</version>
        </dependency>
```



```java
// 自动扫描文件夹下的所有目录，不用每个类都加上@Mapper（不推荐）
@MapperScan("com.example.springboot02")
@SpringBootApplication
public class Springboot02Application {

  public static void main(String[] args) {
    SpringApplication.run(Springboot02Application.class, args);
  }

}
```



#### bean提供注解

```java
@Data
public class UserPlus {

  // 禁止此条像数据库中查询（mybatis-plus提供）
  @TableField(exist = false)
  private String username;

  @TableField(exist = false)
  private String password;

  // @TableId(type = IdType.ASSIGN_ID) // 全局唯一 id
  // @TableId(type = IdType.AUTO) // 数据库 id 自增（数据库需同时设置自增）
  private Long id;
  private String name;
  private Integer age;
  private String email;
  private Date createTime;
  private Date updateTime;
}
```



#### mapper

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```



#### 快速入门

```java
  @Autowired
  UserMapper userMapper;

  @Test
  void contextLoads() {
    /*
     * 查询所有用户
     * 查询参数是一个 wrapper（条件构造器），暂时不用（没有条件就是查询所有）
     */
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
  }
```



#### 配置日志

application.properties

```properties
# 配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```



#### CRUD扩展

##### 增

```java
  @Test
  void t2() {
    User user = new User();
    user.setName("pip");
    user.setAge(18);
    user.setEmail("pd@gmail.com");

    int result = userMapper.insert(user);
    System.out.println("user: " + user); // 自动生成 id
    System.out.println(result);
  }
```



##### 改

```java
  @Test
  void t3(){
    User user = new User();
    user.setId(6L); // 要修改的 id（条件）
    user.setName("pp"); // 要修改的 name（值）
    user.setAge(18);

    int i = userMapper.updateById(user);
    System.out.println(i);
  }
```



##### 自动填充

1. 删除数据库字段的默认值
2. 实体类字段增加注解
3. 编写处理器处理



bean

```java
  // 插入时填充
  @TableField(fill = FieldFill.INSERT)
  private Date createTime;

  // 更新插入时填充
  @TableField(fill = FieldFill.INSERT_UPDATE)
  private Date updateTime;
```



新建一个配置文件夹,编写处理器

```java
@Slf4j
@Component // 填充到 Spring 中
public class MyObjectMetaHandler implements MetaObjectHandler {
  // 插入时候的填充策略
  @Override
  public void insertFill(MetaObject metaObject) {
    log.info("start insert fill ....");
    // 老版本
    // this.setFieldValByName("createTime", new Date(), metaObject);
    // this.setFieldValByName("updateTime", new Date(), metaObject);

    // 新版本
    this.strictInsertFill(metaObject, "createTime", () -> LocalDateTime.now(), LocalDateTime.class);
    this.strictInsertFill(metaObject, "updateTime", () -> LocalDateTime.now(), LocalDateTime.class);
  }

  // 更新的时候填充策略
  @Override
  public void updateFill(MetaObject metaObject) {
    log.info("start update fill ....");
    this.strictUpdateFill(metaObject, "updateTime", () -> LocalDateTime.now(), LocalDateTime.class);
  }
}
```



##### 乐观锁

1. 数据库中添加 version 字段，默认值为 1，长度为 10 或者没有
2. 实体类中添加注解，要 mybatis-plus 的注解
3. 注册组件



bean

```java
  @Version
  private Integer version;
```



config

```java
// @MapperScan("com.example.mbp.mapper") // 可以写在这里
// @EnableTransactionManagement // 事务注解
@Configuration // 代表配置类
public class MyBatisPlusConfig {
  @Bean
  public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();

    // 乐观锁
    mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
    // 分页
    // mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));

    return mybatisPlusInterceptor;
  }
}
```



use

```java
  /*
   * 测试乐观锁成功 单线程情况下
   */
  @Test
  void t4() {
    // 1 查询用户信息
    User user = userMapper.selectById(1L);
    // 2 修改用户信息
    user.setName("第一");
    // 3 执行更新操作
    userMapper.updateById(user);
  }
```

```java
  /*
   * 测试乐观锁失败 多线程情况下
   */
  @Test
  void t5(){
    // 线程一
    User user = userMapper.selectById(1L);
    user.setName("first one");
    user.setAge(33);

    // 线程二 模拟另一个线程执行插队操作
    User user2 = userMapper.selectById(1L);
    user2.setName("一一");
    user2.setAge(77);

    // 此时输出线程一，线程二没有执行。如果没有乐观锁则线程二会覆盖线程一
    userMapper.updateById(user);
  }
```



##### 查

```java
  @Test
  void t6() {
    // 单 id 查询
    // User user = userMapper.selectById(1L);
    // System.out.println(user);

    // 多 id 查询
    // List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    // users.forEach(System.out::println);

    // 查询所有用户 （null就是没有条件，所以查询全部）
    // List<User> users = userMapper.selectList(null);
    // users.forEach(System.out::println);

    // 条件查询之 map
    HashMap<String, Object> map = new HashMap<>();
    map.put("id",6);
    map.put("name","pp");

    // 查询 id=6 并且 name=pp 的记录
    List<User> users = userMapper.selectByMap(map);
    System.out.println(users);
  }
```



##### 分页查询

config

```java
@Configuration // 代表配置类
public class MyBatisPlusConfig {
  @Bean
  public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();

    // 乐观锁
    mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
    // 分页
    mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));

    return mybatisPlusInterceptor;
  }
}
```



use

```java
  @Test
  void t7() {
    Page<User> page = new Page<>(1, 3); // 当前页 页面条数
    userMapper.selectPage(page, null);

    System.out.println("-----------------------------------------");
    page.getRecords().forEach(System.out::println);
    System.out.println("一共记录条数为：" + page.getTotal());
    System.out.println("是否有上一页：" + page.hasPrevious());
    System.out.println("是否有下一页：" + page.hasNext());
    System.out.println("当前页为：" + page.getCurrent());
    System.out.println("当前页大小为：" + page.getSize());
    System.out.println("-----------------------------------------");
  }
```



##### 删

> 物理删除
>
> > 从数据库中直接删除
>
> 逻辑删除
>
> > 数据库中并没有真正删除，而是通过一个变量让他失效  delete=0 => delete=1
> >
> > **例如管理员可以看到被删除的记录，类似回收站，可以选择清空回收站（删除数据库）和撤销回收站（修改变量为可用）**



- 物理删除

```java
  @Test
  void t8(){
    // userMapper.delete(null); // 删除全部（wrapper 为 null）

    // userMapper.deleteById(1);

    // userMapper.deleteBatchIds(Arrays.asList(1,2,3,4));

    // HashMap<String, Object> map = new HashMap<>();
    // map.put("id", 6);
    // map.put("name", "pp");
    // userMapper.deleteByMap(map);
  }
```



- 逻辑删除

1. 数据库中建立字段deleted，int类型，默认值为0，长度为1
2. pojo增加注解
3. properties（新版不需要写）



pojo

```java
  @TableLogic
  private Integer deleted;
```

properties

```properties
spring.datasource.username=root
spring.datasource.password=wasd32100
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# mybatis-plus 配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
# 逻辑删除 - 实体类变量名 对应数据库
mybatis-plus.global-config.db-config.logic-delete-field=deleted
# 逻辑删除 - 已删除值 对应数据库
mybatis-plus.global-config.db-config.logic-delete-value=1
# 逻辑删除 - 未删除值 对应数据库
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

use

```java
  @Test
  void t9() {
    // 删除后数据库里自定义的 deleted 字段从 0 变 1
    userMapper.deleteById(123L);

    // 再运行查询所有用户的时候则查不到
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
  }
```



##### 条件构造器

###### 查询 多条件查询

```java
  @Test
  public void t() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    // name、email 不为空的用户并且年龄大于等于 12
    wrapper
            .isNotNull("name")
            .isNotNull("email")
            .ge("age", 12);
    userMapper.selectList(wrapper).forEach(System.out::println);
  }
```



###### 查询 指定值

```java
@Test
public void t2(){
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  wrapper.eq("name","嗷嗷"); // 数据库字段 值
  // User user = userMapper.selectOne(wrapper); // 查询单个
  List<User> users = userMapper.selectList(wrapper); // 查询多个
  System.out.println(users);
}
```



###### 查询 范围查询

```java
  @Test
  public void t3(){
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.between("age", 14, 18);
    // Integer count = userMapper.selectCount(wrapper); // 返回记录数量
    List<User> list = userMapper.selectList(wrapper); // 返回记录
    System.out.println(list);
  }
```



###### 查询 模糊查询

```java
  @Test
  public void t4() {
    // 查询数据库字段 name 中值不包含"王"的所有记录
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
            .notLike("name", "王") // 不包含
            .likeLeft("email", 156) // 以什么开头
            .likeRight("email", ".com"); // 以什么结尾
    List<Map<String, Object>> maps = userMapper.selectMaps(wrapper);
    maps.forEach(System.out::println);

    // 不包含 notLike 包含反之
  }
```



###### 查询 模糊查询 内嵌SQL

```java
@Test
public void t5() {
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  // 数据库字段 值    返回 id 大于 3 的所有 id
  wrapper.inSql("id", "select id from user where id>3");
  List<Object> objects = userMapper.selectObjs(wrapper);
  objects.forEach(System.out::println);
}
```



###### 查询 排序

```java
  @Test
  public void t6() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    // 降序 desc 升序 asc
    wrapper.orderByDesc("id");
    List<User> list = userMapper.selectList(wrapper);
    list.forEach(System.out::println);
  }
```



# MyBatis分页（pagehelper）

```xml
        <!-- MyBatis分页 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.3.1</version>
        </dependency>
```

page

```java
@Data
public class Entity {
  private Integer page;
  private Integer limit = 10;
}
```

Bean

```java
@Data
public class User extends Entity { // 哪个 Bean 需要分页就继承
  // ...
}
```

service

```java
  public PageInfo<User> query(User user) {
    if (user != null && user.getPage() != null) {
      PageHelper.startPage(user.getPage(), user.getLimit());
    }
    PageInfo<User> pageInfo = new PageInfo<>(userMapper.query(user));
    return pageInfo;
  }
```





# 简单流程

- pom.xml略过
- application.properties或者.yml

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
    username: root
    password: wasd32100
    driver-class-name: com.mysql.cj.jdbc.Driver
```



- bean文件夹

```java
@Data
public class City {
  private Long id;
  private String name;
  private String state;
  private String country;
}
```



- mapper接口文件夹

```java
@Mapper // 里面包含@Component可以在其他地方使用@Autowired
public interface CityMapper {
  @Select("select * from city where id=#{id}")
  public City getById(Long id);
  
  @Options(useGeneratedKeys = true, keyProperty = "id", keyColumn = "id")
  @Insert("insert into city (name, state, country) values (#{name},#{state},#{country}) where id=#{id}")
  public void insert(City city);
}
```



- service文件夹

```java
@Service // 里面包含@Component可以在其他地方使用@Autowired
public class CityService {
  @Autowired
  CityMapper cityMapper;

  public City getById(Long id) {
    return cityMapper.getById(id);
  }
}
```



- controller文件夹

```java
@CrossOrigin
@RestController
public class SQLController {
  @Autowired
  CityService cityService;

  @GetMapping("/city")
  public City getCityById(@RequestParam("id") Long id){
   return cityService.getById(id);
  }
}
```

