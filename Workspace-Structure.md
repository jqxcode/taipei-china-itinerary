# 旅行工作区目录规则

适用路径：`C:\Users\qitxu\OneDrive\Documents\Self\2026-07-05AsiaTravel`

本规则对今后的人工编辑和所有代理都有效。

## 固定结构

```text
2026-07-05AsiaTravel\
├─ *.html                         最终生成页面，保持在根目录
├─ *.md / *.en.md / MyMaps.csv    权威旅行内容和研究资料
├─ itinerary-images\              HTML 使用的行程图片
├─ itinerary-images-candidates\   候选图片
├─ itinerary-maps\                HTML 使用的地图 PNG 与坐标数据
├─ iteration\                     生成器、渲染器、运行日志和验证记录
└─ private\
   ├─ tickets\                    私人票据
   └─ archive\                    已解决但仍保留参考价值的私人流程文档
```

## 强制规则

1. 根目录的生成 HTML 不得移动或改名；相对链接和发布流程依赖这些路径。
2. `itinerary-images\`、`itinerary-images-candidates\`、`itinerary-maps\` 不得移出根目录。生成器使用硬编码绝对路径，HTML 使用相对路径。
3. 新的旅行计划、研究、审计和人工维护内容以 `.md`（需要时配套 `.en.md`）保存在根目录。
4. 新的生成器、渲染器和构建辅助脚本必须放入 `iteration\`；不得把 session-state 临时副本当作规范版本。
5. 身份证件、护照/许可、票据、订房/机票确认、财务记录、预填申请表及类似私人原始资料必须放入 `private\`。
6. 已完成但可能因计划变化而需要复查的私人流程资料放入 `private\archive\`，不得混回公开构建源。
7. 任何最终 HTML 或公开 GitHub Pages 暂存目录都不得引用或复制 `private\`、`iteration\`、私人票据、证件、PDF/XLSX 或备份文件。
8. 删除资料前必须确认其内容已百分之百并入权威文件；无法确认时应归档而不是删除。
9. 根目录不得新增 `*.bak-*`。版本历史应由规范文档和版本控制承担。

## 未来会话如何重新生成

直接运行 `iteration\` 中的脚本，例如：

```powershell
python "C:\Users\qitxu\OneDrive\Documents\Self\2026-07-05AsiaTravel\iteration\build_itinerary_v2.py"
```

其他命令见 `iteration\README.md`。这些脚本按硬编码绝对路径读写根目录的
`itinerary-images\`、`itinerary-maps\` 和顶层 HTML，因此工作区必须保持在当前完整路径。

重新生成后必须检查：

- 生成命令无 traceback；
- HTML 中没有私人文件路径；
- 所有本地 `src`/`href` 目标存在；
- 发布暂存目录不含 PDF、XLSX、证件、票据或构建脚本。
