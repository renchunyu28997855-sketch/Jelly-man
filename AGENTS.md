# AGENTS.md - 糖豆人 (Jelly Man) 游戏开发规范

本文档为 AI 代理提供糖豆人风格游戏项目的开发规范和命令参考。

---

## 1. 项目概述

- **项目名称**: Jelly Man (糖豆人)
- **项目类型**: HTML5 Canvas 网页游戏
- **主要技术**: HTML5, CSS3, Vanilla JavaScript
- **运行方式**: 浏览器直接打开 `index.html`
- **游戏类型**: 多人竞技闯关类游戏（类似 Fall Guys）

---

## 2. 核心原则

### 2.1 代理行为准则

- **不先斩后奏**: 先确认再行动，不要在未获授权的情况下修改代码
- **最小改动**: 只改必要的部分，避免大规模重构
- **验证优先**: 任何修改后必须验证功能正常
- **知错即改**: 发现问题立即修复，不掩盖
- **语言**: 代码注释使用中文，英文代码命名

### 2.2 工作流程

```
接收请求 → 理解意图 → 评估复杂度 → 确认范围 → 执行 → 验证 → 报告
```

---

## 3. 开发命令

### 3.1 简单项目（无需构建）

本项目为纯前端单文件，直接用浏览器打开即可：

```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

### 3.2 本地服务器（推荐）

```bash
# Python 3
python -m http.server 8080

# Node.js
npx http-server -p 8080
```

访问: http://localhost:8080

### 3.3 代码验证

```bash
# 使用 IDE 内置 JavaScript 检查
# 推荐 VS Code + ESLint 插件
```

---

## 4. 代码规范

### 4.1 通用规则

| 规则 | 说明 |
|------|------|
| 缩进 | 2 空格 |
| 行尾 | LF (Unix-style) |
| 编码 | UTF-8 |
| 注释 | 中文注释，英文代码 |

### 4.2 命名约定

| 类型 | 规则 | 示例 |
|------|------|------|
| 文件/目录 | kebab-case | `index.html`, `game-utils.js` |
| 类名 | PascalCase | `JellyPlayer`, `GameState` |
| 函数/变量 | camelCase | `initCanvas`, `gameSpeed` |
| 常量 | UPPER_SNAKE | `PLAYER_SPEED`, `GRAVITY` |
| 配置对象 | PascalCase | `CONFIG`, `COLORS` |

### 4.3 代码结构顺序

```javascript
// 1. 常量配置
const CONFIG = { ... };
const COLORS = { ... };
const SOUNDS = { ... };

// 2. 游戏状态
const GameState = { ... };

// 3. 类定义
class JellyPlayer { ... }
class Game { ... }

// 4. 初始化
window.addEventListener('load', () => { ... });
```

### 4.4 禁止事项

- ❌ 禁止使用 `as any`（本项目为 JS，非 TS）
- ❌ 禁止提交 secrets（API keys, tokens）
- ❌ 禁止删除/修改测试来让测试通过
- ❌ 禁止留下空 catch 块 `catch(e) {}`
- ❌ 禁止 commit 除非用户明确要求

---

## 5. 高清渲染规范

### 5.1 Canvas 高清渲染模式

```javascript
// 标准高清渲染实现
initCanvas() {
  // 1. 计算逻辑尺寸
  const logicalWidth = 1920;
  const logicalHeight = 1080;
  
  // 2. 获取设备像素比
  const dpr = window.devicePixelRatio || 1;

  // 3. 设置物理画布大小
  this.canvas.width = logicalWidth * dpr;
  this.canvas.height = logicalHeight * dpr;

  // 4. 缩放上下文
  this.ctx.scale(dpr, dpr);

  // 5. 通过CSS设置显示尺寸
  this.canvas.style.width = logicalWidth + 'px';
  this.canvas.style.height = logicalHeight + 'px';
}
```

### 5.2 设计要求

- 默认逻辑分辨率: 1920x1080
- 所有绘图使用**逻辑坐标**（不需要手动乘以 dpr）
- 确保在高DPI屏幕上画面清晰无模糊

---

## 6. 游戏逻辑规范

### 6.1 糖豆人角色特性

```javascript
// 糖豆人物理特性
const JELLY_PHYSICS = {
  GRAVITY: 0.5,           // 重力加速度
  JUMP_FORCE: -15,         // 跳跃力度
  MOVE_SPEED: 5,          // 移动速度
  FRICTION: 0.85,         // 摩擦系数
  BOUNCE: 0.3,            // 弹性系数
  MASS: 1                 // 质量
};
```

### 6.2 糖豆人视觉特性

```javascript
// 糖豆人外观
class JellyPlayer {
  constructor(x, y, color) {
    this.x = x;
    this.y = y;
    this.radius = 30;      // 身体半径
    this.color = color;   // 身体颜色
    this.eyeOffset = 8;   // 眼睛偏移
    // 果冻般的摆动动画
    this.wobble = 0;
    this.wobbleSpeed = 0.1;
  }
}
```

### 6.3 游戏状态机

```javascript
const GameState = {
  MENU: 'menu',           // 菜单界面
  COUNTDOWN: 'countdown',// 倒计时
  PLAYING: 'playing',    // 游戏中
  ROUND_END: 'round_end',// 回合结束
  GAME_OVER: 'game_over' // 游戏结束
};
```

### 6.4 碰撞检测

- 圆形碰撞检测用于玩家之间
- AABB 用于障碍物碰撞
- 边界检测防止走出画面

---

## 7. 音效规范

### 7.1 音效类型

| 音效 | 用途 | 格式 |
|------|------|------|
| bgm | 背景音乐 | loop |
| jump | 跳跃 | 短促 |
| land | 落地 | 短促 |
| hit | 碰撞 | 短促 |
| win | 胜利 | 较长 |
| fall | 掉落淘汰 | 较短 |
| countdown | 倒计时 | 短促 |
| button | 按钮点击 | 短促 |

### 7.2 音效实现

```javascript
// 使用 Web Audio API
class SoundManager {
  constructor() {
    this.audioContext = new (window.AudioContext || window.webkitAudioContext)();
    this.sounds = {};
  }

