JDBC

Java DateBase Connectivity（Java语言连接数据库）

# 连接

```java
public class J {
  /*
   * 方式四  优化方式三
   */
  @Test
  public void t4() {
    Connection conn = null;
    try {
      // 1。提供三个基本信息
      String url = "jdbc:mysql://localhost:3306/test?serverTimezone=UTC";
      String user = "root";
      String password = "wasd32100";
      // 2。加载驱动
      Class.forName("com.mysql.cj.jdbc.Driver"); // mysql驱动中可以省略这行代码，默认会执行
      // 3。连接数据库
      conn = DriverManager.getConnection(url, user, password);
      System.out.println(conn);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

  /*
   * 方式五  配置文件  最终版
   */
  @Test
  public void t5() {
    try {
      // 读取配置文件中的基本信息
      InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties"); // 默认src目录
      Properties properties = new Properties();
      properties.load(is);

      String user = properties.getProperty("user");
      String password = properties.getProperty("password");
      String url = properties.getProperty("url");
      String driverClass = properties.getProperty("driverClass");
      // 加载驱动
      Class.forName(driverClass);
      // 连接
      Connection conn = DriverManager.getConnection(url, user, password);
      System.out.println(conn);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

}
```



# prepareStatement

## 增

```java
@Test
public void t() {
  PreparedStatement ps = null;
  Connection conn = null;
  try {
    InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties"); // 默认src目录
    Properties properties = new Properties();
    properties.load(is);

    String user = properties.getProperty("user");
    String password = properties.getProperty("password");
    String url = properties.getProperty("url");
    String driverClass = properties.getProperty("driverClass");
    // 加载驱动
    Class.forName(driverClass);
    // 连接
    conn = DriverManager.getConnection(url, user, password);

    // 预编译SQL语句
    String sql = "insert into customers (name,email,birth) values (?,?,?)"; // ?占位符
    ps = conn.prepareStatement(sql);
    // 填充占位符
    ps.setString(1, "萧炎");
    ps.setString(2, "xiaoyan@gmail.com");
    ps.setDate(3, java.sql.Date.valueOf("2003-01-01"));
    // 执行操作
    ps.execute();
  } catch (Exception e) {
    e.printStackTrace();
  } finally {
      // 关闭资源
    if (ps != null) {
      try {
        ps.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
    if (conn != null) {
      try {
        conn.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
  }
}
```



## 改

```java
  /*
   * 改  使用封装的util
   */
  @Test
  public void t2() {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
      // 1。获取数据库的连接
      conn = JDBCUtils.getConnection();
      // 2。预编译sql语句，返回PreparedStatement的实例
      String sql = "update customers set name = ? where id = ?;";
      ps = conn.prepareStatement(sql);
      // 3。填充占位符
      ps.setObject(1, "莫扎特");
      ps.setObject(2, 18);
      // 4。执行
      ps.execute();
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      // 5。资源关闭
      JDBCUtils.closeResource(conn, ps);
    }
  }
```



## 封装连接

```java
package com.java.util;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtils {
  public static Connection getConnection() throws Exception {
    InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties"); // 默认src目录
    Properties properties = new Properties();
    properties.load(is);

    String user = properties.getProperty("user");
    String password = properties.getProperty("password");
    String url = properties.getProperty("url");
    String driverClass = properties.getProperty("driverClass");
    // 加载驱动
    Class.forName(driverClass);
    // 连接
    Connection conn = DriverManager.getConnection(url, user, password);
    return conn;
  }

  // 关闭资源
  public static void closeResource(Connection conn, Statement ps) {
    if (ps != null) {
      try {
        ps.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
    if (conn != null) {
      try {
        conn.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
  }
  
  // 关闭select资源
  public static void closeResource(Connection conn, Statement ps, ResultSet rs) {
    if (ps != null) {
      try {
        ps.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
    if (conn != null) {
      try {
        conn.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
    if (rs != null) {
      try {
        rs.close();
      } catch (SQLException throwables) {
        throwables.printStackTrace();
      }
    }
  }
}
```



