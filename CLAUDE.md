# 游戏项目进度

## 目标
做一个类似 gaburyu.com 的网页互动体验：点击移动角色、场景切换、NPC对话。
纯原生 Canvas + JS，不用 Phaser。p5.js 叠加做星星特效。

## 文件结构
```
game/
├── index.html              # 主文件，所有游戏逻辑 (~2250行)
├── CLAUDE.md               # 本文件
├── natsumenormal-Sheet-export.png   # 玩家角色精灵图
├── iwata-Sheet.png         # 岩田先生角色精灵图
├── Turnstile-Sheet.png     # 检票口精灵图
├── arcade.png              # 街机贴图
├── shelf.png               # 书架贴图
└── ZhengGeDianHei-16.ttf   # 正格点黑字体（中文像素）
```

## 核心系统

### 1. 角色系统
- 像素清晰渲染（`imageSmoothingEnabled = false` + CSS `image-rendering: pixelated`）
- 点击画布移动，方向自动判断（8方向）
- 走路动画（3帧循环，每8帧切换）
- 斜向移动速度归一化（沿楼梯零漂移）

**精灵图规格（玩家）**:
- 单帧尺寸：48 x 58 px
- 排列：1行12列
- 方向映射：
  - 向下：列 0, 1, 2
  - 向左：列 3, 4, 5
  - 向右：列 6, 7, 8
  - 向上：列 9, 10, 11

### 2. 对话系统
- 数据驱动：`DIALOGUE_SCRIPT` 数组定义全部对白流程
- 支持类型：
  - `line`: 角色台词（player/iwata）
  - `wait`: 等待时间（ms）
  - `choice`: 分支选项
  - `action`: 触发游戏动作（如 walk_to_edge, stars_dense, sprite_back 等）
- 打字机效果（逐字显示）
- 气泡位置：根据角色位置动态计算，位于角色头顶
- 触发方式：点击屏幕或按 Enter/Space

### 3. 书架系统

**交互流程**（街机同款）：
1. 玩家走近书架（距离 < 110px）
2. 按 Enter 或点击书架 → 弹出询问"书架上有一些书，要翻阅吗？"
3. 按 Enter 推进 → 显示"是 / 否"选项
4. 选择后：
   - 是 → 打开书本列表（4本书）
   - 否 → 关闭对话

**选项 UI**（岩田同款风格）：
- ▶ 标记当前选中项
- 高亮背景（深棕色 rgba(139, 90, 43, 0.3)）
- 垂直排列

**阅读界面**：
- 逐行打字机效果显示书本内容
- 每本书有独立的数据结构（title + lines 数组）
- 点击/Enter 关闭阅读返回游戏

**书本数据**（4本书）：
- `无` - 关于死亡与记忆的诗
- `界` - 红楼梦太虚幻境的引用
- `瞬` - 关于重复与停顿的感悟
- `余` - 关于遗留物与对话的记录

**状态变量**：
```javascript
_shelfAsking        // 是否正在询问阶段
_shelfAskStep       // 0=问问题, 1=显示选项
_shelfChoiceSelected // 0=是, 1=否
_shelfOpen          // 选书面板开启
_shelfBookIdx       // 当前阅读的书索引
_shelfRevealIdx     // 已显示的行数
_shelfTypingChars   // 当前行已打出字符数
```

### 4. 街机系统
- 靠近时显示提示气泡
- 点击/Enter 触发对话
- 对话结束后自动关闭

### 5. 键盘控制
| 按键 | 功能 |
|------|------|
| Enter/Space | 推进对话、选择选项、触发交互 |
| ↑/↓ | 选项菜单中切换选择 |
| WASD / 方向键 | 移动角色（取消点击目标） |

### 6. I18N 国际化
- 支持中文/英文双语切换
- 标题画面语言选择按钮（中文/English）
- localStorage 持久化语言偏好
- 所有文本通过 `t(key)` 函数动态获取
- 数据结构：`I18N.zh` / `I18N.en`
- 动态内容：dialogue、books、UI 提示全部可翻译

### 7. 渲染细节
- Canvas 尺寸：全屏，支持移动端缩放（MOBILE_ZOOM = 2）
- 楼梯斜向向量：精确计算零漂移（DIAG_DX ≈ 0.858, DIAG_DY ≈ -0.512）
- 所有贴图使用 Image 对象预加载

### 8. 字体
- `ZhengGeDianHei-16.ttf`：正格点黑，中文像素字体
- Google Fonts `DotGothic16`：日文像素字体（备用）

## 已完成功能
- [x] 角色渲染（像素清晰，无模糊）
- [x] 点击画布移动角色，方向自动判断
- [x] 走路动画（3帧循环）
- [x] 到达目标停下
- [x] 对话气泡（头顶，带小三角）
- [x] 打字机效果（逐字显示）
- [x] 自动换行
- [x] 按空格/Enter触发对话
- [x] 岩田先生 NPC 完整对话流程
- [x] 街机交互（点击/Enter触发对话）
- [x] 书架系统：
  - [x] 靠近检测 + 气泡提示
  - [x] 询问流程"要翻阅吗？"（街机同款交互）
  - [x] 是/否选项（岩田同款UI风格）
  - [x] 4本书选单
  - [x] 书本内容逐行打字机显示
  - [x] 键盘支持（Enter推进，↑↓切换选项）
- [x] 标题画面（空白画面 → 按Enter/Space启动）
- [x] I18N 国际化：
  - [x] 标题画面语言选择（中文/English）
  - [x] 完整中文对话翻译
  - [x] 完整英文对话翻译
  - [x] 书架4本书双语内容
  - [x] 街机游戏提示双语
  - [x] localStorage 语言偏好持久化

## 待做
- [ ] p5.js 星星下落特效（叠加在 canvas 上）
- [ ] 场景切换（进地下等）
- [ ] 更多场景元素

## 技术备注
- `ctx.imageSmoothingEnabled = false` 保证像素清晰
- canvas CSS 需要 `image-rendering: pixelated`
- 气泡跟随 player.x / player.y 实时绘制
- 调用 `startBubble('文字')` 触发对话
- 书架位置：{ x: 961, y: 844 }
- 街机位置：{ x: 37, y: 824 }
