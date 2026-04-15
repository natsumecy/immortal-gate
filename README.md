# 太虚幻境 / Immortal

一个受 gaburyu.com 启发的网页互动叙事游戏。纯原生 Canvas + JavaScript 实现，配合 p5.js 星星特效。

## 游戏简介

你在一个空无一人的站台等待。列车到来，一位似曾相识的先生走下车，在你身旁坐下。你们开始交谈——关于记忆、选择、遗憾，以及那些未曾说出口的话。

## 操作方式

| 按键/操作 | 功能 |
|-----------|------|
| 点击地面 | 移动角色 |
| WASD / 方向键 | 移动角色 |
| Enter / Space | 推进对话、选择选项、触发交互 |
| ↑ / ↓ | 选项菜单中切换选择 |

## 交互元素

- **岩田先生**：靠近后对话，体验完整叙事流程
- **书架**：靠近后按 Enter，可阅读 4 本短篇文字
- **街机**：靠近后按 Enter，可游玩小游戏
- **长椅**：坐下触发列车到来

## 技术栈

- 纯原生 HTML5 Canvas + JavaScript（无游戏引擎）
- p5.js 用于星星粒子特效
- 像素风精灵动画（8方向行走）
- 打字机效果的对话系统

## 文件结构

```
├── index.html              # 主游戏文件
├── README.md               # 本文件
├── assets/
│   ├── fonts/              # 字体文件
│   │   ├── ChillBitmap_16px.ttf
│   │   └── ZhengGeDianHei-16.ttf
│   ├── images/             # 场景贴图
│   │   ├── arcade.png
│   │   ├── bench.png
│   │   ├── door.png
│   │   ├── shelf.png
│   │   ├── station.png
│   │   ├── stationv2.png
│   │   ├── title.png
│   │   ├── train.png
│   │   ├── Turnstile-Sheet.png
│   │   └── waitingroom.png
│   └── sprites/            # 角色精灵图
│       ├── clock-Sheet.png
│       ├── iwata-Sheet.png
│       ├── natsumenormal-Sheet-export.png
│       └── train.png
└── CLAUDE.md               # 项目开发文档
```

## 本地运行

直接在浏览器中打开 `index.html` 即可运行，无需构建步骤。

建议使用本地服务器以避免跨域问题：

```bash
# Python 3
python -m http.server 8000

# 或 Node.js
npx serve .
```

然后访问 `http://localhost:8000`

##  Credits

- 角色设计、场景美术：原创
- 正格点黑字体：开源中文像素字体
- 灵感来源：gaburyu.com

## License

MIT