## 封装通用增删改

```java
  public void update(String sql, Object... args) {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
      // 1。获取数据库的连接
      conn = JDBCUtils.getConnection();
      // 2。预编译sql语句，返回PreparedStatement的实例
      ps = conn.prepareStatement(sql);
      // 3。填充占位符
      for (int i = 0; i < args.length; i++) {
        // sql中index从1开始
        ps.setObject(i + 1, args[i]);
      }
      // 4。执行
      ps.execute();
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      // 5。资源关闭
      JDBCUtils.closeResource(conn, ps);
    }
  }
```

### 删

```java
  public void t3() {
    String sql = "delete from customers where id = ?";
    update(sql, 3); // 形参 3 就是 id=3  几个问号sql后写几个写形参
  }
```

### 改

```java
  public void t3() {    
    // 改  order加``包裹，因为在java中属于关键字
    String sql = "update `order` set order_name = ? where order_id = ?";
    update(sql,"DD","2");
  }
```



## 封装通用查

查询哪个表则需要先配置哪个表的javaBean，设置属性get set

### 获取整条记录

```java
  @Test
  public void t() {
    String sql = "select id,name,email from customers where id = ?";
    List<Customer> list = getForList(Customer.class, sql, 12);
    list.forEach(System.out::println);
  }
```

```java
  public <T> List<T> getForList(Class<T> clazz, String sql, Object... args) {
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
      conn = JDBCUtils.getConnection();
      ps = conn.prepareStatement(sql);

      for (int i = 0; i < args.length; i++) {
        ps.setObject(i + 1, args[i]);
      }

      rs = ps.executeQuery();
      // 获取结果集的元数据 :ResultSetMetaData
      ResultSetMetaData rsmd = rs.getMetaData();
      // 通过ResultSetMetaData获取结果集中的列数
      int columnCount = rsmd.getColumnCount();
      //创建集合对象
      ArrayList<T> list = new ArrayList<>();
      while (rs.next()) {
        T t = clazz.newInstance();
        // 处理结果集一行数据中的每一个列:给t对象指定的属性赋值
        for (int i = 0; i < columnCount; i++) {
          // 获取列值
          Object cloumValue = rs.getObject(i + 1);
          // 获取每个列的列名
          String columnLabel = rsmd.getColumnLabel(i + 1);
          // 给t对象指定的columnName属性，赋值为columValue：通过反射
          Field field = clazz.getDeclaredField(columnLabel);
          field.setAccessible(true);
          field.set(t, cloumValue);
        }
        list.add(t);
      }
      return list;
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      JDBCUtils.closeResource(conn, ps, rs);
    }
    return null;
  }
```



### 获取整条记录中某一个数据

```java
@Test
public void t2(){
  String sql1 = "select order_id orderId,order_name orderName from `order` where order_id = ?";
  Order order = getInstance(Order.class, sql1, 1);
  System.out.println(order.getOrderName());
}
```

```java
public <T> T getInstance(Class<T> clazz, String sql, Object... args) {
  Connection conn = null;
  PreparedStatement ps = null;
  ResultSet rs = null;
  try {
    conn = JDBCUtils.getConnection();

    ps = conn.prepareStatement(sql);
    for (int i = 0; i < args.length; i++) {
      ps.setObject(i + 1, args[i]);
    }

    rs = ps.executeQuery();
    // 获取结果集的元数据 :ResultSetMetaData
    ResultSetMetaData rsmd = rs.getMetaData();
    // 通过ResultSetMetaData获取结果集中的列数
    int columnCount = rsmd.getColumnCount();

    if (rs.next()) {
      T t = clazz.newInstance();
      // 处理结果集一行数据中的每一个列
      for (int i = 0; i < columnCount; i++) {
        // 获取列值
        Object columValue = rs.getObject(i + 1);

        // 获取每个列的列名
        // String columnName = rsmd.getColumnName(i + 1);
        String columnLabel = rsmd.getColumnLabel(i + 1);

        // 给t对象指定的columnName属性，赋值为columValue：通过反射
        Field field = clazz.getDeclaredField(columnLabel);
        field.setAccessible(true);
        field.set(t, columValue);
      }
      return t;
    }
  } catch (Exception e) {
    e.printStackTrace();
  } finally {
    JDBCUtils.closeResource(conn, ps, rs);

  }
  return null;
}
```



