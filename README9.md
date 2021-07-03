# 重构
#### 第八章 重新组织数据

##### 自封字段

问题：类的成员变量需要进行封装吗？

答：需要，这个原因很简单，不光是让类自己可以更加的规范，也是对所有需要使用这个属性的类增加可控性。

代码

```java
private int _low,_high;
boolean includes(int arg){
  return arg >= _low && arg <= _high;
}
```

在这里我们看到了类当中的两个成员变量在函数中的应用就是直接获得，修改成需要获得和设置函数相关

```java
private int _low,_high;
boolean includes(int arg){
  return arg >= getLow() && arg <= getHigh();
}

int getLow(){return _low};
int getHigh(){return _high};
```

##### 总结

因为有设置和获得函数（特别是设置函数可以对设置进行控制，这样不管在是类当中，还是在外部的类都可以进行安全的操作）

------

##### 用对象取代数据

问题：为什么要用对象来取代属性

个人认为：因为如果每一个属性都用一个成员变量进行复制，这个类就可能有很多的成员变量，这样不方便管理，我们可以把适当的成员变量放在一个值对象里面，这样我们既让程序的可读性更高，也让类的空间变的更小。

代码

```java
class Order {
  public Order(String customer){
    _customer = customer;
  }
  
  public String getCustomer(){
    return _customer;
  }
  
  public void setCustomer(String arg){
    _customer = arg;
  }
  
  private String _customer;
}
```

替代变量的代码

```java
class Customer {
  public Customer(String name){
    _name = name;
  }
  
  public String getName(){
    return _name;
  }
  
  private final String name;
}
```

修改后

```java
class Order {
  public Order(String customer){
    _customer = new Customer(customer);
  }
  
  public String getCustomer(){
    return _customer.getName();
  }
  
 	public void setCustomer(String arg){
    _customer = new Customer(arg);
  }
  
  private Customer _customer
}
```

这样我随时可以对多个数据进行读取，也节省了空间。这个是改变一个类有很多的成员变量的操作之一。

------

##### 数组转换成对象

问：为什么要把数组变成对象

个人认为：有可能数组里面的数据有可能不是表达的一个连续的数据，比如数组里面有年纪，姓名，性别这种，可以把这些变成对象

范例

```java
String[] row = new String[3];
row[0] = "lisi";
row[1] = "15";

String name = row[0];
String wins = Integer.parseInt(row[1]);

```

将数组编程对象

```java
class Performance(){}
Performance row = new Performance();
//替换代码
row._data[0] = "liverpool";
row._data[1] = "15";

String name = row._data[0];
int wins = Integer.parseInt(row._data[1]);

```

然后给对象一个取设数值的函数

```java
class Performance {
  private String[] _data = new String[3];
  public String getName(){
    return _data[0];
  }
  public void setName(String name){
    _data[0] = name;
  }
}

//然后修改成
row.setName("Liverpool");
String name = row.getName();
//后面的如法炮制就可以
```

##### 使用常量代替魔法数

总结：使用常量代表一些特殊的数据，比如3.14这种,还有就是多个地方运用

```java
double potentialEnergy(double mass,double height){
  return mass * 9.18 * height;
}

//简化后的
double potentialEnergy(double mass,double height){
  return mass * GRAVI_CONSTANT * height;
}

static final double GRAVI_CONSTANT = 9.18;
```

------

##### 数据封装

尽量把成员变量变成私有的，然后通过设置和获取的方式对数据进行操作，避免使用public进行访问。

------

