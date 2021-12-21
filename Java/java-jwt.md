# pom.xml

```xml
<!-- jwt 做Token认证 -->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.18.1</version>
</dependency>
```



# use

```java
  /*
   * 获取 token
   */
  @Test
  public void t() {
    HashMap<String, Object> map = new HashMap<>();

    Calendar instance = Calendar.getInstance(); // 日历类
    instance.add(Calendar.SECOND, 60); // 当前时间添加 60 秒（60秒后）

    String token = JWT.create()
            // .withHeader(map) // header 可以不写，当前值就是默认值
            .withClaim("userId", 21) // payload
            .withClaim("username", "yy")
            .withExpiresAt(instance.getTime()) // 过期时间
            .sign(Algorithm.HMAC256("ziDIngYiMiYao")); // 签名（加密方式（自定义密钥））

    System.out.println(token);

    // token分成三部分
    // eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 -- header
    // .eyJleHAiOjE2Mjk2OTg3MjcsInVzZXJJZCI6MjEsInVzZXJuYW1lIjoieXkifQ -- payload
    // .uUfcmQLuCDCY69989In6TgoLG6dr_cY3CZN9YZ3q_ZI -- sign
  }
```

```java
  /*
   * 验证token
   */
  @Test
  public void t2() {
    // 创建验证对象 （自定义密钥必须一致，常用于读取数据库）
    JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("ziDIngYiMiYao")).build();

    // 验证
    DecodedJWT verify = jwtVerifier.verify("eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2Mjk3MDAxMDIsInVzZXJJZCI6MjEsInVzZXJuYW1lIjoieXkifQ.bY81AcqqwCQhJy0eL-54-wcejV8hlfblHzSp6uyDbPs");

    System.out.println(verify); // com.auth0.jwt.JWTDecoder@625732
    System.out.println(verify.getClaim("userId").asInt()); // 21
    System.out.println(verify.getClaim("username").asString()); // yy
    System.out.println("到期时间：" + verify.getExpiresAt()); // Mon Aug 23 14:28:22 CST 2021
  }
```



# utils 封装

```java
public class JWTUtils {
  private static final String SIGN = "ziDIngYiMiYao";

  /*
   * 生成 token
   */
  public static String getToken(Map<String, String> map) {
    Calendar instance = Calendar.getInstance();
    instance.add(Calendar.DATE, 7); // 默认七天过期

    // 创建 JWT builder
    JWTCreator.Builder builder = JWT.create();

    // payload
    map.forEach((k, v) -> {
      builder.withClaim(k, v);
    });

    // 过期时间、签名
    String token = builder
            .withExpiresAt(instance.getTime())
            .sign(Algorithm.HMAC256(SIGN));

    return token;
  }

  /*
   * 验证 token
   */
  public static DecodedJWT verifyToken(String token) {
    return JWT.require(Algorithm.HMAC256(SIGN)).build().verify(token);
  }
}
```



# 整合 Springboot

## pojo

```java
@Data
public class User {
  private String id;
  private String name;
  private String password;
}
```



## mapper

```java
@Mapper
public interface UserMapper {
  @Select("select * from user_token where name = #{name} and password = #{password}")
  User login(User user);
}
```



## service

```java
@Service
public class UserService implements UserMapper {
  @Autowired
  UserMapper userMapper;

  @Override
  public User login(User user) {
    User login = userMapper.login(user);
    if (login != null) {
      return login;
    }
    throw new RuntimeException("登录失败");
  }
}
```



## utils

```java
public class JWTUtils {
  private static final String SIGN = "ziDIngYiMiYao"; // 加密签名

  /*
   * 生成 token
   */
  public static String getToken(Map<String, String> map) {
    Calendar instance = Calendar.getInstance();
    instance.add(Calendar.DATE, 7); // 默认七天过期

    // 创建 JWT builder
    JWTCreator.Builder builder = JWT.create();

    // payload
    map.forEach((k, v) -> {
      builder.withClaim(k, v);
    });

    // 过期时间、签名
    String token = builder
            .withExpiresAt(instance.getTime())
            .sign(Algorithm.HMAC256(SIGN));

    return token;
  }

  /*
   * 验证 token
   */
  public static DecodedJWT verifyToken(String token) {
    return JWT.require(Algorithm.HMAC256(SIGN)).build().verify(token);
  }
}
```




## interceptors

```java
/**
 * @Description JWT 拦截器
 * @Param
 * @Return
 */
public class JWTInterceptors implements HandlerInterceptor {
  /**
   * @Description 预先处理拦截器
   * @Param
   * @Return
   */
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    HashMap<String, Object> map = new HashMap<>();
    // 获取前端请求头中token
    String token = request.getHeader("token");

    try {
      JWTUtils.verifyToken(token); // 验证token
      return true; // 放行
    } catch (SignatureVerificationException e) {
      e.printStackTrace();
      map.put("msg","无效签名");
    }catch (TokenExpiredException e){
      e.printStackTrace();
      map.put("msg","token已过期");
    }catch (AlgorithmMismatchException e){
      e.printStackTrace();
      map.put("msg","token算法不一致");
    }catch (Exception e){
      e.printStackTrace();
      map.put("msg","token无效");
    }

    // 设置状态
    map.put("state",false);
    // 将 map 转换为 json jackson
    String json = new ObjectMapper().writeValueAsString(map);
    response.setContentType("application/json;charset=UTF-8");
    response.getWriter().println(json);

    return false;
  }
}
```



## controller

```java
@RestController
@Slf4j
public class UserCtrl {
  @Autowired
  UserService userService;

  /*
   * 登录
   */
  @GetMapping("/login")
  public Map<String, Object> login(User user) {
    HashMap<String, Object> map = new HashMap<>();

    try {
      User login = userService.login(user);

      HashMap<String, String> payload = new HashMap<>();
      payload.put("id", new User().getId());
      payload.put("name", new User().getName());
      // 生成JWT
      String token = JWTUtils.getToken(payload);

      map.put("state", true);
      map.put("token", token);
      map.put("msg", "successfully");
      map.put("data", login);
    } catch (Exception e) {
      map.put("state", false);
      map.put("msg", "fail");
    }

    return map;
  }

  /*
   * 验证
   */
  @GetMapping("/test")
  public Map<String, Object> test(String token) {
    HashMap<String, Object> map = new HashMap<>();
    log.info("当前token为:[{}]", token);

    try {
      DecodedJWT verifyToken = JWTUtils.verifyToken(token); // 验证 token
      map.put("state", 200);
      map.put("msg", "successfully");
      return map;
    } catch (Exception e) {
      e.printStackTrace();
    }

    map.put("state", "fail");
    return map;

  }
}
```