### 总

调用  文件.getForList  或者  文件.getInstance

```java
public class PreparedStatementTest {
  /*
   * 获取整条记录
   */
  public <T> List<T> getForList(Class<T> clazz, String sql, Object... args) {
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
      conn = JDBCUtils.getConnection();
      ps = conn.prepareStatement(sql);

      for (int i = 0; i < args.length; i++) {
        ps.setObject(i + 1, args[i]);
      }

      rs = ps.executeQuery();
      // 获取结果集的元数据 :ResultSetMetaData
      ResultSetMetaData rsmd = rs.getMetaData();
      // 通过ResultSetMetaData获取结果集中的列数
      int columnCount = rsmd.getColumnCount();
      //创建集合对象
      ArrayList<T> list = new ArrayList<>();
      while (rs.next()) {
        T t = clazz.newInstance();
        // 处理结果集一行数据中的每一个列:给t对象指定的属性赋值
        for (int i = 0; i < columnCount; i++) {
          // 获取列值
          Object cloumValue = rs.getObject(i + 1);
          // 获取每个列的列名
          String columnLabel = rsmd.getColumnLabel(i + 1);
          // 给t对象指定的columnName属性，赋值为columValue：通过反射
          Field field = clazz.getDeclaredField(columnLabel);
          field.setAccessible(true);
          field.set(t, cloumValue);
        }
        list.add(t);
      }
      return list;
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      JDBCUtils.closeResource(conn, ps, rs);
    }
    return null;
  }


  /*
   * 获取记录中某一个数据
   */
  public <T> T getInstance(Class<T> clazz, String sql, Object... args) {
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
      conn = JDBCUtils.getConnection();

      ps = conn.prepareStatement(sql);
      for (int i = 0; i < args.length; i++) {
        ps.setObject(i + 1, args[i]);
      }

      rs = ps.executeQuery();
      // 获取结果集的元数据 :ResultSetMetaData
      ResultSetMetaData rsmd = rs.getMetaData();
      // 通过ResultSetMetaData获取结果集中的列数
      int columnCount = rsmd.getColumnCount();

      if (rs.next()) {
        T t = clazz.newInstance();
        // 处理结果集一行数据中的每一个列
        for (int i = 0; i < columnCount; i++) {
          // 获取列值
          Object columValue = rs.getObject(i + 1);

          // 获取每个列的列名
          // String columnName = rsmd.getColumnName(i + 1);
          String columnLabel = rsmd.getColumnLabel(i + 1);

          // 给t对象指定的columnName属性，赋值为columValue：通过反射
          Field field = clazz.getDeclaredField(columnLabel);
          field.setAccessible(true);
          field.set(t, columValue);
        }
        return t;
      }
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      JDBCUtils.closeResource(conn, ps, rs);

    }
    return null;
  }
}
```



## BLOB

### 增

```java
  /*
   * 插入blob型字段 - 图片
   */
  @Test
  public void t() {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
      conn = JDBCUtils.getConnection();
      String sql = "insert into customers(name,email,birth,photo) values (?,?,?,?)";

      ps = conn.prepareStatement(sql);

      ps.setObject(1, "卞雪婷");
      ps.setObject(2, "bxt@gemail.com");
      ps.setObject(3, "1997-5-26");
      FileInputStream is = new FileInputStream(new File("girl.jpg"));
      ps.setBlob(4, is);

      ps.execute();
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      JDBCUtils.closeResource(conn, ps);
    }
  }
```



### 查

