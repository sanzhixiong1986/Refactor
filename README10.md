重构

#### 第十章 简化函数

------

#### 函数名的重要性

范例修改旧函数

```java
public String getTelephoneNumber(){
  return ("(+"+_officeAreaCode+")"+_officeNumber);
}
```

说明：从这个函数当中我们可以看到这个是一个获取本地座机的电话，这个有可能是办公室的电话，有可能是家里的电话，所以我们要给这个电话加上一个可以区分的名字，这样别人在使用的时候会非常的清楚。

```java
class Persion{
  public String getTelephoneNumber(){
    return getOfficeTelephoneNumber();
  }
  //新函数
  public String getOfficeTelephoneNumber(){
    return ("(+"+_officeAreaCode+")"+_officeNumber);
  }
}
```

只是一个函数名的改变，对我们的使用者来说，我们调用者就大概知道函数的具体操作。让别人用起来也很方便，第一个函数的名字虽然也可以让我们知道是得到电话，但是具体的电话是什么类型的可能就不那么清楚了，这个就是函数名字的魅力。

### 函数的参数添加

```java
public int add(){
  return a+b;
}
```

- 添加一个函数名字和原来的名字一样

```java
public int add(int a,int b){
  return a+b;
}
```

然后用旧函数调用新函数,其实这也是函数重载的一种方式，把所有的函数a引用改成b就可以删除a了

```java
public int add(){
	return add(a,b);
}
```

### 函数减少参数

#### 动机：因为很多的程序员只喜欢增加参数，很少会去减少参数，并且一般就算不用的参数也不会对函数造成什么影响，所以都不会去做这个事情。

- 多态的实现函数要例外
- 如果这个函数每一个参数，写这个函数的人都有判断的话，那我们就没有必要在重新做，如果有的话我们就可以重新写一个函数去除去定的参数

#### 它和增加参数差不多的方式进行函数的替换

### 查询函数和修改函数分离

一个函数最好只有一个操作

```java
//有查询和返回这两个操作
String foundMiscreant(String[] people){
  for(int i=0;i<people.length;i++){
    if(people[i].equals("Don")){
      sendAlert();
      return "Don";
    }
    if(people[i].equals("John")){
      sendAlert();
      return "John";
    }
  }
  return "";
}

//调用它的函数
void checkSecurity(String[] people){
  String found = foundMiscreant(people);
  someLaterCode(found);
}
```

### 将查询和设置分开

1.新创建一个方法,把要修改的方法copy进来

```java
String foundPersion(String people){
  for(int i=0;i<people.length;i++){
    if(people[i].equals("Don")){
      return "Don";
    }
    if(people[i].equals("John")){
      return "John";
    }
  }
  return "";
}
```

2.在原来的方法里面加入我们新的方法

```java
String foundMiscreant(String[] people){
  for(int i=0;i<people.length;i++){
    if(people[i].equals("Don")){
      sendAlert();
      //这里也跳用的我们的新的方法
      return foundPersion(people);;
    }
    if(people[i].equals("John")){
      sendAlert();
      //这里也跳用的我们的新的方法
      return foundPersion(people);
    }
  }
  return foundPersion(people);
}

//然后我改一下调用
void checkSecurity(String[] people){
  //这一步是做了一个替换
  foundMiscreant(people);
  String found = foundPersion(people);
  someLaterCode(found);
}
```

3.替换完成就可以把返回值给变成void

```java
void foundMiscreant(String[] people){
  for(int i=0;i<people.length;i++){
    if(people[i].equals("Don")){
      sendAlert();
      return
    }
    if(people[i].equals("John")){
      sendAlert();
      return
    }
  }
}

```

这样我们看到把一个有返回值的，变成了没有返回值函数。

4.修改改名的名字，并且整理函数

```java
void sendAlert(String[] people){
  if(!foundPersion(people).equals(" "))
    	sendAlert();
}
```

------

## 令函数携带参数

### 适用：有些函数做的事情差不多，就要提出相同的地方

### 做法：

- 新建一个函数，使用替换先前的重复函数
- 编译
- 将调用旧函数的代码改为调用新函数
- 编译
- 对所有旧函数重复上面过程

### 范例

```java
class Employee {
  void tenPercentRaise(){
    salary *= 1.1;
  }
  
  void fivePercentRaise(){
    salary *=1.05;
  }
}
```

新建一个函数共同的地方操作,合并

```java
void raise(double factor){
  salary *= (1 + factor);
}
```

------

### 复杂的代码提取

```java
protected Dollars baseCharge(){
  double result = Math.min(lastUsage(),100) * 0.03;
  if(lastUsage()>100)
    result += (Math.min(lastUsage(),200)-100)*0.05;
  if(lastUsage()>200)
    result += (lastUsage() - 200) * 0.07;
  return new Dollars(result);
}
```

修改后

```java
protected Dollars baseCharge(){
  double result = usageInRange(0,100)*0.03;
  result += usageInRange(100,200) * 0.05;
  result += usageInRange(200,Integer.MAX_VALUE) * 0.07;
  return new Dollars(result);
}

protected int usageInRange(int start,int end){
  if (lastUsage() > start) return Math.min(lastUsage(),end) - start;
  else return 0;
}
```

------

