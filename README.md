# dsh-anime-theme

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Platform](https://img.shields.io/badge/DeepSeek%20Harness-0.1.7%2B-blue)](https://github.com/deepseek-ai/deepseek-harness)

**DeepSeek Harness（dsh）二次元主题插件** —— 深色模式下的全屏动漫壁纸 + 半透明玻璃面板 + 中性深灰调色 + 右下角随机动图挂件，带主题面板一键开关。

基于 dsh **官方插件机制**（`dsh.client` 声明 + bundle patch）构建，对上游零修改；官方更新不影响本插件，主题失效时可一键恢复原生界面。

## ✨ 功能

- **全屏动漫壁纸**：预置 12 张壁纸，支持自定义多张（可增删、单独删除）
- **玻璃拟态面板**：半透明毛玻璃卡片 surface
- **中性深灰调色**：基于设计平台 token 的整套配色
- **随机动图挂件**：右下角随机挂件，预置 8 套（间谍过家家 / Lycoris Recoil），支持自定义套装与缩略图管理
- **主题面板**：桌面端标题栏按钮唤出，切换预览 / 壁纸 / 浓度 / 挂件套装 / 挂件大小 / 总开关
- 设置存 `localStorage`，实时生效、刷新与重启后保留

## 📦 安装

### 方式一：官方桌面版「添加插件」（推荐）

打开 DeepSeek 桌面版 → 插件管理 → **添加插件** → 输入仓库地址：

```
https://github.com/huantest2024/dsh-anime-theme
```

点击「安装」即可。

### 方式二：CLI

```bash
dsh plugin --profile <你的profile> install https://github.com/huantest2024/dsh-anime-theme
```

### 方式三：手动部署

```bash
git clone https://github.com/huantest2024/dsh-anime-theme.git
# 复制到 dsh 主目录的 profiles/node_modules/ 下，例如：
cp -r dsh-anime-theme ~/.dsh/profiles/node_modules/
```

然后在 `~/.dsh/profiles/<你的profile>/package.json` 的 `dsh.profile.bundles` 数组中加入 `"dsh-anime-theme"`，重启 dsh。

## 🎨 自定义素材

往素材目录丢文件后重新构建即可：

```bash
assets/wallpapers/      # 放入 .jpg / .png → 新壁纸
assets/anime/<套装名>/  # 放入 .gif → 新挂件套装
node build.mjs          # 重新生成 lib/client.js（素材以 base64 内嵌）
```

生成的新壁纸与套装会自动出现在主题面板里。

## 🧩 兼容性

| dsh 版本 | 状态 |
|---|---|
| 官方桌面版 0.2.0-rc.2 | ✅ 实测通过 |
| 0.1.7-rc.2（CLI / Web） | ✅ 实测通过 |
| 0.1.1-rc.2（旧壳时代） | ✅ 历史验证 |

> dsh 官方声明 0.1.x 存在破坏性变更。本插件只依赖官方公开契约（`dsh.client` 声明、bundle patch、`__ModuleLoader__` 注入）；若上游大版本升级导致主题失效，面板内「关闭主题（恢复原生）」可随时回到原生界面，不影响 dsh 本体使用。

## 🔧 工作原理

- `package.json` 的 `dsh.client` 声明浏览器端插件（`platform: web`，`inject: ["@deepseek-ai/dsh-client-modules"]`）
- `cordis.patch.yml` 通过官方 bundle patch 机制把插件挂进 cordis 插件树
- `lib/client.js`（由 `client-src.js` + 素材经 `build.mjs` 生成）在浏览器端经 `__ModuleLoader__.load({id, factory})` 注册，全部视觉增强均为旁路 CSS + 自有 DOM 节点，不修改、不劫持上游任何组件与数据流

## ⚠️ 素材版权声明

`assets/` 下的动漫壁纸与挂件动图为粉丝制作的装饰性素材，原作版权归各自权利方所有（间谍过家家 / Spy×Family、Lycoris Recoil 等）。仅供个人界面美化使用，**请勿商用或再分发素材本身**。插件代码部分以 MIT 协议开源（见 [LICENSE](./LICENSE)）。

## License

Code: [MIT](./LICENSE) · Bundled media assets: © their respective copyright holders, personal use only.