```java
  /*
   * 查询入blob型字段 - 图片
   */
  @Test
  public void testQuery() {
    Connection conn = null;
    PreparedStatement ps = null;
    InputStream is = null;
    FileOutputStream fos = null;
    ResultSet rs = null;
    try {
      conn = JDBCUtils.getConnection();
      String sql = "select id,name,email,birth,photo from customers where id = ?";
      ps = conn.prepareStatement(sql);
      ps.setInt(1, 24);
      rs = ps.executeQuery();
      if (rs.next()) {
        int id = rs.getInt("id");
        String name = rs.getString("name");
        String email = rs.getString("email");
        Date birth = rs.getDate("birth");

        Customer cust = new Customer(id, name, email, birth);
        System.out.println(cust);

        //将Blob类型的字段下载下来，以文件的方式保存在本地
        Blob photo = rs.getBlob("photo");
        is = photo.getBinaryStream();
        fos = new FileOutputStream("g.jpg");
        byte[] buffer = new byte[1024];
        int len;
        while ((len = is.read(buffer)) != -1) {
          fos.write(buffer, 0, len);
        }

      }
    } catch (Exception e) {
      e.printStackTrace();
    } finally {

      try {
        if (is != null)
          is.close();
      } catch (IOException e) {
        e.printStackTrace();
      }

      try {
        if (fos != null)
          fos.close();
      } catch (IOException e) {
        e.printStackTrace();
      }

      JDBCUtils.closeResource(conn, ps, rs);
    }
  }
```



## 批量增

```java
  //批量插入的方式四：设置连接不允许自动提交数据
  @Test
  public void testInsert3() {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
      long start = System.currentTimeMillis();
      conn = JDBCUtils.getConnection();
      //设置不允许自动提交数据
      conn.setAutoCommit(false);

      String sql = "insert into goods(name)values(?)";
      ps = conn.prepareStatement(sql);
      for (int i = 1; i <= 1000000; i++) {
        ps.setObject(1, "name_" + i);
        //1."攒"sql
        ps.addBatch();
        if (i % 500 == 0) {
          //2.执行batch
          ps.executeBatch();
          //3.清空batch
          ps.clearBatch();
        }
      }

      //提交数据
      conn.commit();
      long end = System.currentTimeMillis();

      System.out.println("花费的时间为：" + (end - start));
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      JDBCUtils.closeResource(conn, ps);

    }

  }
```



# 事务

## 通用的增删改(考虑上事务）

需配合封装连接

```java
  public int update(Connection conn, String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
    PreparedStatement ps = null;
    try {
      // 1.预编译sql语句，返回PreparedStatement的实例
      ps = conn.prepareStatement(sql);
      // 2.填充占位符
      for (int i = 0; i < args.length; i++) {
        ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
      }
      // 3.执行
      return ps.executeUpdate();
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      // 4.资源的关闭
      JDBCUtils.closeResource(null, ps);
    }
    return 0;
  }
```



## 转账例子

```java
  @Test
  public void testUpdateWithTx() {
    Connection conn = null;
    try {
      conn = JDBCUtils.getConnection();
      System.out.println(conn.getAutoCommit());//true
      //1.取消数据的自动提交
      conn.setAutoCommit(false);

      String sql1 = "update user_table set balance = balance - 100 where user = ?";
      update(conn, sql1, "AA");

      //模拟网络异常
      // System.out.println(10 / 0);

      String sql2 = "update user_table set balance = balance + 100 where user = ?";
      update(conn, sql2, "BB");

      System.out.println("转账成功");

      //2.提交数据
      conn.commit();

    } catch (Exception e) {
      e.printStackTrace();
      //3.回滚数据
      try {
        conn.rollback();
      } catch (SQLException e1) {
        e1.printStackTrace();
      }
    } finally {

      JDBCUtils.closeResource(conn, null);
    }
  }
```



## 隔离

### 避免脏读

