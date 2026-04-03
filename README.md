# Flutter 网络请求（dart `http` 包）

## 简介

用官方风格常用的 **`package:http`** 发 GET，配合 `FutureBuilder` 在 `build` 中显示加载圈或返回的响应正文（字符串形式）。侧重展示「不用状态库也能写异步 UI」的最小套路。

## 快速开始

### 环境要求

Flutter SDK；设备能访问公网测试 API。

### 运行

```bash
flutter pub get
flutter run
```

## 概念讲解

### 第一部分：`http.get` 与 `Uri.parse`

`http.get(Uri.parse('https://...'))` 返回 `Future<Response>`，`response.body` 为原始字符串。若要做 JSON，可再用 `dart:convert` 的 `jsonDecode`。

### 第二部分：`FutureBuilder`

`future` 在 StatelessWidget 里直接传入 `fetchData()` 每次 build 会新建 Future（本 Demo 为极简写法）。更严谨做法是把 `Future` 提到 `initState` 或上层缓存；此处重在展示 Future 三态：`connectionState`、`snapshot.hasData`、错误未单独拆分。

## 完整示例

见 `lib/main.dart`：`MyApp` 内联 `fetchData` 与 `FutureBuilder<String>`。

## 注意事项

- 生产环境建议封装 ApiClient、统一错误与超时；并处理 `snapshot.hasError`。
- 需要拦截器、上传下载进度时可迁移到 Dio 等库（可参考本仓库 `flutter-dio-demo`）。

## 完整讲解（中文）

`http` 包像「瑞士军刀里的主刀」：不花哨，但能剪裁大多数 GET/POST。`FutureBuilder` 教你一件重要的事：**异步数据有三态，别只画成功**。本 Demo 为短平快只区分了「有数据」和「加载中」，你接上 `hasError` 就更完整。选 `http` 还是 Dio，往往是团队习惯和工程复杂度问题；从 `http` 入门最直观。
