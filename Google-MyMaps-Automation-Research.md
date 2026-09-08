# Google 地图「列表/自定义地图」自动化 — 方法研究（含 Saved Lists 与 My Maps）

> 研究日期 2026-08-16/17。目标：把「可能要去的地方」批量标进 Google 地图。**你要的是 Maps「已保存→列表」(Saved Lists)——见 §0.5（重点）**；My Maps（自定义地图）作为唯一可批量导入的官方替代，见 §0 起。你已在 **Chrome 登录 Google 账号**。链接自 `Master-Plan.md`。

## 0. 结论先讲（TL;DR）
- **Google My Maps 没有官方「写入 API」** → 不能像 Gmail 那样用 OAuth+API 直接建图/加点。
- 要拿到「类似 gmail 那样的自动化权限」，只能复用你银行门户用的那套 **Chrome CDP 挂到你已登录的会话** 去操作 mymaps 网页。
- **最省事**：直接把已生成的 `Taiwan-Food-MyMaps.csv` 手动导入 My Maps（5 分钟、零风险）。
- **要自动化/可复现**：搭 **Chrome CDP（方法 2）**。
- **要自托管可控地图**：用 **Maps Platform API + key（方法 3）**，但产物不是 My Maps。

## 0.5 ⭐ 你截图的是「已保存 → 列表」(Saved Lists)：API 现状（2026-08-16 核实）
> 你指的是 Google 地图 App/网页 **已保存 → 列表**（korea / Seattle / NOLA … 这种 `Private/Shared · N places`），**不是** My Maps 自定义地图。两者是不同产品：Saved Lists 原生同步到手机、导航时直接显示；My Maps 是叠加的自定义地图（手机在「已保存 → 地图」页看）。

| 能力 | 官方 API? | 说明 |
|---|---|---|
| **建/改 Saved List、往列表加地点** | ❌ **无**（也无 OAuth scope）| 出于隐私安全，只锁在 Maps App/网页 UI；Places/Maps JS API 只能查/显示地点，**碰不到你的个人已保存列表**。多个 Stack Overflow 佐证。 |
| **导出 Saved Lists** | ⚠️ **只出不进** | **Google Takeout** → 「Maps（你的地点/已保存）」可导出 KML/JSON/GeoJSON，仅备份，**不能回写/再同步**。 |
| **批量加地点到列表** | ❌ **无官方途径** | 加点是 UI 手动动作。变通均非官方（见下）。 |

**结论：Google 对「Saved Lists」没有可写 API。** 想批量搞定只有这几条路（都不完美）：
1. **手动加**（最稳）：Maps 里搜每个店 → **Save/保存** → 选列表。40 家逐个点，可靠但费时。
2. **浏览器自动化点 UI**（connector 式 Chrome CDP，非官方）：脚本执行「搜地点→保存→选列表」×40。**不受支持、脆、可能违反 Google ToS，且会遇到我们刚碰到的反自动化拦截**（headless 被静默拦；须**非 headless 真实 Chrome**、你在场时重试）。消费版 Maps 的「保存」流程或许比 My Maps 编辑器好点，但仍有风险。
3. **改用 My Maps 批量导入**（可批量、但产物不同）：`Taiwan-Food-MyMaps.csv` 一次导入 40 家（见下方方法 1）——它是「自定义地图」不是 Saved List，但**手机同样能看**、且是**唯一真正支持批量**的官方入口。
4. **Maps URLs API**：只能生成路线/查看链接（≤10 途经点），**不是**列表。

**给你的建议**：要「就是那种 Saved List」→ 只能 手动加(1) 或 我帮你浏览器自动化(2，你在场重试非 headless)。要「省事且批量」→ 直接用 My Maps 导 CSV(3)。

## 1. 先看你本机现有两套「鉴权范式」（决定 My Maps 能走哪条路）
| 自动化 | 鉴权方式 | 为什么能这样 | 位置/引用 |
|---|---|---|---|
| **Gmail（emailer / gmail-checker）** | **官方 Gmail API + OAuth2**（每邮箱 token；`python src/auth.py --setup --mailbox <id>` 一次性 Google 授权；门户 localhost:8402） | **Gmail 有公开 API** | `~/.copilot/agents/emailer.agent.md`；`${REPO_PERSONAL}/gmail-checker/`（`config/mailboxes.json`、`config/tokens/`） |
| **银行/信用卡门户（connector）** | **Chrome CDP 端口 9223 + 专用 profile**（`$LOCALAPPDATA/Google/Chrome/CDP-Profile`），用户先登录，agent 用 `websocket`(`suppress_origin=True`) 挂上 | **门户没有 API**，只能驱动登录后的网页 | `~/.copilot/agents/connector.agent.md` |
| 其它门户（read-private-url / taobao / scorm / viva / flight-prices） | **Edge/Chrome CDP** 持久 profile（headless） | 同上 | `~/.copilot/skills/read-private-url/SKILL.md`（Edge 9223）、`update-taobao-inventory`（9222） |