```java
	@Test
	public void testTransactionSelect() throws Exception{
		
		Connection conn = JDBCUtils.getConnection();
		//获取当前连接的隔离级别
		System.out.println(conn.getTransactionIsolation());
		//设置数据库的隔离级别：
		conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
		//取消自动提交数据
		conn.setAutoCommit(false);
		
		String sql = "select user,password,balance from user_table where user = ?";
		User user = getInstance(conn, User.class, sql, "CC");
		
		System.out.println(user);
		
	}
```



# 数据库连接池

## druid 德鲁伊

```java
url=jdbc:mysql://localhost:3306/test?serverTimezone=UTC
username=root
password=wasd32100
driverClass=com.mysql.cj.jdbc.Driver

# 设置初始化连接池数量
initialSize=10
# 同步修改默认连接池数量（默认为8）
maxActive=10
```

```java
  @Test
  public void getConnenction() throws Exception {
    Properties pros = new Properties();

    InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties");
    pros.load(is);

    DataSource source = DruidDataSourceFactory.createDataSource(pros);
    Connection conn = source.getConnection();
    System.out.println(conn);
  }
```



# Apache-DBUtils(CURD)

## 增

```java
  @Test
  public void testInsert() {
    Connection conn = null;
    try {
      QueryRunner runner = new QueryRunner();
      conn = JDBCUtils.getConnection();
      String sql = "insert into customers(name,email,birth) values (?,?,?)";
      runner.update(conn, sql, "吴亦凡", "wyf@gmail.com", "2003-01-01");
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
    JDBCUtils.closeResource1(conn,null,null); // 使用Apache-DBUtils关闭资源
    // JDBCUtils.closeResource(conn, null);
    }
  }
```



## 查

为了效果暂未使用try-catch- finally

```java
  @Test
  public void testQuery() throws Exception {
    QueryRunner runner = new QueryRunner();
    Connection conn = JDBCUtils.getConnection();

    // 查询单条记录
    // String sql = "select id,name,email,birth from customers where id = ?";
    // BeanHandler<Customer> handler = new BeanHandler<>(Customer.class); // Customer JavaBean
    // Customer customer = runner.query(conn, sql, handler, 24);
    // System.out.println(customer);

    // 查询多条记录
    // BeanListHandler<Customer> handler = new BeanListHandler<>(Customer.class); // Customer JavaBean
    // List<Customer> query = runner.query(conn, sql, handler, 24);
    // query.forEach(System.out ::println);

    // 查询单条记录 -- 键值对，可取单个值
    String sql = "select id,name,email,birth from customers where id = ?";
    MapHandler handler = new MapHandler(); // 不需要 JavaBean
    Map<String, Object> query = runner.query(conn, sql, handler, 24);
    System.out.println(query.get("name"));

    // 查询多条记录 -- 键值对，可取单个值
    // String sql = "select id,name,email,birth from customers where id < ?";
    // MapListHandler handler = new MapListHandler(); // 不需要 JavaBean
    // List<Map<String, Object>> query = runner.query(conn, sql, handler, 24);
    // System.out.println(query); // [{name=汪峰, birth=2010-02-02, id=1, email=wf@126.com}, {}...]
    // System.out.println(query.get(0).get("name")); // 汪峰（索引0）

    JDBCUtils.closeResource1(conn,null,null); // 使用Apache-DBUtils关闭资源
    // JDBCUtils.closeResource(conn, null);

  }
```



## 查 - 特殊值

**new ScalarHandler()**

为了效果暂未使用try-catch- finally

```java
  @Test
  public void testOtherQuery() throws Exception {
    QueryRunner runner = new QueryRunner();
    Connection conn = JDBCUtils.getConnection();

    // String sql = "select max(birth) from customers"; // 最大生日值
    String sql = "select count(*) from customers"; // 表中共多少条记录
    ScalarHandler handler = new ScalarHandler();
    Object query = runner.query(conn, sql, handler);

    System.out.println(query);
    
    JDBCUtils.closeResource1(conn, null, null); // 使用Apache-DBUtils关闭资源
    // JDBCUtils.closeResource(conn, null);
    
  }
```

