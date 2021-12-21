# fastjson

```xml
<!-- https://github.com/alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```



## Basics

```java
    JSONObject jo = new JSONObject();
    jo.put("id",1);
    jo.put("name","pd");
    jo.put("age",18);
    jo.put("birthday",new Date());

    // {"birthday":1629192103703,"name":"pd","id":1,"age":18}
    System.out.println(jo); // 等价于 toString()
    System.out.println(jo.toString());
```



## JSON 数组

```java
     JSONObject json = new JSONObject();
      json.put("22","fastjson");

      JSONObject jo = new JSONObject();
      jo.put("a","a");
      jo.put("b","b");
      jo.put("c","c");

      JSONObject jo2 = new JSONObject();
      jo2.put("1",1);
      jo2.put("2",2);
      jo2.put("3",3);

      json.put("221",jo);

      JSONArray array = new JSONArray();
      array.put(jo);
      array.put(jo2);

      json.put("list",array);

      System.out.println(json);
      // {"22":"fastjson","221":{"a":"a","b":"b","c":"c"},"list":[{"a":"a","b":"b","c":"c"},{"1":1,"2":2,"3":3}]}

      // 输出 json、text 文件
      File f=new File("test.json");
      FileOutputStream fos1=new FileOutputStream(f);
      OutputStreamWriter dos1=new OutputStreamWriter(fos1);
      dos1.write(String.valueOf(coordinate)); // 写入的内容
      dos1.close();
```



## 序列化

### Java对象转成 JSON

```java
  @Test
  public void t() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    student.setEmail("pd.@gmail.com");
    student.setBirthday(getDate());

    // 用 fastjson 转成 JSON
    Object json = JSON.toJSON(student);
    // {"birthday":1629163655502,"name":"pd","id":1,"age":18,"email":"pd.@gmail.com"}
    System.out.println(json);
  }
```



### 集合转成 JSON

```java
  @Test
  public void t2() {
    ArrayList<Student> list = new ArrayList<>();

    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    student.setEmail("pd.@gmail.com");
    student.setBirthday(getDate());

    Student student2 = new Student();
    student2.setId(2);
    student2.setName("pdf");
    student2.setAge(18);
    student2.setEmail("pdf.@gmail.com");
    student2.setBirthday(getDate());

    list.add(student);
    list.add(student2);

    Object json = JSON.toJSON(list);
    // [
    // {"birthday":1629164261146,"name":"pd","id":1,"age":18,"email":"pd.@gmail.com"},
    // {"birthday":1629164261146,"name":"pdf","id":2,"age":18,"email":"pdf.@gmail.com"}
    // ]
    System.out.println(json);
  }
```



### Map转成 JSON

```java
  @Test
  public void t3() {
    HashMap<String, Student> map = new HashMap<>();

    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    student.setEmail("pd.@gmail.com");
    student.setBirthday(getDate());

    Student student2 = new Student();
    student2.setId(2);
    student2.setName("pdf");
    student2.setAge(18);
    student2.setEmail("pdf.@gmail.com");
    student2.setBirthday(getDate());

    map.put("student",student);
    map.put("student2",student2);

    Object json = JSON.toJSON(map);
    // {
    // "student":{"birthday":1629164838542,"name":"pd","id":1,"age":18,"email":"pd.@gmail.com"},
    // "student2":{"birthday":1629164838542,"name":"pdf","id":2,"age":18,"email":"pdf.@gmail.com"}
    // }
    System.out.println(json);
  }
```





## 反序列化

### JSON 转 Java对象

```java
  @Test
  public void t4() {
    String jsonStr = "{\"birthday\":1629163655502,\"name\":\"pd\",\"id\":1,\"age\":18,\"email\":\"pd.@gmail.com\"}";
    Student student = JSON.parseObject(jsonStr, Student.class);
    // Student(id=1, name=pd, age=18, email=pd.@gmail.com, birthday=Tue Aug 17 09:27:35 CST 2021)
    System.out.println(student);
  }
```



### JSON 转 List集合

