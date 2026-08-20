[README.md](https://github.com/user-attachments/files/31257337/README.md)
# HTML 页面编辑器（浏览器扩展）

一款所见即所得的 **HTML 元素编辑器**：在支持编辑的 HTML 页面中，点击页面文字元素即可实时修改内容、字号、颜色、对齐、行高、字间距与位置，支持 5 套主题色一键切换及任意自定义颜色，并可将修改后的完整 HTML 一键导出下载。编辑面板核心移植自 `siyu-ppt-skill` 的 `template-swiss.html` 侧边编辑栏，封装为 Chrome / Edge Manifest V3 浏览器扩展。

## 功能

- 所见即所得：在包含 `#deck` + `.slide` 的 Swiss 风格 HTML 幻灯片页面中，点击页面文字元素即可选中并编辑
- 按 `E` 键或点击右下角齿轮按钮打开/关闭编辑面板
- 面板可拖动，双击标题栏归位
- 5 套主题色一键切换（克莱因蓝 / 柠檬黄 / 柠檬绿 / 活力橙 / 高端红）
- 自定义主题色：取色器或 `#RRGGBB` 输入任意颜色，自动计算前景对比色与亮色变体并应用
- 点击页面文字元素即可选中并实时编辑：
  - 文字内容、对齐方式
  - 字号、字重、颜色、行高、字间距
  - 位置微调（上/下/左/右）
- 插入新文本行（继承当前样式）
- 重置元素 / 重置此页面 / 重置所有修改
- `Ctrl / ⌘ + S` 导出修改后的完整 HTML（自动剥离编辑面板与齿轮）

## 安装

1. 打开 Chrome 或 Edge，地址栏输入：
   - Chrome: `chrome://extensions/`
   - Edge: `edge://extensions/`
2. 开启右上角 **开发者模式**
3. 点击 **“加载未打包的扩展程序”**
4. 选择本目录：`siyu-html-editor-extension`
5. 如果要打开本地 `file://` 测试页，需要在该扩展详情页开启 **“允许访问文件网址”**

## 使用

1. 安装扩展后，用浏览器打开`siyu-html-editor-extension`文件夹里的 `test/clean-template-swiss.html`进行此插件体验～
2. 按键盘 `E`，或点击右下角 `⚙` 齿轮按钮
3. 点击页面上任意文字元素，在右侧面板中编辑
4. 编辑完成后按 `Ctrl / ⌘ + S` 导出 HTML

## 文件结构

```
siyu-html-editor-extension/
├── manifest.json                  # Manifest V3 配置
├── content/
│   ├── content.css                # 编辑面板样式
│   └── content.js                 # 编辑面板核心逻辑
├── popup/
│   ├── popup.html                 # 扩展图标弹出面板
│   ├── popup.css
│   └── popup.js
├── icons/
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
├── test/
│   └── clean-template-swiss.html  # 去掉编辑面板的干净模板（测试页）
├── tools/
│   └── generate_icons.py          # 图标重生成脚本
└── README.md
```

## 设计要点

- 面板元素 ID 保持为 `editorPanel` / `editorToggle`，因此原模板翻页脚本中对 `#editorPanel.open` 的键盘/滚轮拦截仍然自动生效。
- 通过注入一段页面级桥接脚本，调用页面内部的 `__playSlide`、`__rescanAsciiCanvases`、`__currentSlideIndex`，保证“重置此页面”后能正确恢复入场动画。
- 导出 HTML 时使用 `DOMParser` 清理所有注入的 UI 元素；样式由扩展 CSS 注入，不会残留在导出的 HTML 中。

## 重生成图标

```bash
python tools/generate_icons.py
```

## 来源

- 源模板：`/Users/mac/Desktop/siyu-ppt-skill/assets/template-swiss.html`
