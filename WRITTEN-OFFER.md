# FFmpeg / LAME 的 LGPL 履约与重新链接要约

本文件是 Magpie 随包媒体组件的**库层面**许可履约说明，也是面向接收者的**书面要约**。应用自身源代码不属于本履约范围。

适用产品与版本：

| 平台 | 随包形式 | 组件 | 链接方式 |
|---|---|---|---|
| macOS | `Contents/Resources/bin/ffmpeg`、`ffprobe` + `Contents/Resources/lib/*.dylib` | FFmpeg 7.1.5、LAME 4.0、Opus 1.6.1、libogg 1.3.6、libvorbis 1.3.7 | 动态链接，可替换 dylib |
| iOS | `Runner.app` 内静态库 | FFmpeg 7.1.5（libavformat/libavcodec/libavutil）、LAME 4.0 | 静态链接 |

两端使用同一份 FFmpeg 源码：`ffmpeg-7.1.5.tar.xz`，SHA-256
`de668509caf9e35e3cd162473441fdb29538c6d96ed080292b3cf9e6fc5d558f`。

## 许可范围

- FFmpeg 与 LAME 按 **LGPL-2.1-or-later** 使用，未启用 GPL / nonfree / version3 组件：
  构建参数固定包含 `--disable-gpl --disable-nonfree --disable-version3`，未启用 x264/x265。
- Opus、libogg、libvorbis 为 BSD 风格许可，随包保留原始 COPYING/NOTICE。
- iOS 库额外关闭网络协议、外部程序与设备/滤镜模块（`--disable-network --disable-programs
  --disable-everything`），只保留本地文件转封装所需的最小集合。
- 二进制实际构建参数记录在 `data/ios-media/<platform>/build-manifest.json` 与
  `ffmpeg -version` 的 `configuration:` 行；这些记录是履约材料的一部分，不是推测。

## 对应源代码的固定下载位置

库层面履约材料长期公开在：

- 固定页面：<https://github.com/pulapulapin/magpie-lgpl-compliance/releases/tag/compliance-ffmpeg-7.1.5>
- 固定资产链接（不含应用源代码）：
  <https://github.com/pulapulapin/magpie-lgpl-compliance/releases/download/compliance-ffmpeg-7.1.5/magpie-lgpl-corresponding-source-7.1.5.tar.xz>

该资产由 `scripts/build_lgpl_source_bundle.py` 离线汇总，包含：

- FFmpeg 7.1.5、LAME 4.0、Opus 1.6.1、libogg 1.3.6、libvorbis 1.3.7 的官方源码归档；
- `scripts/build_ffmpeg_lgpl.sh`、`build_ios_media.py`、`build_ios_audio.py`、
  `build_lgpl_source_bundle.py`、`build_aria2.sh`、`make_universal_tools.sh`；
- 许可原文、`SHA256SUMS`、本要约为 `WRITTEN-OFFER.md`；
- iOS 可重链接交付物（见下节）。

资产内 `SHA256SUMS` 覆盖除自身外的全部文件；发布页同时给出资产整体的 SHA-256。
构建脚本只读取本地已校验归档，**不会自动下载**。更换版本必须同时复核源码、许可、
构建参数与回归结果。

若固定页面因平台原因不可用，索取方式见本文件末尾；我们会把同一份材料通过邮件附件或
下载链接补齐。

## 可复现构建脚本

| 脚本 | 作用 |
|---|---|
| `scripts/build_ffmpeg_lgpl.sh` | 从已校验源码构建 macOS LGPL 版 FFmpeg + 音频编解码库，部署目标 12.0 |
| `scripts/build_ios_media.py` | 构建 iOS 静态 libavformat/libavcodec/libavutil，固定源码 SHA-256 |
| `scripts/build_ios_audio.py` | 构建 iOS 静态 libmp3lame |
| `scripts/make_universal_tools.sh` | 合并 arm64 / x86_64 产物 |
| `scripts/build_lgpl_source_bundle.py` | 汇总对应源码与可重链接对象（履约材料） |

## 可重链接交付物（iOS 静态链接）

iOS 为静态链接，LGPL-2.1 第 6 节要求接收者能够用修改后的库重新链接。已公开交付：

- 两端（`iphoneos`、`iphonesimulator`）的静态库归档 `libavformat.a`、`libavcodec.a`、
  `libavutil.a`、`libmp3lame.a`，以及从这些归档解出的**独立 object 文件**
  （`ios-relink/<platform>/objects/<lib>/*.o`）；
- 对应公开头文件（FFmpeg 7.1.5、LAME 4.0）、`build-manifest.json`、`OBJECTS.txt`；
- 本应用的链接参数 `OTHER_LDFLAGS = -lavformat -lavcodec -lavutil -lmp3lame` 与
  具体链接示例（`ios-relink/link-flags.txt`）。

用法：替换同名的库对象 → 保持上述链接参数 → 用你自己的 Apple 身份重新签名。
本材料不含证书、描述文件或私钥，Apple 平台对动态库与商店分发的限制仍然适用。

**书面要约**：如果上述库对象不足以重新链接 Runner 本体（例如缺少应用侧 object 文件），
可书面索取“应用侧重链接包”。交付内容为应用侧 object 文件与链接所需清单，**不包含应用
源代码**。

- 索取方式：邮件 `839200644@qq.com`，标题写明 “LGPL relink request”，注明产品渠道与版本。
- 交付时限：收到请求后 30 日内，以下载链接或物理介质提供；介质按成本收费，不额外收取
  许可费。
- 有效期：自对应二进制发布之日起 3 年内有效。
- 说明：应用源代码不属于履约范围，不会因本要约而公开。

macOS 为动态链接：删除或替换 `Contents/Resources/lib` 中的同名 dylib 即可换用修改后的库；
替换需保留兼容 ABI 并重新签名应用包（`codesign --force --deep --strict`）。

## 包内随附材料

- macOS：`Contents/Resources/licenses/ffmpeg-COPYING.LGPLv2.1.txt`、lame/opus/ogg/vorbis 的
  COPYING、`Contents/Resources/licenses/FFmpeg-LGPL-OFFER.md`（本文件）、
  `Contents/Resources/THIRD_PARTY_NOTICES.md`。
- iOS：`Runner.app/NOTICE.txt`（含本要约摘要、固定下载位置与索取方式）、
  `FFmpeg-LGPL-2.1.txt`、`FFmpeg-LICENSE.md`。

## 验证与边界

- `scripts/build_lgpl_source_bundle.py --check` 校验源码、对象、许可文本与 SHA-256 是否齐备。
- `scripts/check_min_os.py` 是 macOS 打包硬门禁：随包二进制的 minos 不得高于
  `LSMinimumSystemVersion`，且不得引用包外路径。
- 联系方式目前为个人邮箱；后续切换项目域名邮箱时，会同步更新本文件、固定发布页与包内
  NOTICE/OFFER，旧地址保留转发。
- 尚未验证：HEVC-TS 真实样本、macOS 12 实机启动、一次完整的“改库后重链接 Runner 并签名
  安装”演练、App Store 对静态链接的实际审核结论。上述未验证项不因本文件的存在自动变为通过。
