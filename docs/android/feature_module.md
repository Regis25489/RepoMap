# 功能模块
## Glide
> [**Glide**](https://github.com/bumptech/glide) 是一个快速高效的Android图片加载库，注重于平滑的滚动。

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

## Gson
> [**Gson**](https://github.com/google/gson) A Java serialization/deserialization library to convert Java Objects into JSON and back

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
## ButterKnife
> [**ButterKnife**](https://github.com/JakeWharton/butterknife) Bind Android views and callbacks to fields and methods.

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
## OkHttp
> [**OkHttp**](https://github.com/square/okhttp) An HTTP+HTTP/2 client for Android and Java applications. 

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
## Retrofit
> [**Retrofit**](https://github.com/square/retrofit) Type-safe HTTP client for Android and Java by Square, Inc. 

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
## RxAndroid
> [**RxAndroid**](https://github.com/ReactiveX/RxAndroid) RxJava bindings for Android

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
## RxJava
> [**RxJava**](https://github.com/ReactiveX/RxJava) – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM.

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
## Fragmentation
> [**Fragmentation**](https://github.com/YoKeyword/Fragmentation) 为"单Activity ＋ 多Fragment","多模块Activity + 多Fragment"架构而生，简化开发，轻松解决动画、嵌套、事务相关等问题。

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
##  RxPermissions
> [**RxPermissions**](https://github.com/tbruyelle/RxPermissions) Android runtime permissions powered by RxJava2

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
## AndroidUtilCode
> [**AndroidUtilCode**](https://github.com/Blankj/AndroidUtilCode/tree/master/utilcode) 一个强大易用的安卓工具类库

**文档：**<https://github.com/Blankj/AndroidUtilCode/blob/master/utilcode/README-CN.md>

**配置：**
```gradle
dependencies {
   implementation 'com.blankj:utilcode:1.24.2'
   // if u use AndroidX, use the following
   implementation 'com.blankj:utilcodex:1.24.2'
}
```
**使用：**
略
## SlimAdapter
> [**SlimAdapter**](https://github.com/MEiDIK/SlimAdapter) A slim & clean & typeable Adapter without## VIEWHOLDER

**文档：**<https://github.com/MEiDIK/SlimAdapter/blob/master/README.md>

**配置：**
```gradle
dependencies {
   compile 'net.idik:slimadapter:2.1.2'
}
```
**使用：**
```java
SlimAdapter.create()
                .register(R.layout.item_user, new SlimInjector<User>() {
                    @Override
                    protected void onInject(User data, IViewInjector injector) {
                        ...// inject data into views，step 2
                    }
                })
                .register(R.layout.item_interger, new SlimInjector<Integer>() {
                    @Override
                    protected void onInject(Integer data, IViewInjector injector) {
                        ...// inject data into views，step 2
                    }
                })
                .register(R.layout.item_string, new SlimInjector<String>() {
                    @Override
                    protected void onInject(String data, IViewInjector injector) {
                        ...// inject data into views，step 2
                    }
                })
                .registerDefault(R.layout.item_string, new SlimInjector() {
                    @Override
                    protected void onInject(Object data, IViewInjector injector) {
                        ...// inject data into views，step 2
                    }
                })
                .attachTo(recyclerView);
    }
```
## ARouter
> [**ARouter**](https://github.com/alibaba/ARouter) 一个用于帮助 Android App 进行组件化改造的框架 —— 支持模块间的路由、通信、解耦

**文档：**<https://github.com/alibaba/ARouter/blob/master/README_CN.md>

**配置：**
```gradle
android {
    defaultConfig {
        ...
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [**AROUTER_MODULE_NAME: project.getName()**]
            }
        }
    }
}

dependencies {
    // 替换成最新版本, 需要注意的是api
    // 要与compiler匹配使用，均使用最新版可以保证兼容
    compile 'com.alibaba:arouter-api:x.x.x'
    annotationProcessor 'com.alibaba:arouter-compiler:x.x.x'
    ...
}
```
**使用：**
```java
// 在支持路由的页面上添加注解(必选)
// 这里的路径需要注意的是至少需要有两级，/xx/xx
@Route(path = "/test/activity")
public class YourActivity extend Activity {
    ...
}
//初始化SDK
if (isDebug()) {           // 这两行必须写在init之前，否则这些配置在init过程中将无效
    ARouter.openLog();     // 打印日志
    ARouter.openDebug();   // 开启调试模式(如果在InstantRun模式下运行，必须开启调试模式！线上版本需要关闭,否则有安全风险)
}
ARouter.init(mApplication); // 尽可能早，推荐在Application中初始化
// 1. 应用内简单的跳转(通过URL跳转在'进阶用法'中)
ARouter.getInstance().build("/test/activity").navigation();

// 2. 跳转并携带参数
ARouter.getInstance().build("/test/1")
            .withLong("key1", 666L)
            .withString("key3", "888")
            .withObject("key4", new Test("Jack", "Rose"))
            .navigation();
```
## EventBus
> [**EventBus**](https://github.com/greenrobot/EventBus) for Android and Java that simplifies communication between Activities, Fragments, Threads, Services, etc. Less code, better quality.

**文档：**<http://greenrobot.org/eventbus/>

**配置：**
```gradle
dependencies {
   implementation 'org.greenrobot:eventbus:3.1.1'
}
```
**使用：**
```java
//Define events:
public static class MessageEvent { /* Additional fields if needed */ }
//Prepare subscribers: Declare and annotate your subscribing method, optionally specify a thread mode:
@Subscribe(threadMode = ThreadMode.MAIN)  
public void onMessageEvent(MessageEvent event) {/* Do something */};
//Register and unregister your subscriber. For example on Android, activities and fragments should usually register according to their life cycle:
 @Override
 public void onStart() {
     super.onStart();
     EventBus.getDefault().register(this);
 }

 @Override
 public void onStop() {
     super.onStop();
     EventBus.getDefault().unregister(this);
 }
 //Post events:
EventBus.getDefault().post(new MessageEvent());
```
## Logger
> [**Logger**](https://github.com/orhanobut/logger) Simple, pretty and powerful logger for android

**文档：**<https://github.com/orhanobut/logger/blob/master/README.md>

**配置：**
```gradle
dependencies {
   implementation 'com.orhanobut:logger:2.2.0'
}
```
**使用：**
```java
//Initialize
Logger.addLogAdapter(new AndroidLogAdapter());
//And use
Logger.d("hello");
```
## LeakCanary
> [**LeakCanary**](https://github.com/square/leakcanary) A memory leak detection library for Android

**文档：**<https://github.com/square/leakcanary/blob/master/README.md>

**配置：**
```gradle
dependencies {
   debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.0-alpha-2'
}
```
**使用：**
无