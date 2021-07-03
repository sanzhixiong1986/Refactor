# 重构
#### 第九章 简化条件表达式

------

范例

```java
double disabilityAniubt(){
  if(_seniority < 2) return 0;
  if(_monthsDisabled > 12) return 0;
  if(_isPartTime) return 0;
}
```

最直接的操作

```java
double disabilityAniubt(){
  if((_seniority < 2)|| (_monthsDisabled > 12) || (_isPartTime)) return 0;
}
```

提取函数

```java
double disabilityAniubt(){
  if(isNotEligibleForDisability()) return 0;
}

boolean isNotEligibleForDisability(){
  return (_seniority < 2)|| (_monthsDisabled > 12) || (_isPartTime);
}
```

------

#### 使用逻辑与

------

```java
if(onVacation())
  if(lengthOfService() > 10)
    	return 1;
return 0.5;

//合并
if(onVacation() && lengthOfService() > 10) return 1;
else return 0.5;
```

------

#### 合并重复条件片段

范例

```java
if(isSpecialDeal()){
  total = price * 0.95;
  send();
}
else{
  total = price * 0.98;
  send();
}

//修改以后
if(isSpecialDeal())
  total = price * 0.95;
else
  total = price * 0.98;

send();
```

------

#### 以多态取代条件表达式

------

