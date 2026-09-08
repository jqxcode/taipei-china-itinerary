# TWAC 台灣入境卡 — 操作手冊 + 資料準備清單（全家 5 人）

**用途：** 入境台灣前必填的線上入境卡（TWAC，2025/10/1 起取代紙本）。此文件已把官方流程 + 全部欄位 + 下拉選項代碼摸清，做成「**到日子照著點就能秒填**」的 runbook。
**官網：** https://twac.immigration.gov.tw/ （**免費**；官方絕不會收費/要信用卡）
**建立：** 2026-07-26 · 依據：官方 UserManual_Eng.pdf(48頁) + FAQ(29題) + Excel 批量模板代碼表（皆已下載存本資料夾）

---

## ⏰ 什麼時候能填（關鍵）
- 官方規則：**入境前 7 天內**（含入境當天）才開放提交，**不能更早**。
- 本次台北入境 = **9/10（DL69 抵達）** → **最早約 9/4 起** 可填，到登機/入境前完成即可。**現在（7月）系統不接受。**
- 例：官方 FAQ 舉例「10/7 入境 → 10/1 起可填」，即 入境日−6 天。

## 👥 誰要填（本團 5 人全部都要，各自一張）
| 旅客 | 要填? | 國籍選項 | 簽證類型 | 備註 |
|---|---|---|---|---|
| Josh 徐起同（中國護照+入台證）| ✅ 要 | `CHN,PEOPLE'S REPUBLIC OF CHINA` | **入國許可證號** | 簽證號填**入台證號**（核准後才有）；大陸居民持一年多次證、觀光目的→強制要填 |
| Mom / Joylene Castro | ✅ 要 | `USA,UNITED STATES` | **免簽證 Visa-Exempt** | 免填簽證號 |
| Ben / Benjamin Castro | ✅ 要 | `USA,UNITED STATES` | 免簽證 Visa-Exempt | |
| Mani / Keanuu Perez | ✅ 要 | `USA,UNITED STATES` | 免簽證 Visa-Exempt | |
| Fred Perez-cruz | ✅ 要 | `USA,UNITED STATES` | 免簽證 Visa-Exempt | |
> 嬰幼兒也要各填一張。持台灣居留證(ARC)/居留簽證者才免填——本團無人適用。

---

## 🚀 當天最快流程（建議：網頁版 + 護照 OCR 自動帶入）
1. 開 https://twac.immigration.gov.tw/ → 右上可切語言 → 點 **「Submit Arrival Card」** → 讀 Notice → **START**。
2. **Email 驗證**：輸入 `jqx2014@gmail.com` → SEND VERIFICATION CODE →（驗證碼 **5 分鐘**有效）到信箱拿碼 → VERIFY CODE。
3. **旅客資料**（一次可錄 **10 人**，全家 5 人一次搞定）：
   - 第 1 人點 **「UPLOAD YOUR PASSPORT」** 上傳護照照片 → 系統 OCR 自動帶入姓名/護照號/生日/國籍/效期 → CONFIRM（可再手改）。
   - 補：**Occupation（職業）**、**Country/Region Code + Mobile（電話，美國 +1）**、Email。
   - 點 **「+ADD TRAVELER」** 加下一位；第 2 人起可勾 **「Same as Lead Traveler」**把電子文件寄到同一信箱(jqx2014)。全部加完 → **NEXT**。
4. **旅行細節**（逐人）：入境航班、預計離境日、來訪目的、在台住宿。第 2 人起若與第 1 人相同可勾 **「Same as Lead Traveler」**（但**入境航班號**要各自確認/可改）。
5. **Review** → 勾聲明「I have read... and I take full responsibility.」→ **SUBMIT**。
6. 成功後系統寄 **電子文件（含 Arrival Card Number）** 到信箱 → 存好；入境查驗時海關可能要看。
> **填錯免驚**：首頁 **UPDATE** → 輸入 Arrival Card Number + 生日 + 護照號 + 國籍 → 可在入境查驗前隨時改，改完系統重寄新文件（用最新版）。

**替代：Excel 批量匯入（≤16 人）** — Submit 流程裡選匯入 → 上傳填好的 Excel → Review → Submit。人多時較快；本團 5 人用網頁 OCR 已足夠。
> **已預填草稿：`private/TWAC_ACARD_prefilled_DRAFT.xlsx`**（见 `private/`）— 5 人共用欄（入境 DL69/9-10、離境 9/13、MU5098、目的觀光、Email、Mode）+ 國籍/簽證類型都填好；**Josh 整行已填**（缺入台證號）。待補：4 位美籍的護照號/效期/性別/生日/出生地/職業/電話、9/13 離境航班 MU5098；台北飯店已定：Hotel Resonance Taipei, Tapestry Collection by Hilton；No. 7, Linsen S. Rd., Zhongzheng District, Taipei。空白模板 `private/TWAC_ACARD_EXCEL_template.xlsx` 保留未動。
> ⚠️ 匯入前快速確認 3 個格式（我依代碼表填、未實測匯入）：Flight Code＝`DL : Delta Air Lines`、Country/Region Code＝`+1 USA/CAN`、日期＝DD/MM/YYYY。若匯入報錯照官網下拉重選即可。

