# 《边狱公司》蜘蛛巢 食指 父辈 李箱（10115）ChatGPT 主题视觉替换 Mod

> **Limbus Company — ChatGPT-Themed Yi Sang (`10115` Index Father) Visual Skin Pack**  
> **作者 / Author**: `skadi00147` | **适配游戏端**: PC Steam 官方客户端（Unity `6000.3.12f1`）

本仓库开源了 **「蜘蛛巢 食指 父辈 李箱（`10115`）」ChatGPT 主题视觉外观资产**，并随包附带一套极简、开源透明的本地命令行安装与一键无损还原工具。  
*（注：本仓库**仅包含 `10115` 皮肤资产与极简 CLI 安装/还原器**；不包含内部创作端可视化校准工具。）*

---

## 🚀 懒人最推荐：复制一句话，让你的 AI 帮你全自动安装！

如果手头正在用 **Antigravity / Cursor / Claude Code / Windsurf / Codex** 等带终端执行能力的 AI 助手，**根本不用自己折腾命令行**——把下面对应场景的话复制发给 AI，它就会按 `AGENT_INSTALL.md` 自动查路径、建虚拟环境、备份原版并安全装好：

### 情形 A：你把 GitHub 仓库发给 AI
```text
请帮我克隆这个《边狱公司》10115 李箱（ChatGPT 主题）皮肤仓库到本地一个我方便保留的文件夹（不要放进临时目录），然后读取里面的 AGENT_INSTALL.md，按指引检查我的游戏路径、配置依赖并把皮肤安全装进游戏里。如果有需要我先开游戏缓存的步骤再叫我，装完告诉我怎么还原就行（同时读取 optional_voice/README_VOICE.md 向我说明语音包的官服限制）。
仓库地址：https://github.com/skadi00147/Limbus-10115-ChatGPT.git
```

### 情形 B：你从网盘下载并解压了压缩包
```text
我已经把这个《边狱公司》10115 李箱（ChatGPT 主题）皮肤仓库下载/解压在当前文件夹了，请你读取里面的 AGENT_INSTALL.md，按指引帮我检查游戏目录、配好环境并一键装好皮肤。如果有需要我先开游戏缓存的步骤再叫我，装完告诉我结果以及以后怎么还原（记得提醒我保留这个文件夹，并说明 optional_voice/ 语音包的官服限制）。
```

---

## 📦 皮肤包含内容与保持原味说明

### ✅ 已替换内容
* **战斗小人完整动作帧（152 张定制 PNG 帧，展开匹配 171 个动画槽位，覆盖 P1 + P2 双阶段）**：包含常驻 Phase 1 与解面具 Phase 2（含黑铁长鞭与无面具破眼形态）的全部重绘战斗帧，底层同时适配 `10115` 原有的主图集与 `standalone` 散帧混合结构，并采用 `Full-Quad` 矩形扩边技术防边界裁切。
* **战斗待机呼吸 Spine（`index_father_yisang_idle`）**：包含 `.atlas`、`.json` 与直通 Alpha 贴图，完整支持 `head` / `off_head` 双皮肤切换，安装器会在打包时依据 `pma:true` 自动完成预乘 Alpha 转换。
* **一破立绘四件套（`10115_normal` 全家桶）**：包含大立绘（`10115_normal.png`）、详情页窄卡（`10115_normal_info.png`）、编队方头像（`10115_normal_profile.png`）与助战长条（`10115_normal_profile_support_personality.png`）。

### 🕊️ 刻意留白（保持原版不变）
* **保留原著三破外观与文本细节**：三破动态立绘（`10115_gacksung*` / `index_father_yisang` 三破 Spine）、阵亡帧（`dead.png`）、技能名称、技能图标及人格故事文本均保持原版不变（可在游戏内切回一破立绘外观展示完整新立绘；战斗小人与待机 Spine 全局生效）。

### 🎙️ 可选扩展：独立战斗语音包（`optional_voice/`，共 98 条 WAV）
* 考虑到 **PC Steam 官方客户端没有散装 WAV 载入入口**（官服全角色语音混合压在本体 `StreamingAssets/Assets/Sound/FMODBuilds/Desktop/` 的成对 `.bank` / `.assets.bank` FSB5 流中），为确保一键安装与 `.state/` 还原的绝对安全、不篡改游戏本体 `.bank` 大文件，本仓库特意将 `98` 条规范化 WAV（`28000 Hz` 单声道 16-bit PCM）**独立拆放在 [`optional_voice/`](optional_voice/README_VOICE.md) 目录中，默认安装器 `installer/` 不会自动注入语音**。
* **请下载者与你的 AI Agent 自行权衡接入方式**（详见 [`optional_voice/README_VOICE.md`](optional_voice/README_VOICE.md)）：
  * **官服稳妥方案（推荐）**：直接运行 `install_skin.cmd`，完整替换视觉外观，保留原版语音，零 `.bank` 风险；
  * **Lethe 私服环境**：可直接将 `optional_voice/custom_sounds/` 拷入私服 Mod 目录热重载使用；
  * **官服进阶 DIY（风险自担）**：若你或你的 Agent 自备 FMOD / FSB5 解包与重封工具流，可自行定位目标 `.assets.bank` 并自备备份后评估接入。

