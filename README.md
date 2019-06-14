# 仓库资源整理
[TOC]

平台|功能|名称||平台|功能|名称||平台|功能|名称
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
Android|图片加载|[Glide](#glide)||Android|JSON处理|[Gson](#gson)||Android|View注入|[ButterKnife](#butterknife)

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
#### [ButterKnife](https://github.com/JakeWharton/butterknife) 
##### Bind Android views and callbacks to fields and methods.
**文档：**<http://jakewharton.github.io/butterknife/>

**配置：**

```gradle
android {
  ...
  // Butterknife requires Java 8.
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
}

dependencies {
  implementation 'com.jakewharton:butterknife:10.1.0'
  annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0'
}
```
If you are using Kotlin, replace annotationProcessor with kapt.
To use Butter Knife in a library, add the plugin to your buildscript:
```gradle
buildscript {
  repositories {
    mavenCentral()
    google()
   }
  dependencies {
    classpath 'com.jakewharton:butterknife-gradle-plugin:10.1.0'
  }
}
```
and then apply it in your module:
```gradle
apply plugin: 'com.android.library'
apply plugin: 'com.jakewharton.butterknife'
```
Now make sure you use R2 instead of R inside all Butter Knife annotations.
```java
class ExampleActivity extends Activity {
  @BindView(R2.id.user) EditText username;
  @BindView(R2.id.pass) EditText password;
...
}
```
**使用：**

```java
class ExampleActivity extends Activity {
  @BindView(R.id.user) EditText username;
  @BindView(R.id.pass) EditText password;

  @BindString(R.string.login_error) String loginErrorMessage;

  @OnClick(R.id.submit) void submit() {
    // TODO call server...
  }

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    ButterKnife.bind(this);
    // TODO Use fields...
  }
}
```
### 组件