---

## 📋 每人要填的資料（已知先填好，`❓`=需你提供 / `⏳`=待訂後補）
**共用欄位（5 人相同）：** 入境 Mode=AIR、Flight Code=**DL**、Flight No=**69**（DL69）、入境日=**10/09/2026**(9/10, 格式 DD/MM/YYYY)；離境日=**13/09/2026**(9/13)；來訪目的=**3.觀光 Sightseeing**；Email=**jqx2014@gmail.com**；居住國=USA。

| 欄位 | Josh 徐起同 | 4 位美籍 |
|---|---|---|
| English Name（護照拼音）| XU, QITONG | ❓各人護照上的完整拼音（含 middle name）|
| Chinese Name | 徐起同 | 無（留空）|
| Passport Number | **EJ5146622** | ❓各人美國護照號 |
| Passport Expiry | ❓（護照效期，建議≥入境後6個月）| ❓各人效期 |
| Sex | Male | ❓各人 |
| Date of Birth | **1989/04/11** | ❓各人 |
| Nationality | CHN,PEOPLE'S REPUBLIC OF CHINA | USA,UNITED STATES |
| Place of Birth | 中國/China（浙江）| ❓各人出生地 |
| City/State（居住州）| ❓（如 WA）| ❓（如 WA）|
| Place of Residence | USA | USA |
| Visa Type | 入國許可證號 | 免簽證 Visa-Exempt |
| Visa Number | ⏳**入台證號**（核准後填；收件號115396507590）| —（免）|
| Country/Region Code + Mobile | ❓ +1 電話 | ❓ +1 電話（可全家共用一支）|
| Occupation（見下表選）| ❓ | ❓（Mom / Ben / Mani / Fred 各一）|
| Job Title | 僅當職業選 OTHER 才填 | 同左 |
| 入境航班 | AIR / DL / 69（9/10）| 同 |
| 離境日/航班 | 13/09/2026；AIR / MU / 5098（台北松山→上海虹橋，已確認）| 同 |
| 來訪目的 | 3.觀光 Sightseeing | 同 |
| 在台住宿 | ✅ **Hotel Resonance Taipei, Tapestry Collection by Hilton；No. 7, Linsen S. Rd., Zhongzheng District, Taipei（林森南路7号，中正区）**| 同 |

---

## 🙋 提交前我需要你提供（清單）
1. **4 位美籍的護照資料**：號碼、效期、護照上完整拼音、生日、性別、出生地 —— **或**當天給我 4 本護照照片（用 OCR 自動帶入，最省事）。
2. **5 人各自的職業**（從下方 Occupation 清單挑；若選 OTHER 需再給 Job Title）。學生選 STUDENT、家管選 HOMEMAKER、無業/嬰兒選 NONE/BABY/INFANT。
3. **聯絡手機**（美國 +1，一支即可全家共用）。
4. **台北飯店**（住宿欄必填）：Hotel Resonance Taipei, Tapestry Collection by Hilton；No. 7, Linsen S. Rd., Zhongzheng District, Taipei（林森南路7号，中正区）。
5. **9/13 離境航班已確認：MU5098，台北松山→上海虹橋。**
6. Josh 的 **入台證號** —— 等入台證核准後（收件號 115396507590），我會從核准信取得後填入。
7. 5 人的**居住州**（如華盛頓州 WA）與**出生地**。
> 備妥 1–3 後，即使人在 7 月也可先把資料整理進 Excel 模板；**真正提交要等 9/4 起**。

---

