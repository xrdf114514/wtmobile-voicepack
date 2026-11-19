# wtmobile-voicepack
# 战雷手游语音包管理器（非官方）

> ⚠️ **非官方第三方工具**：与 Gaijin Entertainment、《战争雷霆 / War Thunder》及其关联方**没有任何关联**，未获其授权或背书。
> 本项目**不提供、不托管、不分发**任何语音包、音效或游戏资源；所有语音文件由用户自备，或来自用户自行添加的第三方订阅源。
> 自制语音依赖修改游戏文件（改包让游戏优先加载 mod），可能违反游戏用户协议，**账号风险由使用者自行承担**。

把自制的 `.zip` / `.bank` 语音包装进《战争雷霆手游》的 `files/sound/mod`，一键换包、按内容去重存储、整库备份迁移；顺带处理 Shizuku 授权、目录一致性检查、诊断导出这些"用起来才会遇到的麻烦"。

纯 framework 实现，无 AndroidX、无 Gradle，APK 约 210KB。

- 应用名：**战雷手游语音包管理器**
- 包名：`com.wtm.voicepack`
- `minSdk 24` / `targetSdk 34`

## 功能

| 模块 | 说明 |
|---|---|
| 权限通道 | Shizuku（反射调用，免 root）> root > Android 9/10 直连；三态引导（未装 / 未运行 / 未授权） |
| 导入 | `.zip` 自动抽 `.bank`（兼容 GBK 中文名）、可多选散 `.bank`；落库前按内容指纹去重 |
| 语音包库 | 详情 / 快捷操作 / 编辑名称备注标签 / 搜索排序收藏 / 多选批量删除、导出、收藏 / 删包内单个 bank |
| 换包 | 先传 staging 并校验大小 → 再清旧 → 再替换，失败不毁旧文件；「恢复原版」一键清空 mod |
| 启动自检 | 每次启动检查 mod 目录：混多个包、陌生文件、内容重复、与「当前生效」记录不一致、增量/减量关系；一致时不打扰 |
| 在线 | 订阅源（自建 `index.json` 或 `github:用户/仓库`）、WT Live 素材库（端游，需移植）、从网盘链接/分享导入 |
| 备份迁移 | 整库打包（可选带 APK）+ 从备份恢复，保留名称/备注/标签/收藏 |
| 诊断 | 崩溃兜底、环境自检、一键导出诊断日志、路径诊断（`ls -l` 原样输出） |

## 使用

1. **给权限**：首页点「获取文件读写权限」。安卓 11+ 需要 [Shizuku](https://shizuku.rikka.app/download/)（免费开源，作者 Rikka）并用无线调试启动，然后在 Shizuku 里允许本应用；已 root 自动走 `su`。
2. **先改包**：自制语音只在游戏被改成「优先加载 mod」后才生效，本软件不改包、也不碰游戏 APK。
3. **导入**：首页「导入语音包」选 zip，或「从链接 / 分享导入」粘网盘链接或直链。
4. **换包**：语音包库点一个包 →「应用到游戏」→ 首页「▶ 启动游戏」（语音不是热加载，必须重进游戏）。
5. **目标目录**：`Android/data/com.gaijingames.wtm/files/sound/mod`（原版语音在 `files/sound/`，注意中间有 `files`）。

## 已知问题

游戏更新后包失效、换机后同一个版本号却不是同一个包、系统拦截安装、旧版 Shizuku 的事务号问题……这些 **App 无法自动修复** 的情况都写在 [docs/已知问题.md](docs/已知问题.md)，遇到问题先看它。

## 构建

没有 Gradle，一条脚本直出 APK：

```bash
bash build.sh
# 产物：dist/战雷手游语音包管理器.apk
```

需要：JDK 17、`aapt`（aapt v1）、`apksigner`、`zipalign`、`android.jar`(platform-34)、`r8.jar`（D8）。
`keystore/` 不存在时脚本会用 `keytool` 自动生成一个，仓库里不附带签名密钥。

## 目录结构

```
app/src/main/AndroidManifest.xml
app/src/main/java/com/wtm/voicepack/   # 纯 Java，无第三方 UI 库
app/src/main/res/                      # 布局 / drawable / 颜色 / 样式
libs/                                  # Shizuku API / provider / aidl / shared（Apache-2.0）
docs/                                  # 使用说明、改包说明、已知问题、免责声明
build.sh                               # 资源 → javac → d8 → zipalign → apksigner
```

## 许可

代码与文档采用 MIT（见 [LICENSE](LICENSE)）；第三方组件许可见 [docs/第三方许可.md](docs/第三方许可.md)。
