# 第 8 週作業｜Vue 元件化課程頁

這是一個可執行的 Vue 3 + Vite 課程列表示範。頁面以課程卡片與操作按鈕做為重複區塊，展示 props、元件事件、v-for、穩定 key，以及父層管理的互動狀態。所有課程與價格都是課堂用模擬資料，不會連接真實報名或付款服務。

## 執行方式

1. 安裝 Node.js 18 或更新版本。
2. 在此資料夾執行 `npm install`。
3. 執行 `npm run dev`，並開啟終端機顯示的本機網址。
4. 執行 `npm run build` 可輸出正式版本至 `dist/`。

## 操作與驗收

- 初始列表有三筆課程資料；卡片透過 `v-for` 和 `:key="course.id"` 顯示。
- 搜尋可比對課名、介紹、分類與標籤；沒有符合資料時會顯示空狀態及清除搜尋操作。
- 排序可切換推薦順序、價格低至高與高至低。
- 愛心按鈕可切換收藏狀態。
- 新增課程會更新父層的 `courses`，列表即時增加一張卡片。
- 「資料視覺化基礎」為額滿狀態，操作按鈕停用。
- 選擇開放課程時只顯示課堂提示，不會建立報名或付款。

## 元件拆分

- `src/App.vue`：父層 mock data、搜尋、排序、收藏、新增資料與提示狀態。
- `src/components/CourseCard.vue`：透過 `course`、`index`、`favorite` props 顯示可重用課程卡，emit `toggle-favorite` 與 `enroll`。
- `src/components/ActionButton.vue`：透過 `label`、`variant`、`disabled`、`type` props 顯示一致操作按鈕。
- `src/style.css`：頁面版型、元件樣式與行動版排版。

## 提交到 GitHub

將整個 `vue-course-components` 資料夾新增到你的專題 repository，再推送到 GitHub。完成後把 repository 網址補到課程的「AI 協作歷程」表單。這份工作區目前沒有指定的 GitHub repository，因此沒有代你推送或填寫提交表單。
