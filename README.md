# 仓库日志手机App
## 需求
+ 存储商品的过期日期，名字，照片，数量以及条形码。
+ 可以通过摄像头识别条形码，并管理相关的商品。
+ 可以记录商品数量和过期日期提醒，用于提高管理仓库效率。
+ 商品在过期日期的两个星期前就需要提醒。

## 计划
+ 设计数据类型和结构以及存储。
+ 设计Ui。

## 开发日志
> ~~（补充未记录）：在没有学会Kotlin之前，试图通过ai直接快速开发程序，但是由于对该语言不熟，导致程序开发到后期代码逻辑混乱，项目无法持续进行，但是为后续的Ui设计奠定基础~~
### 记录 1：国际化：
res/values路径为默认值资源路径分别存储颜色`colors.xml`，主题`themes.xml`和字符串`strings.xml`。res/values路径下的xml文件全部都不能当成通用 XML 配置文件，不能做 XML变量映射以及复杂结构。  
+ **colors.xml**文件
  ```xml
  <resources>
    <color name="purple_200">#FFBB86FC</color>
  </resources>
  ```
  
+ **strings.xml**文件  
  可以接受的格式有两种以及一个程序名称的默认字符串资源（`app_name`）不仅如此，通过创建`/res/value-(语言编号)`可以实现多种语言的预设，当然values文件是默认语言。假设你写了英语（values-en）和西班牙语（values-es）的语言文件，中文(values-zh)则用values默认创建，根据Android多语言机制，如果无法用户的语言既不是英语也不是西班牙语，则会使用默认values作为当前程序语言。   
  ```xml
  <resources>
    <!-- 默认程序名称 -->
    <string name="app_name">Application</string>

    <!-- 带格式 -->
    <string name="item_count">%1$d items</string>

    <!-- 字符串 -->
    <string name="hint_input">请输入内容</string>
</resources>
  ```

+ **themes.xml**文件
  style标签的parent属性是指定特定的默认系统主题，而四个属性分别对应内容和背景颜色，而On则是触发点击事件后与控件才会有的颜色。
  ```xml
  <resources>
    <style name="Theme.ProductRecorder" parent="Theme.Material3.DayNight.NoActionBar">

      <item name="colorPrimary">@color/blue_500</item>
      <item name="colorOnPrimary">@color/black</item>

      <item name="colorBackground">@color/gray_900</item>
      <item name="colorOnBackground">@color/gray_100</item>
    </style>
  </resources>
  ```
  
> 冷知识：XML 声明`<?xml version="1.0" encoding="utf-8"?>`是可选声明但为什么就只有strings.xml文件经常“没有” 因为最早 Android 官方示例里的 strings.xml它是最常被人手写、复制、翻译的文件少一行：不容易被翻译人员误删，不影响可读性，更不容易引起编码问题。久而久之，成了习惯。而themes / colors 常常“有”？文件更像“工程文件”，基本只由程序员维护，IDE 自动生成 / 重写。  

### 确定数据类型与存储模式
启用room数据库（`./app/build.gradle.kts`）[[参考](https://developer.android.com/training/data-storage/room?hl=zh-cn#kts)]:
```kts
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose)
    //room扩展启用
    alias(libs.plugins.ksp)
}

//...

dependencies {
    implementation(libs.androidx.core.ktx)

    //...
    
    //设置room数据库
    val room_version = "2.8.4"

    implementation("androidx.room:room-runtime:$room_version")
    implementation("androidx.room:room-ktx:$room_version")
    ksp("androidx.room:room-compiler:$room_version")
}
```