**过往踩坑（`session a719f872/plan.md`，Amex/Akamai）**：
- Playwright **直接启动**的浏览器会被 bot 检测拦 → 正解是 **CDP 挂到「用户自己正常启动并登录」的干净 Chrome**（独立 profile、`127.0.0.1`）。
- Chrome **不能对「默认 profile 目录」开 CDP** → 必须用**专用 CDP profile 目录**。
- 检测「是否登录」要**排除 2FA/verify/mfa 页面**，否则会在你输验证码时把页面导走。

## 2. 关键事实：My Maps 没有写入 API
Google Maps Platform 提供的是 **Geocoding / Places / Maps JavaScript / Maps Static / Routes** 等——**没有任何「创建/编辑 My Maps 自定义地图」的公开 API**。My Maps 文件虽然存在你的 Google Drive 里（MIME 类型 `application/vnd.google-apps.map`），但 **Drive API 只能列出/复制/分享/删除，改不了地图里的点/图层**。
→ 所以 Gmail 那种「拿 OAuth token 调 API」的模式对 My Maps **不成立**；要自动化只能操作网页 UI。（此点为长期公认事实；如需可再查一次官方开发者文档确认。）

## 3. 方法清单（按推荐度）

### 方法 1 —— 手动 CSV/KML 导入（0 代码 · 最稳 · ~5 分钟）✅ 默认推荐
已按官方导入规则生成好文件：`Taiwan-Food-MyMaps.csv`（40 家：餐廳12/黃仁勳美食地圖18/奶茶10；UTF-8 BOM；含 `Location` 地理编码搜索列）。
**步骤**：
1. 电脑浏览器登录 **https://mymaps.google.com** → **创建新地图**。
2. 图例里 **添加图层** → 命名（如「台北美食」）→ **导入** → 上传 `Taiwan-Food-MyMaps.csv` → **Select**。
3. 选**定位列** = `Location`（Google 按名称/地址自动地理编码），**标题列** = `Name`。
4. 图层菜单 **按 `Category` 分样式/分色**（或每个 Category 单独一个图层，便于开关）。
5. 有个别点定位偏 → 点该标记手动拖到正确位置。
**限制（官方）**：每次导入 ≤ **2,000 行**；支持 CSV/TSV/KML/KMZ/GPX/XLSX/Google Sheet；每张地图最多 **10 个图层**。
**优点**：零风险、最快；**缺点**：非自动化（下次更新要重导入，可用「Reimport and merge」保留样式）。
**手机查看**：Google 地图 App → **已保存 → 地图** 里能看到这张 My Maps，导航时可参考。

### 方法 2 —— Chrome CDP 挂已登录会话，脚本化操作 My Maps UI（可自动化 · connector 式）
**思路**：跟 `connector` 抓银行门户完全同一套——用**专用 CDP profile** 启 Chrome，你登录一次 Google，agent 用 CDP 脚本化：开 mymaps → 建图/图层 → 触发导入 → 传 CSV → 设列/样式。
**启动（专用 profile，别用默认 profile 目录）**：
```
"C:\Program Files\Google\Chrome\Application\chrome.exe" ^
  --remote-debugging-port=9223 --remote-allow-origins=* ^
  --user-data-dir="%LOCALAPPDATA%\Google\Chrome\CDP-Profile" ^
  --no-first-run --disable-blink-features=AutomationControlled about:blank
```
**连接**：`websocket-client`，`create_connection(WS_URL, suppress_origin=True)`；`Runtime.evaluate` 驱动页面。
**注意/坑**：
- **默认 profile 不能开 CDP**（Chrome 安全限制）→ 用上面的 `CDP-Profile`，**首次要你在该窗口登录一次 Google**（含 2FA）。
- **别用 Playwright 直启浏览器**（会被 bot 检测）——挂到你已登录的 Chrome。
- **上传 CSV** 的文件选择器：用 CDP `DOM.setFileInputFiles` 直接塞文件路径（网页 file input 自动化的标准做法）。
- My Maps UI 无稳定元素 id，选择器**较脆**、Google 改版会失效；每步之间要 `wait`。
**工作量**：中（写一段 CDP 脚本 + 调 UI 选择器）；**风险**：UI 变更/偶发失败，需重试与人工兜底。
**产物**：真正写进**你 Google 账号的 My Maps**（这就是「类似 gmail 权限」的等价物）。

