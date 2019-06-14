# 仓库资源整理
[TOC]

平台|功能|名称||平台|功能|名称||平台|功能|名称
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
Android|图片加载|[Glide](#glide)||Android|JSON处理|[Gson](#gson)||

## Android
### 功能模块
#### [Glide](https://github.com/bumptech/glide) 
##### Glide是一个快速高效的Android图片加载库，注重于平滑的滚动。
**文档：**<https://muyangmin.github.io/glide-docs-cn/>

**配置：**

```gradle
repositories {
  mavenCentral()
  google()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.9.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.9.0'
}
```
**使用：**

```java
Glide.with(fragment)
    .load(url)
    .into(imageView);
```
#### [Gson](https://github.com/google/gson) 
##### A Java serialization/deserialization library to convert Java Objects into JSON and back
**文档：**<https://github.com/google/gson/blob/master/UserGuide.md/>

**配置：**

```gradle
dependencies {
  implementation 'com.google.code.gson:gson:2.8.5'
}
```
**使用：**

```java
class BagOfPrimitives {
  private int value1 = 1;
  private String value2 = "abc";
  private transient int value3 = 3;
  BagOfPrimitives() {
    // no-args constructor
  }
}

// Serialization
BagOfPrimitives obj = new BagOfPrimitives();
Gson gson = new Gson();
String json = gson.toJson(obj);  

// ==> json is {"value1":1,"value2":"abc"}
```
Note that you can not serialize objects with circular references since that will result in infinite recursion.
```java
// Deserialization
BagOfPrimitives obj2 = gson.fromJson(json, BagOfPrimitives.class);
// ==> obj2 is just like obj
```
### 组件
