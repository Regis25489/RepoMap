# 仓库资源整理
[TOC]

平台|功能|名称||平台|功能|名称||平台|功能|名称
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
Android|图片加载|[Glide](#glide)||Android|JSON处理|[Gson](#gson)||Android|View注入|[ButterKnife](#butterknife)
Android|网络请求|[okhttp](#okhttp)||Android|网络请求|[Retrofit](#retrofit)||Android|响应式编程|[RxAndroid](#rxandroid) 
Java|异步编程|[RxJava](#rxjava)||Android|页面框架|[Fragmentation](#fragmentation)||Android|权限管理|[RxPermissions](#rxpermissions)

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
#### [OkHttp](https://github.com/square/okhttp) 
##### An HTTP+HTTP/2 client for Android and Java applications. 
**文档：**<http://square.github.io/okhttp/>

**配置：**
```gradle
dependencies {
  implementation 'com.squareup.okhttp3:okhttp:3.14.2'
}
```
**使用：**
```java
//GET A URL
OkHttpClient client = new OkHttpClient();

String run(String url) throws IOException {
  Request request = new Request.Builder()
      .url(url)
      .build();

  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}
//POST TO A SERVER
public static final MediaType JSON = MediaType.get("application/json; charset=utf-8");

OkHttpClient client = new OkHttpClient();

String post(String url, String json) throws IOException {
  RequestBody body = RequestBody.create(JSON, json);
  Request request = new Request.Builder()
      .url(url)
      .post(body)
      .build();
  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}
```
#### [Retrofit](https://github.com/square/retrofit) 
##### Type-safe HTTP client for Android and Java by Square, Inc. 
**文档：**<https://square.github.io/retrofit/>

**配置：**

```gradle
dependencies {
  implementation 'com.squareup.retrofit2:retrofit:2.6.0'
}
```
**使用：**
```java
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}

Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);

Call<List<Repo>> repos = service.listRepos("octocat");
```
#### [RxAndroid](https://github.com/ReactiveX/RxAndroid) 
##### RxJava bindings for Android
**文档：**<https://github.com/ReactiveX/RxJava/wiki/Getting-Started>

**配置：**
```gradle
dependencies {
  implementation 'io.reactivex.rxjava2:rxandroid:2.1.1'
  implementation 'io.reactivex.rxjava2:rxjava:2.2.9'
}
```
**使用：**
```java
Observable.just(getDrawableFromNet())
        .subscribeOn(Schedulers.newThread())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Consumer<Drawable>() {
            @Override
            public void accept(Drawable drawable) throws Exception {
                ((ImageView)findViewById(R.id.imageView)).setImageDrawable(drawable);
            }
        });
```
#### [RxJava](https://github.com/ReactiveX/RxJava) 
##### RxJava – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM.
**文档：**<https://github.com/ReactiveX/RxJava/wiki/Getting-Started>

**配置：**
```gradle
dependencies {
  implementation 'io.reactivex.rxjava2:rxjava:2.2.9'
}
```
**使用：**

```java
import io.reactivex.functions.Consumer;

Flowable.just("Hello world")
  .subscribe(new Consumer<String>() {
      @Override public void accept(String s) {
          System.out.println(s);
      }
  });
```
#### [Fragmentation](https://github.com/YoKeyword/Fragmentation) 
##### 为"单Activity ＋ 多Fragment","多模块Activity + 多Fragment"架构而生，简化开发，轻松解决动画、嵌套、事务相关等问题。
**文档：**<https://www.jianshu.com/p/d9143a92ad94>

**配置：**
```gradle
dependencies {
  // appcompat-v7包是必须的
  compile 'me.yokeyword:fragmentation:1.3.7'
}
```
**使用：**

```java
public class MainActivity extends SupportActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(...);
      	// Fragmentation is recommended to initialize in the Application
        Fragmentation.builder()
          	 // show stack view. Mode: BUBBLE, SHAKE, NONE
             .stackViewMode(Fragmentation.BUBBLE)
             .debug(BuildConfig.DEBUG)
             ...
             .install();

        if (findFragment(HomeFragment.class) == null) {
            loadRootFragment(R.id.fl_container, HomeFragment.newInstance());  //load root Fragment
        }
    }
```
#### [RxPermissions](https://github.com/tbruyelle/RxPermissions) 
##### Android runtime permissions powered by RxJava2
**文档：**<https://github.com/tbruyelle/RxPermissions/blob/master/README.md>

**配置：**
```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}

dependencies {
    implementation 'com.github.tbruyelle:rxpermissions:0.10.2'
}
```
**使用：**

```java
final RxPermissions rxPermissions = new RxPermissions(this); 
rxPermissions
    .requestEachCombined(Manifest.permission.CAMERA,
             Manifest.permission.READ_PHONE_STATE)
    .subscribe(permission -> { // will emit 1 Permission object
        if (permission.granted) {
           // All permissions are granted !
        } else if (permission.shouldShowRequestPermissionRationale)
           // At least one denied permission without ask never again
        } else {
           // At least one denied permission with ask never again
           // Need to go to the settings
        }
    });
```
### UI组件