### 方法 3 —— Maps Platform API + API key（产物不是 My Maps，但完全可控/可复现）
用 **Geocoding API** 把每个地名解析成经纬度，再用 **Maps JavaScript**（可交互）或 **Maps Static**（图片）自建地图，嵌进网页/HTML。
**前置**：Google Cloud 项目 + 启用相应 API + **API key + 结算账户**（有免费额度/月度赠额）。
**优点**：可编程、可复现、完全自控（就像本行程 HTML 的地图已用**免费 OpenStreetMap** 实现的那样）。
**缺点**：**不是** My Maps、不进你的 Google 账号收藏；要管理 key/计费。
**说明**：如果你其实只想「有张能分享、带标记的地图」，我们已经用免费 OSM 给行程做了 `itinerary-maps\`，同理可给这些美食点出一张自托管地图，**无需任何 Google key**。

### 方法 4 —— Google Drive API（只能管理/分享，改不了内容）
My Maps 在 Drive 里是 `application/vnd.google-apps.map`。用 Drive API（OAuth，和 Gmail 同源）可**列出/改名/复制/分享/删除**这些地图，但**不能编辑地图里的点或图层**。→ 仅用于「批量分享/整理已存在的地图」。

### 方法 5 —— Google 地图 App「已保存 → 列表」（手机 · 手动）
比 My Maps 更轻：在 Google 地图 App 建个**列表**（如「Taipei Eats」），逐个搜索收藏。**无批量导入**（要手动加），但**同步到手机、导航时直接显示**，适合作方法 1 的补充。

## 4. 方法对比表
| 方法 | 是否自动化 | 是否写进你的 My Maps | 前置 | 工作量 | 风险 | 何时选 |
|---|---|---|---|---|---|---|
| 1 CSV/KML 手动导入 | 否 | ✅ 是 | 无（CSV 已备好） | 极低 | 极低 | **默认，先做这个** |
| 2 Chrome CDP 挂登录会话 | ✅ 是 | ✅ 是 | 专用 CDP profile + 登录一次 | 中 | UI 脆 | 想自动化/可复现 |
| 3 Maps Platform API+key | ✅ 是 | ❌ 否(自建地图) | GCP 项目+key+计费 | 中 | key/计费管理 | 想自托管可控地图 |
| 4 Drive API | ✅ 是 | ⚠️ 仅管理不编辑 | OAuth 授权 | 低 | 低 | 批量分享/整理 |
| 5 Maps App 列表 | 否 | ⚠️ 是「列表」非 My Maps | 无 | 低 | 无 | 手机上随手收藏 |

## 5. 建议路线
1. **现在**：用**方法 1** 把 `Taiwan-Food-MyMaps.csv` 导进一张 My Maps（最快最稳）。
2. **若要自动化/以后常更新**：搭**方法 2**（复用 connector 的 Chrome-CDP profile；我可以写好脚本，你只需在该窗口登录一次 Google）。
3. **若要一张自托管、可嵌网页的地图**：用**方法 3** 的思路，但可先用**免费 OSM**（无需 Google key）先做，和行程 HTML 同款。

## 6. 相关文件与引用
- 导入数据：`Taiwan-Food-MyMaps.csv`（本资料夹）。
- 官方导入规则（已核实 2026-08-17）：Google My Maps 帮助「Import map data to a layer」（CSV/TSV/KML/KMZ/GPX/XLSX/Sheet；≤2,000 行/次；需含 经纬度/地址/地名/WKT 列）。
- 本机范式来源：`agents/emailer.agent.md`（Gmail API/OAuth）、`agents/connector.agent.md`（Chrome CDP 9223）、`skills/read-private-url/SKILL.md`（Edge CDP）、`session-state/a719f872…/plan.md`（CDP 挂登录会话的踩坑）。
