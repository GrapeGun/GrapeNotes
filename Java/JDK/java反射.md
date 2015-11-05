javad的反射机制
===
###1.Class类
任何一个类都是Class类的实例对象,其构造方法为私有,只能由JVM来创建.Class的实例对象有三种表示方式:

```java
public class ClassDemo{
    public static void main(String[] args){
        // 普通类创建实例
        Foo foo1 = new Foo();
        // Class类实例的表示方式:
        // 1.任何一个类都有一个隐含的静态成员变量class
        Class c1 = Foo.class;
        // 2.已知该类的对象,通过对象的getClass()方法
        Class c2 = foo1.getClass(); 
        // 3. Class.forName("类的全称")方法加载类
        Class c3 = null;
        c3 = Class.forName("package.path.Foo")
        // 说明 c1 == c2 返回true c2 == c3 返回true;
        
        // 通过Class类的实例,可以创建Foo类的实例
        Foo foo = (Foo)c1.newInstance();
        // 上述代码中会抛出异常
    }
}

class Foo{}
```

>类也是对象,是Class类的实例对象,我们将这个对象成为该类的类类型(Class Type)

###2.Class类的动态加载
+ 编译时加载类 : 静态加载类
+ 运行时加载类 : 动态加载类

###3.类的信息
####3.1 类类型
```java
// obj 是Object类的实例
// getClass()是一个native修饰的方法,会使用JNI调用C++代码
Class c = obj.getClass(); // 若obj是Object,那么c就是Object的类类型,若是Object类的子类,c便是Object子类的类类型
// 类类型的全名称
String cName = c.getName();
```
####3.2 Method类
Method 是方法的对象,每一个成员方法都是一个Method对象.
+ getMethods() 方法,获取类中所有的public方法,包括从父类继承而来的方法
+ getDeclaredMethods()获取的是所有该类自己声明的方法(不管访问权限)
```java
// 继续上面的代码
Method[] ms = c.getMethods();   // 获取所有包括父类的public方法
// 打印方法名
for(int i=0; i<ms.length;i++){
    // 返回方法的返回值类型的类类型
    Class returnType = ms[i].getReturnType();
    // 返回方法名
    ms[i].getName();
    // 获取参数类型,得到参数列表个参数类型的类类型
    Class[] paramTypes = ms[i].getParameterTypes();
}
```
####3.3 Field类
Field 是成员变量的对象
+ getFields() 获取所有的public成员变量的信息(同样包含父类)
+ getDeclaredFields() 获取该类自身声明的成员变量(忽略访问权限)
```java
// 继续上面的代码
// 得到该类的所有自己声明的成员变量
Field[] fs = c.getDeclaredFields();

for(Field f : fs){
    // 得到成员变量的类型的类类型名
    Class fieldType = f.getType();
    String typeName = fieldType.getName();
    // 得到成员变量的名
    String fieldName = f.getName();
}
```
####3.4 Constructor类
Constructor 类是构造方法的对象
+ getConstructors() 获得所有的public构造方法
+ getDeclaredConstructors() 获取所有自己声明的构造方法
```java
Constructor[] cs = c.getDeclaredConstructors();
for(Constructor c : cs){
    // 获取构造方法的参数类型
    Class[] paramTypes = c.getParameterTypes();
}
```

###4.方法反射的基本操作
基本语法:`method.invoke(对象,参数列表);`
如:
```java
// A是一个class 其中包含一个public void print(int a ,int b)
A a = new A();
Class c = a.getClass();
// print是方法名,此处会抛出异常
Method m = c.getDeclaredMethod("print",
                new Class[]{int.class,int.class});
// 或
m = c.getDeclaredMethod("print",int.class,int.class);
// 原来的操作方式
a.print(10,20);
// 方法的反射操作,即使用Method的实例对象来进行方法操作
// 方法如果有返回值返回具体值,如果没有返回null
Object o = m.invoke(a,new Object[]{10,20});
// 或
o = m.invoke(a,10,20);
```

###5.反射与集合泛型
(艹,重点来了)通过Class,Method来了解下泛型的本质....首先看下面的例子
```java
ArrayList list = new ArrayList();
ArrayList<String> list1 = new ArrayList<String>();
list1.add("hello");
// list1.add(20); 编译出错
Class c1 = list.getClass();
Class c2 = list1.getClass();
System.out.println(c1 == c2);
```
这段代码将会输出true,这说明 ***编译后集合的泛型是去泛型化的***
>结论:Java中集合的泛型主要用来防止错误输入的,只在编译阶段有效,绕过编译后无效

下面这段代码将绕过编译
```java
try{
        // 这段代码会绕过编译,都是在运行是操作的..我去太叼了
        Method m = c2.getMethod("add",Object.class);
        m.invoke(list1,100); // 这样就不能foreach了
        System.out.println(list1.size); // 将会输出2
    } catch(Exception e){
        e.printStackTrack();
    }
```