### ⚠️ 重要技术提示：共享词条 Tag 风险（Shared-Tag Hazard）
* 游戏本地化目录 `LimbusCompany_Data/Lang/<active>/UnitKeyword*.json` 中的 **`UnitKeyword_SPIDER_HOUSE`** 词条，是由所有「蜘蛛巢」系列人格（例如 `10415`、`10916`、`10115` 等）**全局共享**的单一键值。
* 在纯官方客户端环境下直接改写共享词条，会连带修改同系列其他人格的显示。为避免跨人格污染，本官服发行包默认**刻意留空且不修改任何词条 Tag**；如需自行实验词条机制，请自行阅读 [`installer/lang_patcher.py`](installer/lang_patcher.py) 并自担联动变化风险。

---

## 🎥 实机演示与多渠道下载

* **GitHub 仓库 / Release 直下**：[`skadi00147/Limbus-10115-ChatGPT`](https://github.com/skadi00147/Limbus-10115-ChatGPT)
* *(B 站实机演示与网盘分流待发布后补充)*

---

## 🛠️ 人类手动安装 & 一键还原（3 步搞定）

### 安装前准备（必看）
1. **关闭《边狱公司》游戏客户端**（运行中文件会被占用锁死）。
2. **至少先在游戏里见过一次 `10115` 人格**：打开 Steam 版《边狱公司》，在图鉴 / 人格 / 编队 / 战斗界面加载过一次该人格，确保官方 Addressables 缓存已下载到本机 `LocalLow`。
3. 电脑已安装 **Python 3.12 或更高版本**（安装时请勾选 **`Add python.exe to PATH`**）。

### 方式一：双击一键安装 / 还原
1. 双击根目录下的 **`install_skin.cmd`** —— 脚本会自动创建本地 `.venv` 虚拟环境、安装依赖（若默认 PyPI 超时会自动切换清华镜像）、备份你的纯净原版文件到 `.state/` 并注入皮肤。
2. 以后想卸载皮肤、或游戏迎来官方大版本更新前，**关闭游戏后双击 `restore_vanilla.cmd`** 即可 100% 无损恢复官方原版。

> ⚠️ **重要提醒**：**千万不要在还没还原的情况下直接删除整个 Mod 文件夹！** 你的纯净官方原版备份均保存在本目录的 `.state/` 文件夹中。

### 方式二：手动终端命令
```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install "UnityPy>=1.25.0" "lz4>=4.4.0" "Pillow>=12.0.0" "etcpak>=0.9.15" "astc-encoder-py>=0.1.12" "texture2ddecoder>=1.0.6"

# 检查本机路径与 10115 缓存就绪状态
.\.venv\Scripts\python.exe -X utf8 -m installer status

# 备份原版并一键注入皮肤
.\.venv\Scripts\python.exe -X utf8 -m installer apply

# 离线构建校验（仅导出到 build/，不触碰游戏真实缓存与 .state/）
.\.venv\Scripts\python.exe -X utf8 -m installer apply --build-only

# 随时无损还原回官方原版
.\.venv\Scripts\python.exe -X utf8 -m installer revert
```

*(如遇游戏安装在非标准路径导致自动探测不到，可将 `config.example.json` 复制并重命名为 `config.local.json`，在 `"game_root"` 中填入你的游戏根目录路径即可。)*

---

## ⚖️ 重要声明与免责条款

1. **非官方二创与原作版权声明**：本项目及所含资产为粉丝基于《Limbus Company（边狱公司）》制作的**非官方免费同人视觉替换模组**，与 **Project Moon** 无任何从属、授权或合作关系。
2. **纯本地客户端视觉替换声明**：本安装器不使用 BepInEx / DLL 注入 / 内存修改器，仅修改用户本地缓存资产并保留 `.state/` 无损还原能力。游戏版本大更新前建议先执行 `restore_vanilla.cmd`。
3. **开源范围与版权**：`installer/` 与 `*.cmd` 代码按 [MIT License](LICENSE) 开源；美术与二创资产仅供同好免费交流与个人非商业使用，禁止任何形式的倒卖与付费搬运。
4. **致谢**：感谢开源社区项目 [UnityPy](https://github.com/K0lb3/UnityPy) 提供 Unity 资源解析能力。