```java
  @Test
  public void t5() {
    String jsonStr = "[{\"birthday\":1629164261146,\"name\":\"pd\",\"id\":1,\"age\":18,\"email\":\"pd.@gmail.com\"},{\"birthday\":1629164261146,\"name\":\"pdf\",\"id\":2,\"age\":18,\"email\":\"pdf.@gmail.com\"}]";
    List<Student> students = JSON.parseArray(jsonStr, Student.class);
    // [
    // Student(id=1, name=pd, age=18, email=pd.@gmail.com, birthday=Tue Aug 17 09:37:41 CST 2021),
    // Student(id=2, name=pdf, age=18, email=pdf.@gmail.com, birthday=Tue Aug 17 09:37:41 CST 2021)
    // ]
    System.out.println(students);
  }
```



### JSON 转 Map

```java
  @Test
  public void t6() {
    String jsonStr = "{\"student\":{\"birthday\":1629164838542,\"name\":\"pd\",\"id\":1,\"age\":18,\"email\":\"pd.@gmail.com\"},\"student2\":{\"birthday\":1629164838542,\"name\":\"pdf\",\"id\":2,\"age\":18,\"email\":\"pdf.@gmail.com\"}}";
    Map<String, Student> map = JSON.parseObject(jsonStr, new TypeReference<Map<String, Student>>() {
    });
    // {
    // student=Student(id=1, name=pd, age=18, email=pd.@gmail.com, birthday=Tue Aug 17 09:47:18 CST 2021),
    // student2=Student(id=2, name=pdf, age=18, email=pdf.@gmail.com, birthday=Tue Aug 17 09:47:18 CST 2021)
    // }
    System.out.println(map);
  }
```



## 枚举

### WriteMapNullValue

```java
  @Test
  public void t() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    // student.setEmail();
    student.setBirthday(getDate());

    /*
     * 当某一项为 null，fastjson序列化时默认不带那一条参数
     * // student.setEmail(); 注释掉 email 让他为 null，输出时默认不带 email
     * {"birthday":1629172079939,"name":"pd","id":1,"age":18}
     */
    // Object json = JSON.toJSON(student);

    /*
     * 使用此方法会带上为 null 的那一条参数，只是值为null
     * {"age":18,"birthday":1629179071629,"email":null,"id":1,"name":"pd"}
     */
    Object json = JSON.toJSONString(student, SerializerFeature.WriteMapNullValue);

    System.out.println(json);
  }
```



### WriteNullStringAsEmpty

```java
  @Test
  public void t2() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    // student.setEmail();
    student.setBirthday(getDate());

    /*
     * 一
     *   值为 null 那一条参数不显示
     * 二
     *   值为 String 型且为 null，则将值显示 ""
     * {"birthday":1629183064795,"email":"","id":1,"name":"pd"}
     */
    Object json = JSON.toJSONString(student, SerializerFeature.WriteNullStringAsEmpty);
    System.out.println(json);
  }
```



### WriteNullNumberAsZero

```java
  @Test
  public void t3() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    // student.setAge(18);
    // student.setEmail();
    student.setBirthday(getDate());

    /*
     * 一
     *   值为 null 那一条参数不显示
     * 二
     *   值为数值型且为 null，则将值显示为数字 0
     * {"age":0,"birthday":1629181880342,"id":1,"name":"pd"}
     */
    Object json = JSON.toJSONString(student, SerializerFeature.WriteNullNumberAsZero);
    System.out.println(json);
  }
```



### WriteNullBooleanAsFalse

```java
  @Test
  public void t4(){
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);
    // student.setEmail();
    // student.setFlag(true);

    /*
     * 一
     *   值为 null 那一条参数不显示
     * 二
     *   值为布尔型且为 null，则将值显示为 false
     * {"age":18,"birthday":null,"flag":false,"id":1,"name":"pd"}
     */
    Object json = JSON.toJSONString(student, SerializerFeature.WriteNullBooleanAsFalse);
    System.out.println(json);
  }
```



### WriteDateUseDateFormat

### PrettyFormat

```java
  /*
   * WriteDateUseDateFormat
   */
  @Test
  public void t5() {
    Student student = new Student();
    student.setName("pd");
    student.setAge(18);
    student.setBirthday(getDate());

    /*
     * 格式化日期
     * before
     *   {"age":18,"birthday":1629183625631,"name":"pd"}
     * after
     *   {"age":18,"birthday":"2021-08-17 14:59:22","name":"pd"}
     */

    // toJSONString为可变形参，可加入 PrettyFormat 改变输出形式
    // {
    // 	"age":18,
    // 	"birthday":"2021-08-17 15:05:38",
    // 	"name":"pd"
    // }
    Object json = JSON.toJSONString(student, SerializerFeature.WriteDateUseDateFormat, SerializerFeature.PrettyFormat);
    System.out.println(json);
  }
```