  // 生成简单的音效
  playTone(frequency, duration, type = 'sine') {
    const oscillator = this.audioContext.createOscillator();
    const gainNode = this.audioContext.createGain();
    
    oscillator.connect(gainNode);
    gainNode.connect(this.audioContext.destination);
    
    oscillator.frequency.value = frequency;
    oscillator.type = type;
    
    gainNode.gain.setValueAtTime(0.3, this.audioContext.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, this.audioContext.currentTime + duration);
    
    oscillator.start();
    oscillator.stop(this.audioContext.currentTime + duration);
  }
}
```

### 7.3 音效控制

- 用户首次交互后初始化 AudioContext（浏览器策略）
- 提供音效开关
- 背景音乐音量可调

---

## 8. 动画规范

### 8.1 糖豆人摆动动画

```javascript
// 使用正弦函数实现果冻般的摆动
updateWobble(deltaTime) {
  this.wobble += this.wobbleSpeed * deltaTime;
  this.scaleX = 1 + Math.sin(this.wobble) * 0.1;
  this.scaleY = 1 - Math.sin(this.wobble) * 0.1;
}
```

### 8.2 眼睛跟随

```javascript
// 眼睛始终看向移动方向
drawEyes(ctx) {
  const lookX = this.vx * 2;
  const lookY = this.vy * 2;
  // 绘制眼睛跟随 lookX, lookY
}
```

### 8.3 表情变化

- 正常: 眼睛正视
- 跳跃: 眼睛惊讶
- 加速: 眼睛向后倾斜
- 落后: 眼睛向下看

---

## 9. 响应式规范

### 9.1 自适应布局

```css
#gameContainer {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #1a1a2e;
}

#gameCanvas {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}
```

### 9.2 断点设置

- 移动端: 触控操作
- 平板: 触控或手柄
- 桌面: 键盘操作

---

## 10. 游戏模式

### 10.1 核心玩法

1. **糖豆人闯关**: 多名玩家（8-20人）同时竞技
2. **障碍物**: 旋转风车、摆动球、滑坡等
3. **淘汰机制**: 掉出平台或触碰障碍物淘汰
4. **生存到最后**: 最后一名获胜

### 10.2 关卡类型

- 竞速关: 跑到终点
- 生存关: 躲避障碍物
- 积分关: 收集金币
- 搏击关: 把对手推下去

---

## 11. 验证要求

### 11.1 功能验证

修改后必须验证以下功能正常：

- [ ] 游戏可以正常开始
- [ ] 方向键/WASD/空格 控制移动和跳跃
- [ ] 糖豆人有果冻般的物理效果
- [ ] 碰撞检测正常工作
- [ ] 障碍物正常运作
- [ ] 音效正常播放
- [ ] 高DPI屏幕显示清晰

### 11.2 视觉验证

- [ ] 在高DPI屏幕上画面清晰无模糊
- [ ] 颜色与设计规格一致
- [ ] 动画流畅 (60fps)
- [ ] 响应式布局正常

### 11.3 技术验证

- [ ] 无 JavaScript 控制台错误
- [ ] 代码结构清晰
- [ ] 无硬编码敏感信息

---

## 12. 常见问题处理

### 12.1 调试策略

1. **最小复现**: 先理解问题再修改
2. **一次一改**: 只改一个变量，排除其他干扰
3. **验证每次**: 改完立即验证，不要批量改

### 12.2 失败处理

修复 3 次失败后：
1. 停止所有修改
2. 恢复到最后已知可用的状态
3. 记录已尝试的方案
4. 咨询 Oracle

---

## 13. Git 上传指南

### 13.1 初始化仓库

```bash
git init
git add .
git commit -m "Initial commit: Jelly Man game"
```

### 13.2 添加远程仓库

```bash
git remote add origin https://github.com/renchunyu28997855-sketch/Jelly-man.git
git branch -M main
git push -u origin main
```

---

*本规范适用于糖豆人 (Jelly Man) 游戏项目的所有 AI 代理开发工作。*
*本文档由 Sisyphus AI Agent 生成，最后更新于 2026-02-24*
