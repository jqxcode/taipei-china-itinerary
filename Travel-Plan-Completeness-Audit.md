# Asia Travel Workspace 完整性 / 一致性 / 安全审计

**审计时间：** 2026-08-30 22:48–23:10 PDT  
**审计方式：** 只读检查本地工作区、生成脚本、Pages staging 与 live GitHub Pages；未修改任何现有文件。  
**敏感信息处理：** 本报告不复述护照号、证件号、二维码内容、订单/确认代码、卡数据、个人邮箱或电话。

## 0. 执行摘要

- **审计要求簇：55 项**
  - ✅ Complete：**23**
  - 🟡 Partial：**11**
  - ❌ Missing：**2**
  - ⚠️ Conflict：**14**
  - ⛔ Blocked：**5**
- **最高优先级结论：**
  1. **live 页面仍是旧路线**：9/16 南京→杭州、9/17 杭州→浙江台州、9/18 台州→上海；不符合当前权威主线。
  2. **Pages staging 不是最新本地输出**：缺少 12 张每日合并地图，且仍含旧式单点静态地图/旧路线资产。
  3. **公开面存在敏感泄露风险**：staging/generated HTML 含本地票据 PDF 链接；多个公开 HTML 暴露酒店确认标识及过度具体的旅客/证件状态信息。
  4. `China-HSR-Plan.*`、`China-HSR-Map.html`、`Hotel-Selection.html`、`Reservation-Audit.en.md`、`TWAC-Arrival-Card-Runbook.md` 含明显旧状态。
  5. 南京独立计划与 Master/generated HTML 的 9/15–9/16 分配冲突；9/16 新的“全日豪华洗浴中心”研究尚未到达。
  6. 台北美食图宣称“一图 40 家”，但实际仅 26 个点有 marker；10 个奶茶全部没有地图 marker。

---

## 1. 文件资产清单

### 1.1 工作区总体

