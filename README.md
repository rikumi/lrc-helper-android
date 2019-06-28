# lrc-helper-android
网易云音乐歌词助手 for Android O/P

## 简介
- 无需 Root，在通知中心实时显示网易云音乐当前歌词的小插件；
- 通过 DIY，可配合其它通知读取应用，如 **Kustom Widget**、**Automate**，实现将歌词显示到桌面微件、将当前歌词状态发送到第三方服务如 IFTTT 等。

## 截图（本应用不含小部件）
<img width="200" alt="screenshot" src="https://user-images.githubusercontent.com/5051300/60326626-97ba9680-99bc-11e9-879d-a9f042e87c2c.png">

## 使用
- 在网易云音乐中设置通知栏样式为系统通知栏；
- [下载 Release](https://github.com/rikumi/lrc-helper-android/releases/latest)；
- 安装后将会跳转通知读取权限，开启权限后可使用，同时隐藏启动器图标；
- 记得关闭电池优化；如果不慎杀掉了进程，重启手机可以自动启动；
- 开启通知折叠：在 **应用管理 - 歌词助手 - 通知 - 歌词通知 - 行为** 中设置 **无声显示并将重要性级别最小化**，再次打开通知中心即可折叠通知，折叠后只显示一行，展开显示两行。

## 原理
- 每 0.5 秒读取并筛选音乐通知，并通过 MediaSession 读取音乐播放进度；
- 利用网易云 API 搜索匹配的歌词，并使用正则解析保存，根据播放进度更新通知内容；
- 由于网易云 API 限制，若调用过于频繁会触发 IP 反爬机制，约 1 小时之内恢复；但正常聆听音乐过程中一般不会受此影响。

## 配合 Kustom Widget 使用
利用如下插值表达式可读取并实时显示当前歌词；同时需要在 Kustom Widget 中将更新模式设置为 **快速（总是每秒更新）**。

```
$ni(io.github.rikumi.lyrichelper, title, tu(rnd, 1, 1, 1))$
```

由于 Kustom Widget 中读取通知的宏是 **非响应式的**，不会随通知内容实时刷新，因此需要在表达式内通过增加冗余参数（`tu(rnd, 1, 1, 1)`），引入响应式因素，使表达式每秒重算。