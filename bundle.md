# Bundle 的使用

## 简介
Bundle类是一个key-value对，“A mapping from String values to various Parcelable types.”

两个activity之间的通讯可以通过bundle类来实现

## 步骤代码

### 1.new一个bundle类
```java
Bundle bundle=new Bundle();
```
### 2.bundle压入数据
```java
bundle.putString("accessToken",accessToken);
```
### 3.new一个intent对象，并将bundle加入intent中
```java
Intent intent=new Intent();
intent.setClass=(MainActivity.this,target.class);
intent.putExtras(bundle);
//or use 
// intent putExtra("acesstoken",bundle);
```
### 4.在目标class中提取数据
```java
Bundle bundle = getIntent().getExtras();   
String data = bundle.getString("Data");//读出数据
```