## 注解

### @JSONField

***此注解可放置在变量、方法、类上***



#### name

Bean

```java
@Data
public class Student {
  private Integer id;

  @JSONField(name = "user_name")
  private String name;
  private Integer age;
  private String email;
  private Date birthday;
  private Boolean flag;
}
```



Java

```java
  @Test
  public void t() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);

    /*
     * 给 Bean 变量上加上 @JSONField(name = "user_name")，此时输出名字从 name 改成 user_name
     * {"age":18,"id":1,"user_name":"pd"}
     */
    Object json = JSON.toJSONString(student);
    System.out.println(json);
  }
```



#### ordinal

bean

```java
@Data
public class Student {
  private Integer id;

  @JSONField(name = "user_name", ordinal = 1)
  private String name;

  @JSONField(ordinal = 2) // 数值排序越大越往后
  private Integer age;

  private String email;
  private Date birthday;
  private Boolean flag;
}
```



java

```java
  @Test
  public void t() {
    Student student = new Student();
    student.setId(1);
    student.setName("pd");
    student.setAge(18);

    /*
     * {"id":1,"user_name":"pd","age":18}
     */
    Object json = JSON.toJSONString(student);
    System.out.println(json);
  }
```



#### format

bean

```java
@Data
public class Student {
  private Integer id;
  private String name;
  private Integer age;
  private String email;

  @JSONField(format = "YYYY-MM-dd") // 自定义格式
  private Date birthday;

  private Boolean flag;
}
```



java

```java
@Test
public void t() {
  Student student = new Student();
  student.setId(1);
  student.setName("pd");
  student.setAge(18);
  student.setBirthday(getDate());

  /*
   * {"birthday":"2021-08-17","id":1,"user_name":"pd","age":18}
   */
  Object json = JSON.toJSONString(student);
  System.out.println(json);
}
```



#### serialize

bean

```java
@Data
public class Student {
  private Integer id;
  private String name;
  private Integer age;
  private String email;

  @JSONField(serialize = false) // 让此条不转JSON（忽略序列化）
  private Date birthday;

  private Boolean flag;
}
```



java

```java
@Test
public void t() {
  Student student = new Student();
  student.setId(1);
  student.setName("pd");
  student.setAge(18);
  student.setBirthday(getDate());

  /*
   * {"id":1,"user_name":"pd","age":18}
   */
  Object json = JSON.toJSONString(student);
  System.out.println(json);
}
```



#### deserialize

bean

```java
@Data
public class Student {
  private Integer id;
  private String name;
  private Integer age;
  private String email;

  @JSONField(deserialize = false) // 禁止反序列化
  private Date birthday;

  private Boolean flag;
}
```



### @JSONType

***此注解只能放置在类上***



#### includes

bean

```java
@Data
@JSONType(includes = {"id", "name"}) // 自定义要被序列化的字段
public class Person {
  private int id;
  private String name;
  
  @JSONField(serialize = true) // 序列化该字段，此注解优先级低于@JSONType
  private int age;
  
  private String address;
}
```



java

```java
  @Test
  public void t() {
    Person person = new Person();
    person.setId(1);
    person.setAddress("南京");
    person.setName("pd");
    person.setAge(18);

    /*
     * 应该输出四个，现在只输出两个
     * {"id":1,"name":"pd"}
     */
    String jsonString = JSON.toJSONString(person);
    System.out.println(jsonString);
  }
```



#### orders

bean

```java
@Data
@JSONType(includes = {"id", "name"}, orders = {"name", "id"}) // 自定义位置排序
public class Person {
  private int id;
  private String name;
  private int age;
  private String address;
}
```



java

```java
  @Test
  public void t() {
    Person person = new Person();
    person.setId(1);
    person.setAddress("南京");
    person.setName("pd");
    person.setAge(18);

    /*
     * {"name":"pd","id":1}
     */
    String jsonString = JSON.toJSONString(person);
    System.out.println(jsonString);
  }
```

