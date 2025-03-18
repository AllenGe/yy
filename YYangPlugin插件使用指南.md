# YYangPlugin 插件使用指南

## 一、插件简介

本插件基于 UniApp ,App离线SDK, 开发原生插件，提供 Facebook SDK 的核心功能集成，支持 iOS 和 Android 双平台：
iOS：最低支持 iOS 13，集成 Facebook SDK 18.0.0（手动集成方式本地加载）
Android：最低支持 API 21，集成 Facebook SDK 18.0.2

- [uni-app 原生语言插件](https://uniapp.dcloud.net.cn/plugin/native-plugin.html)

### 提供以下核心功能：

### 1、SDK初始化

### 2、自动事件记录开关控制

### 3、广告主编号（advertiser-id）收集功能

### 4、自定义事件手动上报 

### 5、iOS 设置 Advertiser Tracking Enabled（支持ios13, ios14+）



## 二、集成步骤

### 1、下载插件 YYangPlugin.zip

iOS：集成 Facebook SDK 18.0.0； Android：集成 Facebook SDK 18.0.2
[YYangPlugin.zip](https://raw.githubusercontent.com/AllenGe/yy/main/YYangPlugin.zip) https://raw.githubusercontent.com/AllenGe/yy/main/YYangPlugin.zip

### 2、解压导入插件 YYangPlugin

放在uni-app项目nativeplugins目录(目录不存在则创建)
![如图所示](https://raw.githubusercontent.com/AllenGe/yy/main/plugin01.jpg)

### 3、配置本地原生插件

在manifest.json -> App原生插件配置 -> 选择本地插件 -> 选择需要打包生效的插件 -> 保存后提交云端打包生效。

![如图所示](https://raw.githubusercontent.com/AllenGe/yy/main/plugin02.jpg)

4、引用，调试
在vue页面或nvue页面引入这个原生插件。 使用uni.requireNativePlugin的api，参数为插件的id。
var yyangModule = uni.requireNativePlugin("YYangPlugin-YYModule")

## 三、方法使用

### 1、SDK 初始化 initSDK

yyangModule.initSDK(params, callback);
参数说明 (params包含以下参数，callback可选)

| 参数名       |   类型 | 是否必填 |                          说明 |
| :----------- | -----: | -------: | ----------------------------: |
| platformName | String |       是 | 平台标识（固定为 'facebook'） |
| appID        | String |       是 |        Facebook上应用的 AppID |
| displayName  | String |       是 |     Facebook上应用的 应用名称 |
| clientToken  | String |       是 |   Facebook上应用的 客户端口令 |

示例代码：

```javascript
const params = {
    platformName: 'facebook',
    appID: process.env.VUE_APP_FB_APP_ID,
    displayName: 'MtUniApp',
    clientToken: process.env.VUE_APP_FB_CLIENT_TOKEN
};
yyangModule.initSDK(params, (ret) => {
    console.log('初始化结果:', ret);
});
```

### 2、自动事件记录控制 callAutoEventsEnabled

yyangModule.callAutoEventsEnabled(params, callback);
参数说明 (params包含以下参数，callback可选)

| 参数名       | 类型    | 是否必填 | 说明                                 |
| ------------ | ------- | -------- | ------------------------------------ |
| platformName | String  | 是       | 平台标识（固定为 'facebook'）        |
| isEnabled    | Boolean | 否       | 默认false，不启用自动记录， true启用 |

示例代码：

```javascript
const params = {
    platformName: 'facebook',
    isEnabled: true
};
yyangModule.callAutoEventsEnabled(params, (ret) => {
    console.log('设置自动事件控制开关:', ret);
});
```

### 3、广告主编号（advertiser-id）收集功能 callAdvertiserIDEnabled

yyangModule.callAdvertiserIDEnabled(params, callback);
参数说明 (params包含以下参数，callback可选)

| 参数名       | 类型    | 是否必填 | 说明                                               |
| ------------ | ------- | -------- | -------------------------------------------------- |
| platformName | String  | 是       | 平台标识（固定为 'facebook'）                      |
| isEnabled    | Boolean | 否       | 默认false，禁用 advertiser-id 收集功能， true 启用 |

示例代码：

```javascript
const params = {
    platformName: 'facebook',
    isEnabled: true
};
yyangModule.callAdvertiserIDEnabled(params, (ret) => {
    console.log('设置advertiser-id 收集功能:', ret);
});
```

### 4、自定义事件上报 callEvent

yyangModule.callEvent(params, callback);
参数说明 (params包含以下参数，callback可选)

| 参数名       |   类型 | 是否必填 |                                                         说明 |
| ------------ | -----: | -------: | -----------------------------------------------------------: |
| platformName | String |       是 |                                平台标识（固定为 'facebook'） |
| methodName   | String |       是 |                                  方法名（一般为 'logEvent'） |
| eventName    | String |       是 |                                                     事件名称 |
| customParams | Object |       否 | [自定义参数](https://developers.facebook.com/docs/app-events/reference#standard-event-parameters-2) |

示例代码：

```javascript
const params = {
    platformName: 'facebook',
    methodName: 'logEvent',
    eventName: 'EventNameClickSomething',
    customParams: {
        'name': '事件记录',
        'type': 'test'
    }
};
yyangModule.callEvent(params, (ret) => {
    console.log('事件上报结果:', ret);
});
```

### 5、 iOS 设置 Advertiser Tracking Enabled（支持ios13, ios14+）

yyangModule.requestTrackingAuth(params, callback);
请求广告追踪授权（仅ios）
参数说明 (callback可选)

示例代码：

```javascript
yyangModule.requestTrackingAuth((ret) => {
    console.log('广告追踪授权结果:', ret);
    //status 对应值：
    //0: NotDetermined    // 未决定
    //1: Restricted       // 受限制
    //2: Denied           // 拒绝
    //3: Authorized       // 授权
    //4: Unavailable      // 系统版本不支持时返回（iOS <14 的情况）
});
```

## 四、插件开发教程

### 1、[Android 插件开发教程](https://nativesupport.dcloud.net.cn/NativePlugin/course/android.html)

### 2、[iOS 插件开发教程](https://nativesupport.dcloud.net.cn/NativePlugin/course/ios.html)

### 3、[Android 原生工程配置](https://nativesupport.dcloud.net.cn/AppDocs/usesdk/android.html)

### 4、[iOS 原生工程配置](https://nativesupport.dcloud.net.cn/AppDocs/usesdk/ios.html)

### 5、[Android 打包发行](https://nativesupport.dcloud.net.cn/AppDocs/package/android.html)

### 6、[iOS 打包发行](https://nativesupport.dcloud.net.cn/AppDocs/package/ios.html)
