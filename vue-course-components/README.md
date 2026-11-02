# Vue 元件化專題｜拾物・校園二手市集

「拾物」是課堂展示用的校園二手好物市集，讓學生瀏覽書籍、3C 周邊與生活用品。專案使用 Vue 3、Vite、JavaScript 與 `<script setup>`，不連接真實 API；刊登、聯絡與交易皆為模擬功能。

## 執行方式

1. 安裝 Node.js 18 或更新版本。
2. 在此資料夾執行 `npm install`。
3. 執行 `npm run dev`，開啟終端機顯示的本機網址。
4. 執行 `npm run build` 可輸出正式版本至 `dist/`。

## 功能與驗收

- 三筆 mock data 均含 `id`、`title`、`desc`、`price`、`tags`、`available`，以 `v-for` 和穩定的 `:key="product.id"` 渲染。
- 搜尋可比對物品名稱、介紹、分類及標籤；分類篩選可切換全部、書籍、3C、生活。
- 排序支援最新上架、價格低至高與高至低。
- 收藏按鈕可切換收藏狀態；刪除刊登會從父層資料移除該商品並同步清除收藏。
- 新增模擬刊登會更新父層 `products`，並即時顯示新卡片。
- 復古桌燈為已售出狀態，按鈕停用。
- 搜尋無結果時顯示空狀態與清除篩選操作。
- 所有按鈕都不會進行實際聯絡、付款或交易。

## 元件拆分

- `src/App.vue`：父層管理模擬商品資料、搜尋、分類、排序、收藏、新增、刪除與提示狀態。
- `src/components/ProductCard.vue`：接收 `product`、`index`、`favorite` props，以商品資料渲染重用卡片，並 emit `toggle-favorite`、`view-product`、`delete-product` 事件。
- `src/components/ActionButton.vue`：接收 `label`、`variant`、`disabled`、`type` props，統一按鈕樣式與停用狀態。
- `src/style.css`：市集版面、商品插圖、卡片及行動版樣式。
