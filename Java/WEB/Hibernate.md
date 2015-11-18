Hibernate
===
###一 基本知识
####1. ORM(Object/Relationship Mapping)
#####1.1 SQL的缺陷
在程序中经常需要将一个对象进行持久化保存,一般要写很多sql,但是这要有不少缺陷:
+ 不同数据库使用的sql语法不同
+ 同样的功能,在不同的数据库中有不同的实现方式(比如oracle的rownum)
+ 难以移植

#####1.2 Hibernate
是一款开源ORM框架技术,对JDBC进行了轻量级的对象封装.
![Hibernate在程序中的结构位置](http://img.mukewang.com/564096f30001b14b12800720.jpg)

#####1.3 其他ORM技术
+ MyBatis
+ Toplink
+ EJB 重量级ORM框架技术

####2. Hibernate配置文件
#####2.1 hibernate.cfg.xml
```xml
<hibernate-configuration>
  <session-factory>
    <!--基本属性设置-->
    <property name="connection.username">root</property> <!--用户名-->
    <property name="connection.password">root</property> <!--密码-->
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property> <!--连接驱动-->
    <property name="connection.url">jdbc:mysql:///hibernate?useUnicode=true&amp;characterEncoding=UTF-8</property> <!--URL-->
    <property name="dialect">org.hibernate.dialect.MySQLDialect</property> <!--方言-->

    <!--其他设置-->
    <property name="show_sql">true</property>
    <property name="format_sql">true</property>
    <property name="hbm2ddl.auto">create</property>

  </session-factory>
</hibernate-configuration>

```

#####2.2 创建一个学生POJO(或者Entity)
按照创建JavaBean的方法创建
```java
public class Students {

    private int sid;

    private String sname;

    private String gender;

    private Date birthday;

    private String address;

    public Students(){}

    public Students(int sid, String sname, String gender, Date birthday, String address) {
        this.sid = sid;
        this.sname = sname;
        this.gender = gender;
        this.birthday = birthday;
        this.address = address;
    }

    public int getSid() {
        return sid;
    }

    public void setSid(int sid) {
        this.sid = sid;
    }

    public String getSname() {
        return sname;
    }

    public void setSname(String sname) {
        this.sname = sname;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

#####2.3Students类与数据库对象关系映射文件
Students.hbm.xml
```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="com.students.pojo.Students" table="students" schema="" catalog="school">
        <id name="sid" type="int">
            <column name="SID"/>
            <generator class="assigned"/>
        </id>
        <property name="sname" type="java.lang.String">
            <column name="SNAME"/>
        </property>
        <property name="gender" type="java.lang.String">
            <column name="GENDER"/>
        </property>
        <property name="birthday" type="java.util.Date">
            <column name="BIRTHDAY" />
        </property>
        <property name="address" type="java.lang.String">
            <column name="ADDRESS"/>
        </property>
    </class>
</hibernate-mapping>
```

在创建好对象关系映射文件后,需要将该资源在hibernate配置问价中添加mapping如下:
```xml
<mapping class="com.students.pojo.Students"/>
<mapping resource="com/students/pojo/Students.hbm.xml"/>
```

>注意xml文件放置的位置,在idea中,都放在resources文件夹中

####3.Hibernate的基本使用
#####3.1 基本流程
通过Hibernate API编写访问数据库的基本过程是:创建配置对象->创建服务注册对象->创建会话工厂对象->打开会话->打开事务

```java
// 1. 创建配置对象
Configuration config = new Configuration().configure();
// 2. 创建服务注册对象
ServiceRegistry serviceRegistry = new ServiceRegistryBuilder().applySettings(   config.getProperties()).buildServiceRegistry();
// 3. 创建会话工厂对象
SessionFactory sessionFactory = config.buildSessionFactory(serviceRegistry);
// 4. 打开会话
Session session = sessionFactory.openSession();
// 5. 打开事务
Transaction transaction = session.beginTransaction();
// 6. 创建需要保存的对象
Students s = new Students(1,"mingzi","nan");
// 7. 保存
session.save(s);
// 8. 提交事务
transaction.commite();
```

#####3.2 session
可以将session理解为数据库对象,session与jdbc中的connection是一个多对一的关系,每一个session都有一个与之对应的connection,一个connection不同时刻可以供多个session使用.

session中常用的方法:增删改查,save(),update(),delete(),createQuery()等.

#####3.3 获取session对象
1) **openSession(SessionFactory)**
2) **getCurrentSession(SessionFactory)** 该方法需要在hibernate.cfg.xml文件中添加如下属性:
><property name="hibernate.current_session_context_class">thread | jta</property> 其中,thread表示本地事务(jdbc事务) jta是全局事务(jta事务)

3) 两种获取session的区别

+ getCurrentSession在事务提交或者回滚之后会自动关闭,而openSession需要手动关闭
+ openSession每次都是创建新的session对象,getCurrentSession使用现有的session对象



#####3.4 transaction
hibernate对数据的操作适逢装在事务当中的,并且默认是非自动提交的方式,例如,session保存对象时,如果不开启事务,并且手工提交事务,对象并不会真正保存到数据库中.

jdbc的connection是自动提交事务的,其实可以通过session的doWork方法来设置session自动提交事务,但通常不建议这么做.
```java
session.doWork(new Work(){
    @Override
    public void excute(Connection conn) throws SQLException{
        conn.setAutoCommit(true);
    }    
});

// 可能需要添加下面这句
session.flush();
```

#####3.5 hbm配置文件常用设置
```xml
<hibernate-mapping
  schema="schemaName"
  catalog="catalogName"
  default-cascade="cascade_style" //级联风格
  default-access="field | property | ClassName" // 访问策略
  default-lazy="true | false"   // 加载策略
  package="packageName"
/>
<!--类与表对象的对应-->
<class
  name="ClassName"
  table="tableName"
  batch-size="N" // 抓取策略
  where="condition"
  entity-name="EntityName"
/>
<!--主键属性-->
<id
  name="propertyName"
  type="typeName"
  column="columnName"
  length="length"
  <generator class="generatorClass"/>      // 主键生成策略
/>
<!--组件属性,如在学生类中的地址,地址是一个引用类,-->
<component name="address" class="Address">
    <property name="postcode" column="POSTCODE"></property>
    <property name="phone" column="PHONE"></property>
    <property name="address" column="ADDRESS"></property>
</component>

```

主键生成策略,常用的有increment, identity, sequence, native, assigned

+ native 适用于代理主键,根据底层数据库对自动生成标识符的方式,自动选择indentity, sequence,或者hilo
+ assigned 适用于自然主键, 由Java应用程序负责生成标识符

####4. Blob对象
创建以及读取一本Blob对象,同时包括输入输出流操作
```java
@Test
public void writeStudentsImage() throws IOException {
    Students s = new Students();
//        s.setSid(1);
    s.setSname("付国楠");
    s.setBirthday(new Date());
    s.setGender("男");
    s.setAddress("北京haidian");
    // 获取图片
    File f = new File("F:\\CUGB研究生\\2015简历整理\\一寸照_50k.jpg");
    InputStream in = new FileInputStream(f);
    Blob image = Hibernate.getLobCreator(session).createBlob(in,in.available());
    s.setPicture(image);
    session.save(s);
    in.close();
}

@Test
public void readStudentsImage() throws Exception{
    Students s = (Students)session.get(Students.class,1);
    // 获取Blobdui对象
    Blob image = s.getPicture();
    // 获取照片输入流
    InputStream in = image.getBinaryStream();
    // 输出流
    File f = new File("F:\\CUGB研究生\\2015简历整理\\一寸照_java.jpg");
    OutputStream out = new FileOutputStream(f);
    byte[] buffer = new byte[in.available()];
    in.read(buffer);
    out.write(buffer);
    in.close();
    out.close();
}

```

####5.Hibernate增删查改
```java
@Test
public void testGetStudents(){
    Students s = (Students)session.get(Students.class,1);
    System.out.println(s);
}

@Test
public void testLoadGetStudents(){
    Students s = (Students)session.load(Students.class,1);
    System.out.println(s);
}

@Test
public void testUpdateStudents(){
    Students s = (Students)session.load(Students.class,1);
    s.setGender("女");
    System.out.println(s);
    session.update(s);
}

@Test
public void testDeleteStudents(){
    Students s = (Students)session.load(Students.class,1);
    session.delete(s);
//        System.out.println(s);
}

```