`C:\Users\qitxu\OneDrive\Documents\Self\2026-07-05AsiaTravel\`

| 类别 | 数量 | 说明 |
|---|---:|---|
| Markdown source/research | 24 | 包含 Master、城市计划、预约、酒店、现金、eSIM、TWAC、研究 |
| Generated HTML | 12 | 主行程、预约、酒店、HSR、现金、eSIM、地图 |
| Root images | 10 | 多数为证件/参考图片；不应进入公开部署 |
| PDF/XLSX binary documents | 10 | 含证件、航班、TWAC 等高敏感文件 |
| CSV data | 2 | 台北美食与上海清单 |
| Backups | 15 | 五组文件的时间戳备份 |
| `itinerary-images\` | 44 | 当前照片资产 |
| `itinerary-images-candidates\` | 14 | 九份/阳明山候选图 |
| `itinerary-maps\` | 111 | 12 日图、40 food 图、57 其他 PNG、2 JSON |
| `private\tickets\` | 2 | 高风险票据 PDF；必须永不公开 |
| **合计** | **244** | 研究文件到达后重计 |

### 1.2 权威源 / 生成输出 / 研究 / 备份

**权威主源**
- `Master-Plan.md`
- `Taipei-3Day-Executable-Plan.md`
- `master-plan-hotel.md`
- `Reservation-Audit.md`
- `Nanjing-3Day-Plan.md`
- `Suzhou-Day-Plan.md`
- `Shanghai-Hongqiao-Arrival.md`
- 权威状态应以用户本次给定状态优先于文件。

**生成输出**
- `Taipei-China-Itinerary.html`
- `Reservations.html`
- `Hotel-Selection.html`
- `China-HSR-Plan.html`
- `China-HSR-Map.html`
- `Cash-Exchange.html`
- `esim.html`
- `Taipei-Food-Map.html`
- `Taipei-Hotels-Map.html`
- `Nanjing-Day1-Map.html`、`Nanjing-Day2-Map.html`、`Nanjing-Day3-Map.html`

**关键生成器**
- `build_itinerary_v2.py`：当前主生成器，2026-08-30 22:44，约 98 KB。
- `build_food_map_v2.py`：台北 food map。
- `build_hotels_map.py`、`build_hsr_map.py`
- `render_bilingual_html.py`、`render_hotel_html.py`、`render_md_html.py`
- 旧生成器：`build_itinerary.py`。

**重复 / 备份**
- Workspace 有 **15** 个 `.bak-*`。
- `build_itinerary_v2.py` 有 **5** 个当晚备份。
- `build_itinerary_v2.run.log` 为 **0 bytes**，不能作为成功生成证据。
- Pages staging 还保留 6 张当前本地已删除的旧路线照片，以及 7 张旧单点地图。

**研究文件最终到达状态**
- ✅ `Taipei-Visual-Food-Research.md`
- ❌ `Ximending-Shopping-Services.md`
- ❌ `Suzhou-Huqiu-Shantang-Analysis.md`
- ❌ `Zhejiang-Taizhou-Districts-Plan.md`
- ❌ `Nanjing-Luxury-Bathhouse.md`

---

## 2. Requirement Traceability Matrix

状态：✅ Complete / 🟡 Partial / ❌ Missing / ⚠️ Conflict / ⛔ Blocked

| # | Requirement | 状态 | Source files | Public surface | Exact action |
|---:|---|---|---|---|---|
| 1 | 旅行日期 9/9–9/20 | ✅ | Master、generator | local/stage | 无 |
| 2 | DL69 9/9 SEA→TPE | ✅ | Master、Taipei plan、generator | local/stage/live | 无 |
| 3 | MU5098 9/13 TSA→SHA 17:15–18:55 | ✅ | Master、Taipei plan、generator | local/stage/live | 保持 18:55，清除旧 19:05 |
| 4 | DL280 9/20 PVG→SEA | ✅ | Master、generator | local/stage/live | 出发前复核航站楼 |
| 5 | Hotel Resonance 9/10–13 已订 | ✅ | Master、hotel、reservation | 多个 HTML | 公开版只显示“booked”，删除确认标识 |
| 6 | Ben 全程 <10 天 | ⚠️ | Requirements、Master | Master | 当前 9/9–9/20 与“<10 天”不一致；明确该约束已放弃或调整旅客日期 |
| 7 | Mom 至少一段 lie-flat | ⛔ | Requirements、Master | Master | **需用户确认支出**后才能升舱/重出票 |
| 8 | Josh 台湾许可状态 | ✅ | Master、permit docs | 不应公开细节 | 公开版只写“entry documents ready” |
| 9 | TWAC 离境日期/航班 | ⚠️ | TWAC runbook/XLSX | 本地 | 从旧 9/12 待订改为 9/13 MU5098；重新生成草稿 |
| 10 | 中国 240h transit 合规 | 🟡 | Requirements、Master | Master | 出发前按实际口岸/联程票再次官方核验 |
| 11 | NPM 5 张门票+龙藏经已付款 | ✅ | Taipei plan、Reservation zh、generator | local HTML | 状态正确 |
| 12 | NPM 09:00 入馆、09:00–11:45、12:00–13:00 | ✅ | Taipei plan、generator | local/stage | 无 |
| 13 | 票据 PDF 只留本地，绝不公开 | ⚠️ | Taipei plan、generator | local/stage HTML | 删除所有 `tickets/` 与 `%23en_US.pdf` href；部署 denylist |
| 14 | NPM English docent | ⛔ | reservation、generator | local/stage | 官网不可用；保留 audio guide fallback |
| 15 | Sep 4 NPM 完成记录 | ✅ | generator/local HTML | local only | 发布前先安全净化 |
| 16 | Sep 5 airport comparison + Klook recommendation | ✅ | Airport-Transfer-Compare、generator | local | 无 |
| 17 | 实际购买机场接送 | ⛔ | reservation、Taipei plan | local/stage | 涉及付款，需用户确认；建议最晚 9/8 |
| 18 | 9/10 TPE→hotel→宁夏 | ✅ | Taipei plan、Master、generator | local/stage | 无 |
| 19 | 9/11 NPM→101→鼎泰丰→临江 | ✅ | Taipei plan、generator | local/stage | 无 |
| 20 | 9/12 当前主线 | 🟡 | Master/Taipei plan/generator | local/stage/live 不同 | 本地已选北投；Master 部分旧段仍混入九份；统一为北投主线、九份/阳明山备选 |
| 21 | 9/13 西门→TSA→SHA→七宝 | ✅ | Taipei plan、Master、generator | local/stage | 无 |
| 22 | 著名夜市体验 | ✅ | Taipei/Nanjing plans | local/stage | 无 |
| 23 | 各地特色食品 | 🟡 | Master、city plans、food data | itinerary/food map | 先修点位与分店，再做日程内精选 |
| 24 | Dedicated shopping day / luggage | 🟡 | Requirements、Master | itinerary | 目前仅西门半日+上海早晨；确认是否足够 |
| 25 | 中国典型全日 spa/bathhouse | ❌ | Requirements | 无 | 完成 `Nanjing-Luxury-Bathhouse.md` 并决定是否替换 9/16 |
| 26 | 9/13 虹桥/七宝 | ✅ | Master、Shanghai plan、generator | local/stage | 无 |
| 27 | 9/14 苏州 | ✅ | Suzhou plan、Master、generator | local/stage | 保留拙政园/苏博/观前/网师园 |
| 28 | 9/15 南京抵达/秦淮 | ⚠️ | Master vs Nanjing plan | local/stage | Nanjing plan Day 1 仍写钟山；按 Master 重排独立计划和三张南京地图 |
| 29 | 9/16 南京全日 | ⚠️ | Master vs Nanjing plan | local/stage | 独立计划写总统府/南博/玄武湖/秦淮，Master 写钟山+城中；且 bathhouse 新要求未整合 |
| 30 | 9/17 南京→浙江台州 | ✅ | Master、Nanjing plan、generator | local/stage | 订票前复核车次 |
| 31 | 9/18 浙江台州 | 🟡 | Master、generator | local/stage | 缺 district-level research；府城墙明确 no-climb |
| 32 | 9/19 上海 central skyline | ✅ | Master、Shanghai plan、generator | local/stage | 无 |
| 33 | 9/20 PVG | ✅ | Master、generator | local/stage | 保留大缓冲 |
| 34 | HSR 路线文档与当前主线一致 | ⚠️ | `China-HSR-Plan.*`、map | public | 删除主动杭州段；重画南京→浙江台州直达主线 |
| 35 | 大陆高铁/酒店实际预订 | ⛔ | Master、reservation、hotel | public status | 涉及付款；用户确认后购买，购买前可自主完成候选/核价 |
| 36 | 酒店文档/HTML 双语一致 | ⚠️ | hotel md/en/html | public | 重新生成；当前 HTML 中文块仍是上海→杭州→南京→苏州旧线 |
| 37 | Mom no-climb | ✅ | Nanjing/Suzhou/Taipei、generator | local/stage | 保持 |
| 38 | 一律使用“浙江台州”消歧义 | 🟡 | Master、HSR、generator | public | 标题/首现统一写“浙江台州”；站名可保留“台州站” |
| 39 | 每日恰好一张编号合并地图 | 🟡 | generator/local assets | local 有、stage/live 无 | 部署 12 张 `day-2026-09-DD.png`，并验证每个 day 仅一个 `figure.daymap` |
| 40 | 不要 mini static maps | ⚠️ | generator/assets/stage/live | stage/live | 当前 generator 不渲染 mini map，但保留 57 张其他 PNG；stage/live 仍渲染旧 mini maps；清理部署集合 |
| 41 | Taipei Food：food+milk tea 一图、不同颜色 | ⚠️ | food generator、JSON、research | public map | 40/40 有记录，但仅 26 marker；10 奶茶 marker 为 0。为每个连锁选择具体分店，food/drink 两色，provenance 改 badge |
| 42 | 九份/阳明山图片 | 🟡 | image assets、new research | live/候选目录 | 已有 Jiufen、Qingtiangang、Xiaoyoukeng、Zhuzihu；需按 license/无障碍说明集成备选卡 |
| 43 | Active 与 archived routes 分离 | ⚠️ | Master、HSR、live、assets | public | 杭州只进 Archive，不得出现在当前路线、绿色主线或 live daily cards |
| 44 | Reservation zh/en/html 同步 | ⚠️ | Reservation zh/en、HTML | local/stage | 中文本地较新；英文和 staging 仍把 NPM 当待办、保留杭州倒推 |
| 45 | Cash/ATM/DCC/EasyCard | ✅ | cash zh/en/html、Master | public | 核心建议一致；发布版避免具体个人卡账户信息 |
| 46 | eSIM 路线与推荐一致 | ⚠️ | esim zh/en/html、Master | public | eSIM 文档开头仍写厦门/泉州/福州/杭州；Master 首选又与 eSIM 文档首选不同；统一 |
| 47 | PUBLIC SECURITY | ⚠️ | generated/stage/live | public | 见 §6；先净化再发布 |
| 48 | local/stage/live freshness | ⚠️ | 三层输出 | public | 见 §7；需安全构建→stage→部署→live hash/content 验证 |
| 49 | 5 个 pending research 到齐 | ❌ | research files | 不应直接 public | 目前 1/5；完成其余 4 个 |
| 50 | Points/credits optimization | ✅ | Master、session research | Master | 购买仍受用户确认 |
| 51 | Hainan points 请求 | 🟡 | Requirements、旧研究 | 非主线 | 当前路线已不使用 Hainan；明确归档为“不适用当前行程” |
| 52 | Hilton/Aspire hotel strategy | 🟡 | hotel docs、Master | Hotel HTML | 台北完成，大陆仅 shortlist，尚未预订 |
| 53 | 中国末段买行李 | 🟡 | Requirements、Master | itinerary | 仅原则存在；缺 9/19 可执行店铺/时间 |
| 54 | September weather/Plan B | ✅ | Taipei/Master/city plans | itinerary | 无 |
| 55 | Google My Maps/Saved List | ⛔ | MyMaps research | 无 | 无官方写 API；若要 Saved List 需用户登录/手动，或用户在场的 CDP UI 自动化 |

---

## 3. 核心一致性检查

### 3.1 Master vs generated HTML

- 当前 `build_itinerary_v2.py` 与最新 local HTML 已使用：
  - 9/13 虹桥/七宝
  - 9/14 苏州
  - 9/15 南京秦淮
  - 9/16 南京 no-climb
  - 9/17 南京→浙江台州
  - 9/18 浙江台州
  - 9/19 上海 central skyline
  - 9/20 PVG
- 但 `Master-Plan.md` 内仍有历史叙述和旧台北 food/九份段落；应让“锁定主线”成为唯一 active block，其他统一移到明确 Archive。
- local HTML 仍有票据 PDF href，和正文“票据不公开”的声明自相矛盾。

### 3.2 Taipei plan

- NPM 时间、5 张票、两次 QR、MU5098 出发机场均正确。
- 文件本身曾直接链接 `tickets\*.pdf`；票据现存于 `private\tickets\`，纯本地文档可引用，但 renderer 不得带入 public HTML。
- 行末预约汇总仍有旧机场接机价格区间，与 Sep 5 新比较结果不一致。
- 9/12 主体已是北投，但部分总结和 Master 历史块仍把九份/阳明山写成并列主计划。

### 3.3 Reservation

- `Reservation-Audit.md` 与最新 `Reservations.html` 本地版已记录 NPM 完成和 Klook 推荐。
- `Reservation-Audit.en.md` 仍把 NPM/英文导览列为待办，并保留杭州预约倒推。
- staging `Reservations.html` 比本地旧，仍把龙藏经描述为“如果想看”的未完成事项。

### 3.4 Hotel

- `master-plan-hotel.md` 是当前路线。
- `Hotel-Selection.html` 英文上部基本新，但中文块仍明确写旧的“上海→杭州→南京→苏州→上海”。
- 多处 HTML 暴露酒店确认标识；公开输出不需要该字段。

### 3.5 HSR

- `China-HSR-Plan.md/.html` 自称“仅规划不改行程”，并把杭州作为进浙江台州的推荐节点。
- `China-HSR-Map.html` 绿色主线仍是上海→苏州→南京→杭州→台州→上海。
- 这与权威 active route 直接冲突；杭州必须只保留灰色 archived/reference node。

### 3.6 Nanjing

- `Nanjing-3Day-Plan.md`：
  - 9/15 = 钟山
  - 9/16 = 总统府/南博/玄武湖/秦淮
- Master/generator：
  - 9/15 = 抵达+秦淮
  - 9/16 = 钟山+总统府/南博/玄武湖
- 三张南京 standalone map 因此不能视为当前权威日图。
- 新要求“9/16 全日豪华 bathhouse pending research”尚未进入任何 active 日程。

### 3.7 Cash / eSIM / TWAC

- Cash：ATM、拒绝 DCC、EasyCard 现金加值方向一致，只有金额区间在不同摘要中略有差异。
- eSIM：核心技术建议可用，但路线首段仍是已废弃的东南沿海/杭州路线；首选产品在 Master 与 eSIM 文档之间不一致。
- TWAC：仍使用 9/12 离台和“航班待订”；必须改成 9/13 已订航班。

---

## 4. 每日 9/9–9/20 覆盖表

`actual stops` 来自当前 `build_itinerary_v2.py`；地图均已在本地生成，但未进入 staging/live。

| 日期 | Expected | Actual stop order | Combined map | Photos/assets | Error |
|---|---|---|---|---|---|
| 9/9 | SEA→DL69 | Seattle → SEA | `day-2026-09-09.png` | 2/2 | 无 |
| 9/10 | TPE→hotel→宁夏 | TPE → Taipei city → Ningxia | `day-2026-09-10.png` | 3/3 | 酒店未作为独立 stop |
| 9/11 | hotel→NPM→101→临江 | Taipei city → NPM → Taipei 101 → Linjiang | `day-2026-09-11.png` | 4/4 | 正确 |
| 9/12 | 北投→地热谷→饶河 | Taipei city → Beitou hot spring → Thermal Valley → Raohe | `day-2026-09-12.png` | 2/4 | 缺北投、地热谷照片；九份/阳明山仅为备选 |
| 9/13 | 西门→TSA→SHA→虹桥→七宝 | Taipei city → Ximending → TSA → SHA → Hongqiao → Qibao | `day-2026-09-13.png` | 6/6 | 正确 |
| 9/14 | 虹桥→苏州园林日 | Hongqiao → Suzhou Station → hotel → 拙政园 → 苏博 → 观前 → 网师园 → 平江 → hotel | `day-2026-09-14.png` | 景点/车站有图；hotel/观前缺 | stop 首尾 hotel 重复属路线语义，可接受 |
| 9/15 | 苏州→南京→秦淮 | Suzhou hotel → station → Nanjing South → hotel → Qinhuai → Laomendong → hotel | `day-2026-09-15.png` | hotel 缺图 | 与 Nanjing standalone Day1 冲突 |
| 9/16 | 南京 full day | hotel → 明孝陵神道 → 美龄宫 → 总统府 → 南博 → 玄武湖 → hotel | `day-2026-09-16.png` | hotel 缺图 | 未包含 bathhouse；行程可能过满 |
| 9/17 | 南京→浙江台州 | hotel → 颐和路 → 南京南 → 台州站 → hotel → 椒江 seafood | `day-2026-09-17.png` | hotel 缺图 | 车次待购买前复核 |
| 9/18 | 浙江台州 | hotel → 临海 → 紫阳街 → fishing port → 椒江 seafood → hotel | `day-2026-09-18.png` | hotel/fishing port 缺图 | district research 缺；府城墙需明确不登城 |
| 9/19 | 浙江台州→上海 central | hotel → 台州站 → 虹桥 → central hotel → Bund → Lujiazui → hotel | `day-2026-09-19.png` | hotel 缺图 | 正确 |
| 9/20 | 上海 central→PVG | central hotel → PVG | `day-2026-09-20.png` | hotel 缺图 | 正确 |

**地图结论**
- 本地有 **12/12** 每日合并地图，顺序编号由 generator 生成。
- staging/live 有 **0** 个 `figure.daymap`。
- local HTML 有 12 个 `figure.daymap`、87 张 card；staging 仍是 0 daymap、65 card；live 是 0 daymap、100 card。

---

## 5. Maps / Photos / Assets

### 5.1 Daily maps

- 本地 12 张日图均在 22:45 同批生成。
- staging 缺全部 12 张日图。
- `itinerary-maps\` 仍有 **57** 张非 day/non-food PNG，其中包含当前/归档单点静态地图。
- 当前 generator 的 `card()` 已将 `map_html` 置空，所以最新 local HTML 不显示 mini maps；但旧资产和旧发布面仍存在。

### 5.2 Taipei Food map

- JSON 有 **40** 条：30 food + 10 milk-tea/drink。
- generator 只给 `precise` / `district` 记录 marker：**26** 个。
- **14** 个 city-level chain 只显示文本，其中 **10 个奶茶全部无 marker**。
- 图例显示奶茶颜色/图层，但图层为空，属于用户可见的错误承诺。
- 新研究指出多处 area/coordinate 冲突、共享假 centroid 和分店不明确。
- 正确模型：`type=food|drink` 决定颜色；“Jensen Huang”应为 provenance badge，不应作为第三类颜色。

### 5.3 Jiufen / Yangmingshan

- 已有：
  - `jiufen.jpg`
  - `qingtiangang.jpg`
  - `xiaoyoukeng.jpg`
  - `zhuzihu.jpg`
  - 另有 14 个候选图片。
- `Taipei-Visual-Food-Research.md` 已提供 license、attribution、无障碍与天气提示。
- 当前 9/12 active 为北投，因此这些图应放在“备选路线卡”，不能暗示 active。
- 当前 active 北投反而缺 `beitou_hotspring` 与 `thermal_valley` 图片。

### 5.4 Active vs archived

- Staging 比 local 多 6 张旧路线照片：杭州/龙井/虎丘/山塘/中山陵/纪念馆类资产。
- Staging 比 local 多 7 张旧单点地图。
- Live 仍用杭州 active cards。
- 建议发布目录只从 manifest allowlist 构建，不要复制整个 assets 目录。

---

## 6. PUBLIC SECURITY

### 6.1 已确认风险

| Surface | Pattern/category | 风险 | Sanitization |
|---|---|---|---|
| local generated itinerary | `tickets/`、`%23en_US.pdf` href 共 4 处 | 高 | renderer 永久丢弃 PDF/ticket href |
| Pages staging itinerary/index | 同类 href 共 8 处 | 高 | 发布前删除；禁止复制 `tickets\` |
| ticket PDFs | QR/barcode、票/订单标识、联系信息 | 极高 | 仅保存在私有本地目录；绝不进入 repo/staging |
| Hotel/Reservations/itinerary HTML | 酒店确认标识 | 中 | 公开版只保留 booked/status，不显示代码 |
| live itinerary | 旅客身份/证件类别、精确航班住宿关联 | 中 | 改为家庭级泛化信息；移除姓名、证件类别与过度精确组合 |
| workspace root | ID images、permit/booking PDFs、prefilled workbook | 极高 | 与 public build source 分离；部署只允许明确列出的 HTML/JPG/PNG |
| public/staging HTML | 精确个人联系方式模式 | 中 | 删除电话/邮箱；公共服务电话如确需保留必须单独 allowlist |

### 6.2 没有发现 / 当前 live 状态

- 当前 live itinerary 未发现 `tickets/` 或 `%23en_US.pdf` 链接；但这不降低 staging 的部署风险。
- ticket URL 在 staging 当前可能返回 404；一旦以后误复制 PDF，就会成为可兑换票据泄露。

### 6.3 必须采用的发布安全模型

1. Public build 使用 **allowlist manifest**。
2. 默认拒绝：`*.pdf`、`*.xlsx`、`*.csv`（除非明确公开）、`tickets\`、`id-*`、`*permit*`、`*confirmation*`、`*.bak-*`、profile/cache。
3. HTML sanitizer 删除：
   - ticket/PDF href
   - 酒店/订单/确认标识
   - 个人邮箱/电话
   - 姓名+证件/国籍+完整旅行路线的组合描述
4. 发布后重新抓取 live 并扫描；不能只检查本地源。

---

## 7. Freshness：local vs staging vs live

### 7.1 文件层

- `Taipei-China-Itinerary.html`
  - local：22:45
  - staging：22:32，hash 不同
  - live：HTTP 200；`Last-Modified` 为 2026-08-30 18:22 PDT 左右；与 local/staging 均不同
- `Reservations.html`
  - local：22:38
  - staging：22:11，hash 不同
- Hotel/HSR/Cash/eSIM/Food/Hotel-map：local 与 staging hash 相同，但不代表内容正确。

### 7.2 内容层

| Surface | Route | NPM Sep4 state | Daily combined maps | Ticket links |
|---|---|---|---:|---:|
| local | 当前无杭州主线 | 已完成 | 12 | 4 |
| staging | 当前无杭州主线 | 旧/不完整 | 0 | 8 |
| live | **旧杭州主线** | 旧“optional” | 0 | 0 |

**结论：** local 最新但不安全；staging 较旧且更不安全；live 最旧但目前没有 ticket href。任何直接“把 local 全量复制到 Pages”都会把票据链接公开，必须先净化。

---

## 8. True blockers / User decisions

### 真正需要用户确认

1. **Mom cabin spending**：升舱/重出票需要支出确认。
2. **所有付款动作**：机场接送、大陆 HSR、酒店、门票/洗浴中心。
3. **My Maps Saved List**：需要用户登录/手动操作，或用户在场允许 CDP 驱动。
4. **English docent**：官方入口不可用；若坚持真人英文导览，需要选商业产品并确认付款。

### 可自主完成，不应等待用户

- 清理 active/archive 信息架构。
- 同步 Master、city plans、Reservation zh/en/html、hotel zh/en/html、HSR。
- 更新 TWAC 草稿中的日期/已订航班字段。
- 完成其余 4 个研究文件。
- 修 food point data 和图片 attribution。
- 修改 generator 去除 ticket href 与敏感字段。
- 构建 allowlist、生成 staging、自动安全扫描和一致性验证。
- 研究南京 bathhouse 候选、价格、营业时间、5 人/长辈适配；只在付款/最终替换时问用户。

---

## 9. Top 10 Gaps

1. **Live 仍发布杭州 active route。**
2. **Staging/local 存在 ticket PDF 链接，发布即有高风险。**
3. **酒店确认标识和旅客/证件组合信息在 public HTML 过度暴露。**
4. **12 张每日合并地图未发布；live/stage 仍是 mini-map 体系。**
5. **9/16 南京 bathhouse 新要求完全缺失。**
6. **Nanjing standalone plan/maps 与 Master/generator 的 9/15–16 对调冲突。**
7. **HSR plan/map 仍把杭州列为推荐 active node。**
8. **Taipei Food Map 的 10 个奶茶没有 marker，多个 food 坐标/区域冲突。**
9. **TWAC 仍写 9/12 离境/航班待订。**
10. **Reservation English/staging、Hotel HTML 中文块、eSIM route 均为旧状态。**

---

## 10. Ordered Integration Checklist

1. **Freeze authority**
   - 以本次用户权威状态建立单一 machine-readable itinerary source。
   - Active route 固定：Taipei → Hongqiao/Qibao → Suzhou → Nanjing → 浙江台州 → central Shanghai → PVG。

2. **Security first**
   - 修改 `build_itinerary_v2.py`：删除所有 ticket/PDF href、确认标识、个人联系/证件细节。
   - 为 public build 增加 allowlist/denylist。
   - 验证：public tree 中 PDF/XLSX/tickets/ID/backup 数量必须为 0。

3. **Resolve 9/16**
   - 完成 `Nanjing-Luxury-Bathhouse.md`。
   - 提供 A：全日 bathhouse；B：现有南京 classics；不要混成不可执行的超满日。

4. **Synchronize city sources**
   - 更新 `Nanjing-3Day-Plan.md` 与三张南京 map。
   - 完成 `Zhejiang-Taizhou-Districts-Plan.md`。
   - 完成苏州和西门研究并整合。

5. **Fix reservations/TWAC**
   - 同步 `Reservation-Audit.md`、`.en.md`、`Reservations.html`。
   - 更新 `TWAC-Arrival-Card-Runbook.md` 和 prefilled workbook 为 9/13 MU5098。

6. **Fix hotel/HSR**
   - 从 `master-plan-hotel.md` 重新生成完整双语 `Hotel-Selection.html`。
   - 重写 `China-HSR-Plan.md/.html` 与 `China-HSR-Map.html`；杭州只放 Archive。

7. **Fix maps**
   - 保持每个 9/9–9/20 day 恰好一个编号 combined map。
   - 不再发布 mini static map 资产。
   - 验证 DOM：12 个 day、12 个 `figure.daymap`、每 day=1。

8. **Fix photos**
   - 补北投/地热谷 active 图片。
   - 集成九份/阳明山候选图片及 attribution，仅放 fallback。
   - 可给酒店 stop 使用统一非敏感 hotel icon，不必重复酒店照片。

9. **Fix Taipei Food Map**
   - 为 14 个 chain 选 itinerary-nearby branch。
   - 40/40 marker；food 与 milk tea 两色；provenance badge；删除假 centroid。
   - 验证 marker count=40，drink marker count=10。

10. **Safe build and publish**
    - 生成到全新 clean staging，而不是增量覆盖旧目录。
    - 扫描敏感 pattern。
    - 比较 local clean build 与 staging hash。
    - 发布后抓取 live，验证路线、12 日图、NPM 状态、无 ticket/PDF/确认标识、无杭州 active cards。

---

## 11. 最终判定

当前 workspace 的**最新本地主行程已接近权威路线**，但不能直接发布：公共安全、旧路线残留、双语输出不同步、南京日期冲突、每日地图未部署和 food marker 缺失均属实质性问题。  
应先执行“安全净化 → 权威源统一 → city/research 整合 → clean rebuild → staging scan → live verification”，再把 live 视为可用的旅行执行面。
