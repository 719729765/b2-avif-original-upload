# B2 AVIF Original Upload Support

<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/b2940490-7142-4463-952e-e25a95d3346a" />


**WordPress 6.9.4 / B2 PRO 经典编辑器 AVIF 原图上传兼容插件**

解决 WordPress 后台上传 AVIF 图片时出现：

> Web 服务器无法为该图像生成响应式图像大小。请在上传前将其转换为 JPEG 或 PNG 格式。

本插件针对 WordPress 6.9.4 及 B2 PRO 经典编辑器的媒体库上传流程进行兼容处理，允许上传 AVIF 原图，并跳过 AVIF 图片的服务器端缩略图生成。

## 作者信息

- **作者：** 码铃薯
- **官方网站：** https://www.tudoucode.cn/
- **项目名称：** B2 AVIF Original Upload Support

## 功能特点

- **AVIF 原图保留：** 上传 AVIF 时不转换为 JPEG 或 PNG。
- **保留原始压缩率：** 不重新编码，不改变原始图片文件内容。
- **跳过缩略图生成：** 不为 AVIF 自动生成 thumbnail、medium、large 等中间尺寸。
- **跳过大图缩放：** 避免自动生成 `-scaled` 图片。
- **经典编辑器兼容：** 针对 WordPress 经典编辑器媒体库上传流程进行处理。
- **其他格式不受影响：** JPG、JPEG、PNG、WebP 等图片继续使用 WordPress 原有处理机制。
- **独立 MU 插件：** 不需要修改 B2 PRO 主题文件，降低主题更新造成的代码丢失风险。

## 运行原理

本插件通过 WordPress 提供的过滤器调整 AVIF 上传行为：

| 处理项目 | AVIF | 其他图片 |
|---|---|---|
| 上传 MIME 类型 | 允许 | 原有规则 |
| 服务器解码支持检查 | 尝试放行 | 原有规则 |
| 自动生成缩略图 | 跳过 | 正常生成 |
| 自动生成 scaled 图片 | 跳过 | 正常处理 |
| 原始文件 | 保留 | 正常处理 |

**注意：** 跳过缩略图生成不等于完全绕过图片格式识别。服务器仍需能够读取 AVIF 图片的基本尺寸信息，WordPress 才能正常建立附件元数据。

## 安装方法

### 1. 创建目录

进入 WordPress 网站：

```text
wp-content/
```

如果不存在 `mu-plugins` 文件夹，请手动创建：

```text
wp-content/mu-plugins/
```

### 2. 创建插件文件

在该目录下创建：

```text
b2-avif-original-upload.php
```

### 3. 添加代码

将项目中的 PHP 源代码完整复制到该文件。

### 4. 完成安装

MU 插件会由 WordPress 自动加载，不需要在后台手动启用。

可以进入：

```text
WordPress 后台 → 插件 → 必须使用的插件
```

查看插件是否正常加载。

### 5. 测试上传

进入：

```text
文章编辑器 → 添加媒体 → 上传文件
```

选择 `.avif` 图片进行测试。

## 兼容性

| 项目 | 说明 |
|---|---|
| WordPress | 6.9.4 |
| 主题 | B2 PRO |
| 编辑器 | Classic Editor 经典编辑器 |
| 上传入口 | 经典编辑器媒体库弹窗 |
| PHP | 建议 PHP 7.4 及以上 |
| 图片格式 | AVIF |
| 安装方式 | MU Plugin |

兼容性以实际环境测试为准，不代表所有服务器环境均可直接使用。

## 注意事项

1. 本插件不会把 AVIF 转换成 JPEG 或 PNG。
2. 原始 AVIF 文件不会因为插件处理而重新压缩。
3. AVIF 不生成服务器端缩略图，因此部分主题的缩略图尺寸调用可能直接回退到原图。
4. 如果服务器无法识别 AVIF 文件尺寸，仍可能无法正常建立媒体附件元数据。
5. 插件不会自动修复服务器 GD、Imagick 的 AVIF 解码能力。
6. 已经上传的 AVIF 图片不会自动重新处理。
7. 建议安装前备份网站及数据库。

## 常见问题

### Q1：为什么上传 AVIF 后没有缩略图？

这是预期行为。插件主动跳过 AVIF 中间尺寸生成，以避免服务器图片处理能力不足导致上传失败。

### Q2：会不会降低 AVIF 图片压缩率？

不会。插件不重新编码原始 AVIF 文件。

### Q3：会影响 JPG、PNG、WebP 上传吗？

设计上不会主动修改这些格式的上传和缩略图处理流程。

### Q4：为什么仍然可能上传失败？

服务器需要具备基本的 AVIF 文件尺寸识别能力。如果 `wp_getimagesize()` 无法识别 AVIF，仍需进一步处理服务器环境或附件元数据兼容性。

## 版本记录

### v2.0.0

- 增加 AVIF MIME 注册。
- 增加 AVIF 上传兼容处理。
- 增加经典媒体上传配置处理。
- 禁止 AVIF 自动生成中间尺寸。
- 禁止 AVIF 自动生成 scaled 大图。
- 采用 MU 插件方式安装。
- 保留原始 AVIF 文件。

## 免责声明

本项目仅用于改善 WordPress 图片上传兼容性。

使用前请自行备份网站文件及数据库。不同服务器环境、PHP 扩展及 WordPress 配置可能影响实际效果。

本项目与 WordPress 官方及 B2 PRO 主题开发团队没有官方合作或隶属关系。

---

**作者：码铃薯**

**官方网站：[www.tudoucode.cn](https://www.tudoucode.cn/)**

如果本项目对你有所帮助，欢迎 Star、Fork 和提交 Issue。
