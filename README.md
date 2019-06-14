# 仓库资源整理
[TOC]

平台|功能|名称
:-:|:-:|:-:
Android|图片加载|[Glide](#glide)

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
### 组件
