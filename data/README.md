# 資料集說明 / Dataset Description

本專案使用的原始資料來自 **Garmin Health API**，共三個 CSV 檔案。
由於資料涉及個人健康隱私，**原始資料不包含於此 repo**。

若您希望使用自己的 Garmin 資料重現本分析，請依照下方格式說明準備資料，
並將三個檔案放置於 `./巨量資料概論/` 資料夾中。

---

## 資料夾結構

```
巨量資料概論/
├── dailies.csv              # 每日活動摘要（6,047 筆）
├── sleeps.csv               # 睡眠紀錄（3,976 筆）
└── dailies_heartrate.csv    # 詳細心率時間序列（約 2,000 萬筆）
```

---

## 1. `dailies.csv` — 每日活動摘要

每筆資料代表某位使用者某一天的活動摘要，共 **6,047 筆**。

| 欄位名稱 | 型別 | 說明 |
|---|---|---|
| `_userId` | str | 使用者匿名識別碼 |
| `_upload_datetime` | str | 資料上傳時間 |
| `_activity_status` | int | 活動狀態碼 |
| `summaryId` | str | 唯一摘要識別碼（可與心率資料 join） |
| `date` | str | 活動日期（YYYY-MM-DD） |
| `activityType` | str | 活動類型（如 WALKING） |
| `activeKilocalories` | float | 主動消耗熱量（大卡） |
| `bmrKilocalories` | float | 基礎代謝率熱量（大卡） |
| `steps` | float | 步數 |
| `distanceInMeters` | float | 移動距離（公尺） |
| `durationInSeconds` | float | 監測總時長（秒）|
| `activeTimeInSeconds` | float | 活動時間（秒） |
| `startTimeInSeconds` | float | 當日開始的 Unix 時間戳記（秒） |
| `startTimeOffsetInSeconds` | float | 時區偏移量（秒） |
| `moderateIntensityDurationInSecon` | float | 中等強度運動時長（秒） |
| `vigorousIntensityDurationInSecon` | float | 高強度運動時長（秒） |
| `floorsClimbed` | float | 爬樓層數 |
| `minHeartRateInBeatsPerMinute` | float | 最低心率（bpm） |
| `maxHeartRateInBeatsPerMinute` | float | 最高心率（bpm） |
| `averageHeartRateInBeatsPerMinute` | float | 平均心率（bpm） |
| `restingHeartRateInBeatsPerMinute` | float | **靜止心率**（bpm）⭐ 本研究關鍵欄位 |
| `stepsGoal` | float | 步數目標 |
| `intensityDurationGoalInSeconds` | float | 強度時長目標（秒） |
| `floorsClimbedGoal` | float | 爬樓目標 |
| `averageStressLevel` | float | 平均壓力指數 |
| `maxStressLevel` | float | 最高壓力指數 |
| `stressDurationInSeconds` | float | 壓力持續時間（秒） |
| `restStressDurationInSeconds` | float | 休息壓力時間（秒） |
| `activityStressDurationInSeconds` | float | 活動壓力時間（秒） |
| `lowStressDurationInSeconds` | float | 低壓力時間（秒） |
| `mediumStressDurationInSeconds` | float | 中等壓力時間（秒） |
| `highStressDurationInSeconds` | float | 高壓力時間（秒） |
| `stressQualifier` | str | 壓力等級分類 |

**範例資料（前 2 筆，部分欄位）：**

| _userId | date | distanceInMeters | restingHeartRateInBeatsPerMinute | startTimeInSeconds |
|---|---|---|---|---|
| x31a515b | 2019-12-19 | 2406.0 | 62.0 | 1576684800 |
| x31a515b | 2019-12-20 | 0.0 | 62.0 | 1576771200 |

---

## 2. `sleeps.csv` — 睡眠紀錄

每筆資料代表某位使用者某一晚的睡眠事件，共 **3,976 筆**（一晚可能有多筆）。

