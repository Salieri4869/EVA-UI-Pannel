#  EVA-UI-Pannel

**ACTIVE TIME REMAINING — 正面视图**

EVA 风格的装甲风 HUD 倒计时／时钟界面，纯 HTML + CSS + 内联 SVG 实现，无需构建、双击即开。

---

## ✨ 特性

- **充电式装甲背景**：断电纯黑，通电后沿「左下 → 右上」扫掠充能显现
- **双模式**：倒计时 `5:00.00`（精确到厘秒）＋ 24H / 12H 实时时钟，点标题左侧小方块循环切换
- **警戒 / 归零**：最后 7 秒整体切红闪烁；归零后标题、斑马柱、STOP 红条分组异步闪烁
- **青线随电亮起**：装甲刻度、分隔线按延迟依次点亮，模拟电源接通时序
- **CRT 断电观感**：通电淡入、断电瞬间熄灭，字体缺失自动回退
- **全键盘可操作**：开关、模式框、按钮均支持 Tab + Enter，聚焦时才显示描边

## 📁 文件结构

```
.
├── 活動限界まで.html            # 唯一入口，单文件自包含
└── font/
    └── EVA-Matisse_Classic.ttf  # EVA 标题字体（需自行放入）
```

## 🚀 快速开始

```bash
git clone https://gitee.com/AmadeusC204/eva-ui-pannel.git
cd src
ls font/EVA-Matisse_Classic.ttf   # 确认字体已就位
open 活動限界まで.html            # 双击打开即可
```

想用静态服务器（字体加载更稳）：

```bash
python3 -m http.server 8080
# 访问 http://localhost:8080/活動限界まで.html
```

## 🎮 用法

| 操作 | 效果 |
|---|---|
| 点击 `POWER SUPPLY` | 接通／切断主电源，装甲充能 + 屏幕点亮 |
| 点击标题左侧小方块 | 循环切换：倒计时 → 24H 时钟 → 12H 时钟 |
| 点击 `INTERNAL / EXTERNAL` | 切换活动模式，激活框斑马柱亮起 |
| 点击 `SLOW / NORMAL / RACING` | 选择倍速，按钮底部红条指示当前档 |
| 倒计时归零 | 自动进 `STOP`，标题与 DANGER 框分组闪烁 |
| 键盘 Tab + Enter | 全功能可键盘操作 |

> 断电时数码管、模式框、按钮、DANGER 框一律隐藏，只留黑色装甲轮廓。

## 🧠 状态机

UI 完全由 `<body>` 上的 class 组合驱动，改状态只需增删 class：

| class | 含义 | 触发 |
|---|---|---|
| `pw` | 主电源接通，充能动画 + 全 UI 淡入 | 点 POWER SUPPLY |
| `clk` | 时钟视图（隐藏倒计时，显示 HH:MM:SS） | 点标题左侧方块 |
| `danger` | 最后 7 秒：琥珀切血红，青线保持 | 倒计时 ≤ 7s |
| `zero` | 活動限界到達：A/B 两组异频闪烁，仅留 STOP 红条 | 倒计时 = 0 |

```js
document.body.classList.add('pw');      // 开机
document.body.classList.toggle('clk'); // 切时钟
document.body.classList.add('danger'); // 进警戒
document.body.classList.add('zero');   // 归零锁定
```

## 🔤 字体

标题用 **EVA Matisse Classic**（自托管 @font-face）。该字体为闭源商业字体，**仓库不含此文件**：

- 请自行把 `EVA-Matisse_Classic.ttf` 放进 `font/` 目录
- 缺失时回退到系统 `serif`，布局不崩但失去 EVA 字形味道

## 🎨 自定义

视觉令牌集中在 `<style>` 顶部，改一处全局生效：

```css
body{
  --amber:#ffac1e;   /* 主色 */
  --red:#ff2415;     /* 警戒红 */
  --cyan:#3fd8ff;    /* 刻度青 */
}
```

| 想改 | 位置 |
|---|---|
| 倒计时初始值（默认 5:00.00） | 页面底部 JS 的初始秒数常量 |
| 警戒阈值（默认 7s） | JS 中 `danger` 的判定值 |
| 画布尺寸（默认 1300×600） | `.panel { width / height }` |
| 数码管亮度 | `.seg{opacity:.07}` → `.seg.on{opacity:1}` |

## 📄 许可

界面代码：MIT © 2026。

`EVA-Matisse_Classic.ttf` 版权归其原作者／发行方，请勿随仓库再分发，使用前请确保已获合法授权。

---

> *「活動限界まで — あと 5:00.00」*
