# 工作区重组报告

完成日期：2026-08-30

## 1. 初始根目录分类

### A. 最终成品与显示资源（路径不变）

- HTML：`Cash-Exchange.html`、`China-HSR-Map.html`、`China-HSR-Plan.html`、`esim.html`、`Hotel-Selection.html`、`Nanjing-Day1-Map.html`、`Nanjing-Day2-Map.html`、`Nanjing-Day3-Map.html`、`Reservations.html`、`Taipei-China-Itinerary.html`、`Taipei-Food-Map.html`、`Taipei-Hotels-Map.html`
- 资源目录：`itinerary-images\`、`itinerary-images-candidates\`、`itinerary-maps\`
- 根目录图片：`Taipei-Food-Map.png`、`taipei-airports-map.png`

### B. 权威内容源（保留根目录）

- `Airport-Transfer-Compare.md`
- `China-HSR-Plan.md`、`China-HSR-Plan.en.md`
- `esim.md`、`esim.en.md`
- `Final-Integration-Report.md`
- `Google-MyMaps-Automation-Research.md`
- `master-plan-hotel.md`、`master-plan-hotel.en.md`
- `Master-Plan.md`
- `Nanjing-3Day-Plan.md`
- `Nanjing-Luxury-Bathhouse.md`
- `onsen-compare.md`
- `Requirements-and-Context.md`
- `Reservation-Audit.md`、`Reservation-Audit.en.md`
- `Shanghai-Hongqiao-Arrival.md`
- `Suzhou-Day-Plan.md`
- `Suzhou-Huqiu-Shantang-Analysis.md`
- `Taipei-3Day-Executable-Plan.md`
- `Taipei-Visual-Food-Research.md`
- `Travel-Plan-Completeness-Audit.md`
- `TWAC-Arrival-Card-Runbook.md`
- `TWAC-Family-Document-Request-EN.md`
- `TWD-Cash-Exchange-Plan.md`、`TWD-Cash-Exchange-Plan.en.md`
- `Ximending-Shopping-Services.md`
- `Zhejiang-Taizhou-Districts-Plan.md`
- `Taiwan-Food-MyMaps.csv`、`Shanghai-Dianping-MyMaps.csv`

`onsen-compare.md` 未删除：它包含北投、上海和杭州温泉的独有比较与价格资料，未被只聚焦南京的 `Nanjing-Luxury-Bathhouse.md` 完整覆盖。

### C. 构建机械（迁入 `iteration\`）

从临时 session-state 复制：

- `build_itinerary_v2.py`
- `build_food_map_v2.py`
- `build_hotels_map.py`
- `build_hsr_map.py`
- `render_bilingual_html.py`
- `render_hotel_html.py`
- `render_md_html.py`
- `bilingual_common.py`
- `sync_supporting_docs.py`

`render_hotel_html.py` 已改成现行双语酒店页面的兼容入口，避免旧单语实现覆盖 `Hotel-Selection.html`。

未复制 `build_public_pages.py` 及 `pages-deploy\`：它们属于明确排除的发布暂存流程。未复制一次性探针、订票自动化和已完成的集成迁移脚本，因为它们不是重新生成现行 HTML 的依赖。

### D. 私人原始资料（迁入 `private\`）

- `tickets\` → `private\tickets\`
- `3id-keanuu-2.jpg`
- `id-fred.jpg`
- `id-joylene.jpg`、`id-joylene-2.jpg`、`id-joylene-3.jpg`
- `id-keanuu.jpg`、`id-keanuu-2.jpg`
- `入台证.pdf`
- `T&C.pdf`
- `TECO-Appointment-Confirmation-2026-07-30.pdf`
- `TWAC_UserManual_Eng.pdf`
- `TWAC_ACARD_EXCEL_template.xlsx`
- `TWAC_ACARD_prefilled_DRAFT.xlsx`
- `Id-benjamin.pdf`
- 两份 `Confirmation Booking ... Delta Air Lines...pdf`
- 一份 `Express Checkout Booking ... Delta Air Lines...pdf`
- `Mani-remaining.png`

`Mani-remaining.png` 经查看是 Delta eCredit 余额/兑换记录，属于财务/票务私人记录。

以下已解决但仍可能有参考价值的资料归档到 `private\archive\`，未删除：

- `TECO-Seattle-Call-Script.md`
- `Taiwan-Permit-Application-Datasheet.md`

同时更新了 `Master-Plan.md`、`TWAC-Arrival-Card-Runbook.md` 和归档资料中的本地文件路径。

### E. 已删除的冗余备份

删除前均确认同名现行文件存在且非空，共 15 个：

- `master-plan-hotel.md.bak-20260830-2201`
- `Master-Plan.md.bak-20260830-2201`
- `Master-Plan.md.bak-20260830-2227`
- `Master-Plan.md.bak-20260830-2234`
- `Reservation-Audit.md.bak-20260830-2201`
- `Reservation-Audit.md.bak-20260830-2234`
- `Reservations.html.bak-20260830-2201`
- `Reservations.html.bak-20260830-2234`
- `Taipei-3Day-Executable-Plan.md.bak-20260830-2227`
- `Taipei-3Day-Executable-Plan.md.bak-20260830-2234`
- `Taipei-China-Itinerary.html.bak-20260830-2201`
- `Taipei-China-Itinerary.html.bak-20260830-2217`
- `Taipei-China-Itinerary.html.bak-20260830-2227`
- `Taipei-China-Itinerary.html.bak-20260830-2234`
- `Taipei-China-Itinerary.html.bak-20260830-2241`

## 2. 验证结果

- 9 个构建脚本已从 `iteration\` 成功执行或完成入口测试；无脚本 traceback。真实日志保存在 `iteration\build_itinerary_v2.run.log` 和 `iteration\generator-verification.run.log`。
- 8 个无需兼容修改的脚本与 session-state 源副本 SHA-256 完全一致；记录见 `iteration\script-copy-verification.csv`。
- `build_itinerary_v2.py` 运行成功：57 个点、40 个台北美食卡片、12 张每日地图；运行时复用了现有缓存，并尝试补充缺失媒体。
- 运行生成器时发现两个既存的可复现性因素：行程生成器会联网补抓缺失照片；部分当前 Markdown 内容比迁移前 HTML 更新。为遵守“不要改变现有成品”，验证后恢复了预操作 HTML 基线，并移除了验证期间新下载且原 HTML 未引用的图片。
- 最终 12 个顶层 HTML 与预操作副本逐字节一致；`Taipei-China-Itinerary.html` SHA-256 仍为 `5A9B0A1BAB420E54379957014A3ACBD1283F081D5DE9547BACB1793E5FB3DA7C`。
- 所有 HTML 的本地 `src`/`href` 均存在，缺失本地引用数为 0。
- HTML 属性中指向 `private\`、`iteration\`、本地票据、证件或本地 PDF 的引用数为 0。字面文本搜索仍会命中普通词组 `Tickets/reservations`、CSS 的 `grid-`，以及 `Cash-Exchange.html` 中两条公开的 Fidelity 官方 PDF 外链；这些都不是私人或本地文件引用。
- 根目录 28 个权威 Markdown 文件全部可按 UTF-8 读取且非空；两个 MyMaps CSV 仍在根目录。
- 根目录 `*.bak-*` 数量为 0。
- `private\` 中共有 22 个文件，另含 `tickets\` 和 `archive\` 两个子目录。
- `pages-deploy\` 操作前后聚合 SHA-256 均为 `FDCD2F050E78BB566ABAC9CEC5AC7862A2AE84411E50602D61E5B19BDA714DB8`，确认未触碰。

详细机器校验结果见 `iteration\verification-results.json`。