| 欄位名稱 | 型別 | 說明 |
|---|---|---|
| `_userId` | str | 使用者匿名識別碼 |
| `_datetime` | str | 睡眠事件偵測時間 |
| `_durationInSeconds` | int | 睡眠總時長（秒） |
| `summaryId` | str | 唯一識別碼 |
| `date` | str | 睡眠日期（YYYY-MM-DD） |
| `durationInSeconds` | float | 睡眠持續時間（秒） |
| `startTimeInSeconds` | float | **睡眠開始 Unix 時間戳記**（秒）⭐ 本研究關鍵欄位 |
| `startTimeOffsetInSeconds` | float | 時區偏移量（秒） |
| `unmeasurableSleepInSeconds` | float | 無法辨識的睡眠時間（秒） |
| `deepSleepDurationInSeconds` | float | **深度睡眠時間**（秒）⭐ 本研究依變數 |
| `lightSleepDurationInSeconds` | float | 淺層睡眠時間（秒） |
| `remSleepInSeconds` | float | REM 睡眠時間（秒） |
| `awakeDurationInSeconds` | float | 清醒時間（秒） |
| `validation` | str | 資料驗證狀態（如 AUTO_FINAL） |

**範例資料（前 2 筆，部分欄位）：**

| _userId | date | durationInSeconds | deepSleepDurationInSeconds | startTimeInSeconds |
|---|---|---|---|---|
| x3356405 | 2020-04-06 | 31080.0 | 8460.0 | 1586183456 |
| x3356405 | 2020-04-06 | 30960.0 | 1380.0 | 1586183672 |

---

## 3. `dailies_heartrate.csv` — 詳細心率時間序列

每筆資料為某位使用者在某一天某個時間點的心率量測值，共約 **2,000 萬筆**。

| 欄位名稱 | 型別 | 說明 |
|---|---|---|
| `_userId` | str | 使用者匿名識別碼 |
| `_datetime` | str | 日期時間 |
| `_upload_date_seconds` | int | 上傳日期（秒） |
| `_activity_status` | str | 活動狀態 |
| `summaryId` | str | 唯一識別碼（可與 dailies.csv join） |
| `date` | str | 日期 |
| `timeseconds` | int | 相對於當日開始的秒數偏移 |
| `HeartRate` | int | **心率**（bpm）⭐ 本研究關鍵欄位 |
| `startTimeInSeconds` | int | 當日開始的 Unix 時間戳記（秒） |

**範例資料（前 3 筆）：**

| _userId | date | timeseconds | HeartRate | startTimeInSeconds |
|---|---|---|---|---|
| x31a515b | 2019-12-19 | 425 | 69 | 1576684800 |
| x31a515b | 2019-12-19 | 440 | 69 | 1576684800 |
| x31a515b | 2019-12-19 | 455 | 69 | 1576684800 |

> 💡 **注意**：`timeseconds` 是相對秒數偏移，實際時間 = `startTimeInSeconds + timeseconds`

---

## 資料來源與隱私說明

- 原始資料由 **Garmin Health API** 提供，屬於真實世界數據（Real-World Data）
- 所有 `_userId` 均已匿名化處理
- 資料時間範圍：2019 年底至 2020 年代初
- **資料不對外公開**，請勿聯繫作者索取原始資料

---

## 如何使用自己的 Garmin 資料重現分析

1. 申請 [Garmin Health API](https://developer.garmin.com/health-api/) 開發者帳號
2. 依照上方欄位格式整理您的資料為對應的 CSV 格式
3. 將三個檔案放置於 `./巨量資料概論/` 資料夾
4. 執行 `pip install -r requirements.txt`
5. 開啟並執行 `巨量期末專案_第三組.ipynb`

> ⚠️ `dailies_heartrate.csv` 資料量龐大（約 2,000 萬筆），建議至少預留 **8GB RAM**。
