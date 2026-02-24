# 🍬 糖豆人 (Jelly Man)

一款高清画质的糖豆人风格多人竞技闯关网页游戏，类似 Fall Guys。

![游戏预览](https://img.shields.io/badge/HTML5-Canvas-blue) ![版本](https://img.shields.io/badge/版本-1.0.0-green) ![License](https://img.shields.io/badge/License-MIT-yellow)

## 🎮 游戏介绍

糖豆人 (Jelly Man) 是一款基于 HTML5 Canvas 的多人竞技游戏。你将控制一个可爱果冻般的角色，与 11 名 AI 对手同台竞技，躲避各种障碍物，坚持到最后即可获胜！

## ✨ 特性

- **高清画质** - 支持高 DPI 屏幕，清晰无模糊
- **果冻物理** - 独特的果冻般弹跳和摆动动画
- **丰富音效** - 跳跃、落地、碰撞、胜利等多种音效
- **12 人对战** - 与 11 名 AI 玩家同时竞技
- **障碍物系统** - 旋转风车、摆动球等多种障碍
- **响应式设计** - 支持桌面和移动设备
- **中文界面** - 全中文游戏界面

## 🕹️ 操作方式

| 按键 | 功能 |
|------|------|
| 方向键 / WASD | 移动 |
| 空格 / 上箭头 / W | 跳跃 |

## 🚀 运行方式

### 方式一：直接打开

在浏览器中直接打开 `index.html` 文件即可开始游戏。

```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

### 方式二：本地服务器（推荐）

```bash
# Python 3
python -m http.server 8080

# Node.js
npx http-server -p 8080
```

然后在浏览器访问 http://localhost:8080

## 🎯 游戏规则

1. 游戏开始前有 3 秒倒计时
2. 每回合限时 60 秒
3. 躲避旋转风车和摆动球障碍物
4. 不要掉出平台，否则被淘汰
5. 坚持到最后一名即可获胜

## 🛠️ 技术栈

- HTML5 Canvas
- CSS3
- Vanilla JavaScript
- Web Audio API (音效)

## 📁 项目结构

```
Jelly-man/
├── index.html      # 游戏主文件
├── AGENTS.md       # 开发规范文档
└── README.md       # 项目说明文档
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

---

**Enjoy the game! 🎉**
