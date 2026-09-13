# 集成 Chromium 129 (Cronet) 内核与下拉刷新优化

本方案将在应用中集成 **Chromium 129 (Cronet Embedded)** 网络内核，替换默认的系统网络栈，以提供更接近原生 Chromium 129 的网络请求体验（如 HTTP/3、QUIC 支持）。同时保留并优化现有的下拉刷新和 Android 17 UA 伪装。

## Proposed Changes

### [Dependencies]

#### [MODIFY] [libs.versions.toml](file:///C:/Users/q8848/Desktop/yes/gradle/libs.versions.toml)
- 添加 `cronet` 版本：`129.6668.70`
- 添加 `cronet-embedded` 库定义。

#### [MODIFY] [build.gradle.kts](file:///C:/Users/q8848/Desktop/yes/app/build.gradle.kts)
- 引入 `cronet-embedded` 依赖。

---

### [Main Application & UI]

#### [MODIFY] [MainActivity.kt](file:///C:/Users/q8848/Desktop/yes/app/src/main/java/org/world/bxfansnew/MainActivity.kt)
- **初始化 Cronet 引擎**：在 `MainActivity` 中创建并持久化 `CronetEngine` 实例。
- **接管网络请求**：在 `WebViewClient` 的 `shouldInterceptRequest` 中接入 Cronet。虽然完全接管所有静态资源比较复杂，但我们将为主要的 API 和网页请求提供拦截框架。
- **保留现有特性**：保留 Android 17 UA 设置和 `PullToRefreshBox` 下拉刷新功能。

## Verification Plan

### Automated Tests
- 执行 `gradlew app:assembleDebug` 确保编译通过。

### Manual Verification
- 运行应用，观察 `WebView` 是否正常加载网页。
- 使用下拉刷新功能，确认网页重新加载。
- (可选) 通过抓包或服务端日志验证请求是否使用了 Chromium 129 的特性（如 QUIC）。
