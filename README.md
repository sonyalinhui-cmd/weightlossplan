# Joan Fitness OS Web App

這是一個 Google Apps Script 一頁式打卡網頁，手機打開即可輸入飲食、蛋白質、喝水、運動、心情與身體數據，資料會寫入 Google Sheet。

## 建立方式

1. 建立一個新的 Google Sheet。
2. 到 `擴充功能` -> `Apps Script`。
3. 將 `Code.gs` 的內容貼到 Apps Script 的 `Code.gs`。
4. 新增一個 HTML 檔案，命名為 `Index`，貼上 `Index.html` 的內容。
5. 儲存後執行一次 `ensureSheets_`，授權後會自動建立三個工作表：
   - `Daily Log`
   - `Meal Log`
   - `Body Tracker`
6. 點右上角 `部署` -> `新增部署作業`。
7. 類型選 `網頁應用程式`。
8. 執行身分選 `我`。
9. 存取權依需求選：
   - 只有自己和朋友：選受限制對象，並分享權限
   - 任何知道網址的人都可打卡：選知道連結的人
10. 部署後，把 Web App URL 加到手機主畫面。

## 每天怎麼用

1. 選日期與人員。
2. 勾早餐、午餐、晚餐。
3. 在餐點明細新增吃的食物與份數。
4. 系統會即時計算蛋白質與完成率。
5. 填喝水、步數、運動、心情。
6. 按 `送出到 Google Sheet`。

## 食物模板在哪裡改

目前食物模板在 `Code.gs` 的 `getMealTemplates_()` 裡。

新增食物時照這個格式加一行：

```js
{ name: '雞胸肉', category: '蛋白質', calories: 165, protein: 31 },
```

## 運動姿勢圖片在哪裡放

運動動作資料在 `Code.gs` 的 `getExerciseLibrary_()` 裡。每個動作都有一個 `imageUrl` 欄位。

請把姿勢照片放到可公開讀取的位置，再把圖片網址貼進去：

```js
imageUrl: 'https://你的圖片網址',
```

建議圖片來源：

- 自己拍照後上傳到 Google Drive，分享設定改成知道連結的人可查看
- 放到網站或圖床，使用可以直接開啟圖片的網址
- 若要用 Google Drive 圖片，請把分享連結轉成直連格式

Google Drive 直連格式：

```text
https://drive.google.com/uc?export=view&id=圖片檔案ID
```

圖片檔案 ID 是分享連結 `/d/` 和 `/view` 中間那一段。

## 備註

如果這個 Web App 是綁在 Google Sheet 裡建立的，`CONFIG.SPREADSHEET_ID` 可以保持空字串。若 Apps Script 是獨立專案，請填入 Google Sheet 網址中的 spreadsheet ID。