## 🔖 下拉選項代碼速查（官方 Excel 代碼表摘錄）
**Nationality（國籍）：** 美籍=`USA,UNITED STATES`；Josh=`CHN,PEOPLE'S REPUBLIC OF CHINA`；加拿大=`CAN,CANADA`；台灣=`ROC,REPUBLIC OF CHINA(TAIWAN)`。
**Visa Type（簽證類型）：** 免簽證 Visa-Exempt(include TAC)｜持有簽證 Holding a Visa｜落地簽證 Landing Visa/臨時入國｜（台灣籍）未具入國許可 Permit-Exempt / 具入國許可 Permit｜（港澳陸）**入國許可證號**。
**Purpose of Visit（目的）：** 1.商務 Business｜2.求學 Study｜**3.觀光 Sightseeing/Travel/Leisure**｜4.展覽 Exhibition｜5.探親 Visit Relative（要填親屬姓名+電話）｜6.醫療｜7.會議｜8.就業｜9.宗教｜10.其他 Others（要填 Reason）。
**Sex：** Male｜Female。 **Mode of Travel：** AIR｜SEA。 **Accommodation：** Residential Address｜**Hotel Name**｜Transfer（選 Hotel Name 要填飯店名+地址）。
**Occupation（職業，41 項）：** ACCOUNTANT/CPA、ACTOR、ARMY/NAVY、ARTIST、BANKER、BUSINESS/DIRECTOR/CEO、CARETAKER、CLERK/STAFF、CONSULTANT、CREW/SEAMAN、DIPLOMAT、DIVER、DOCTOR/DENTIST、ENGINEER/ARCHITECT、FARMER、FISHERMAN、GOVERNMENT OFFICER、DOMESTIC WORKER、**HOMEMAKER**、**IT**、JOURNALIST、SEAFARER、LAWYER、MECHANIC、MANUFACTURE WORKER、MUSICIAN、NURSE、PILOT、PRIEST、PROFESSOR/LECTURER、RESEARCHER、SALESMAN、SCIENTIST、SECRETARY、SPECIALIST、**STUDENT/SCHOLAR/PUPIL**、TEACHER、TECHNICIAN、WRITER、**NONE/BABY/INFANT（無業/嬰兒）**、OFW、**OTHER（其他，需填 Job Title）**。
**航空公司代碼（Flight Code）常用：** DL=Delta｜CI=China Airlines｜BR=EVA Air｜CA=Air China｜CZ=China Southern｜MU=China Eastern｜MF=Xiamen｜3U=Sichuan｜HU=Hainan｜ZH=Shenzhen。

## 📄 Excel 批量模板 34 欄（`private/TWAC_ACARD_EXCEL_template.xlsx` 工作表1；支援 16 人）
Traveler No｜Date of Entry(DD/MM/YYYY)｜English Name｜Chinese Name｜Passport Number｜Passport Expiry｜Sex｜DOB｜Nationality｜Country/Place of Birth｜City/State or Province｜Place of Residence｜Visa Type｜Visa Number｜Country/Region Code｜Mobile Number｜Occupation｜JobTitle｜Email｜Entry Mode｜Entry Flight Code｜Entry Flight Number｜Entry Vessel｜Intended Exit Date｜Exit Mode｜Exit Flight Code｜Exit Flight Number｜Exit Vessel｜Purpose of Visit｜Relatives Name｜Relatives Mobile｜Reason｜Accommodation in Taiwan｜Residential Address or Hotel Name。

## 🛠 技術備註 / 踩坑點
- **日期格式 DD/MM/YYYY**（Excel 匯入）：9/10 入境 = `10/09/2026`；9/13 離境 = `13/09/2026`。別填成美式 MM/DD。
- **English Name** 欄只接受英文字母 + 空格，勿加符號；若改填中文只收**繁體**。
- **護照效期** 建議 ≥ 入境後 6 個月（不足會跳提醒）。
- **Josh 的簽證號 = 入台證號**，須待入台證核准後才有 → Josh 的 TWAC 要等入台證下來（約 5 工作日內，早於 9/4，無衝突）。
- **官方 API**（供參，未自動化）：後端 `/acard-backend`；護照 OCR `/acard-api-proxy/image/passportInfo`；地址查詢 `/acard-api-proxy/tgos/queryAddr`。
- **相關檔案（见 `private/`）：** `private/TWAC_UserManual_Eng.pdf`（官方 48 頁圖解）、`private/TWAC_ACARD_EXCEL_template.xlsx`（空白批量模板+代碼表）、`private/TWAC_ACARD_prefilled_DRAFT.xlsx`（已預填草稿，待補缺項）。

## ✅ 進度
- [x] 走通全流程、摸清全部欄位與下拉代碼（2026-07-26，離線完成，未消耗 email 驗證碼）
- [x] 官方手冊 + Excel 模板已下載存檔
- [x] Excel 草稿已預填（共用欄 + Josh 整行；`private/TWAC_ACARD_prefilled_DRAFT.xlsx`）
- [x] 收齊 4 位美籍護照資料 + 5 人職業 + 電話
- [x] 台北飯店：Hotel Resonance Taipei, Tapestry Collection by Hilton；No. 7, Linsen S. Rd., Zhongzheng District, Taipei（林森南路7号，中正区）
- [x] 9/13 離境航班：MU5098（台北松山→上海虹橋）
- [x] **9/4 起** 正式線上提交 5 張 TWAC —— **已於 2026-09-04 (UTC+8 09-05 06:53) 全部提交成功！**
  電子入台證已寄至 jqx2014@gmail.com，5 人護照號碼與提交時間：
  - EJ5146622 (Josh) 06:53:34
  - A62715237 (Mom) 06:53:35
  - 644716683 (Ben) 06:53:37
  - A18417751 (Mani) 06:53:39
  - 584980216 (Fred) 06:53:41
  無需列印，入境時準備好電子憑證備查即可（TWAC 非簽證，仍須符合入境條件）。
