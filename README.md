# Joan Fitness OS Web App

這是一個 Google Apps Script 一頁式打卡網頁，手機打開即可輸入飲食、蛋白質、喝水、運動、心情與身體數據，資料會寫入 Google Sheet。

## 建立方式

### 方式 A：直接用 Google Apps Script 網頁

1. 建立一個新的 Google Sheet。
2. 到 `擴充功能` -> `Apps Script`。
3. 將 `Code.gs` 的內容貼到 Apps Script 的 `Code.gs`。
4. 新增一個 HTML 檔案，命名為 `Index`，貼上 `index.html` 的內容。
5. 儲存後執行一次 `ensureSheets_`，授權後會自動建立四個工作表：
   - `Daily Log`
   - `Meal Log`
   - `Body Tracker`
   - `Meal Template`
6. 點右上角 `部署` -> `新增部署作業`。
7. 類型選 `網頁應用程式`。
8. 執行身分選 `我`。
9. 存取權依需求選：
   - 只有自己和朋友：選受限制對象，並分享權限
   - 任何知道網址的人都可打卡：選知道連結的人
10. 部署後，把 Web App URL 加到手機主畫面。

### 方式 B：放到 GitHub Pages，資料寫入 Google Sheet

GitHub Pages 不能使用 `google.script.run`，所以要用 Apps Script 當後端 API。

1. 先照方式 A 建立並部署 Apps Script Web App。
2. 複製部署後的 Web App URL，格式大概像：

```text
https://script.google.com/macros/s/部署ID/exec
```

3. 打開 `index.html`，把最上方的 `WEB_APP_URL` 改成你的 Web App URL：

```js
const WEB_APP_URL = 'https://script.google.com/macros/s/部署ID/exec';
```

4. 把 `index.html` 上傳到 GitHub repo 根目錄，或上傳到 GitHub Pages 設定的 `/docs` 目錄。
5. 到 GitHub repo 的 `Settings` -> `Pages`，選擇部署分支。
6. 開啟 GitHub Pages 網址後即可使用。

注意：GitHub Pages 版本會用 `no-cors` 送資料到 Apps Script，所以前端無法讀取 Apps Script 回傳內容，只會顯示「已送出」。實際資料是否成功請看 Google Sheet。

## 每天怎麼用

1. 選日期與人員。
2. 勾早餐、午餐、晚餐。
3. 在餐點明細直接打字，例如：

```text
2顆蛋 + 豆漿 + 優格
```

也可以用：

```text
茶葉蛋*2、無糖豆漿、白飯半碗
```

4. 系統會即時計算蛋白質與完成率。
5. 填喝水、步數、運動、心情。
6. 按 `送出到 Google Sheet`。

## 食物模板在哪裡改

食物模板在 Google Sheet 的 `Meal Template` 分頁管理，不需要改程式。

欄位如下：

- `食物名稱`
- `分類`
- `估計熱量`
- `蛋白質`
- `別名`
- `備註`

新增食物時直接在 `Meal Template` 加一列，例如：

```text
雞胸肉 | 蛋白質 | 165 | 31 | 雞肉、雞胸 | 
```

`別名` 很重要。網頁會用食物名稱和別名比對你輸入的文字，例如你輸入 `2顆蛋 + 豆漿 + 優格`，會用 `蛋`、`豆漿`、`優格` 去匹配模板並計算蛋白質。

如果某個餐點沒有匹配到模板，也會照樣寫進 `Meal Log`，只是蛋白質會以 `0` 計算，方便之後回頭補模板。

## 運動姿勢圖片在哪裡放

運動動作資料在 `Code.gs` 的 `getExerciseLibrary_()` 裡。每個動作都有一個 `imageUrl` 欄位。

請把姿勢照片放到可公開讀取的位置，再把圖片網址貼進去：

```js
imageUrl: 'https://你的圖片網址',
```

建議圖片來源：

- 自己拍照後上傳到 Google Drive，分享設定改成知道連結的人可查看
- 放到網站或圖床，使用可以直接開啟圖片的網址
- 若要用 Google Drive 圖片，可以直接貼分享連結；網頁會自動轉成可顯示的縮圖網址

Google Drive 分享連結格式：

```text
https://drive.google.com/file/d/圖片檔案ID/view?usp=sharing
```

圖片檔案 ID 是分享連結 `/d/` 和 `/view` 中間那一段。

網頁實際顯示時會自動轉成：

```text
https://drive.google.com/thumbnail?id=圖片檔案ID&sz=w1200
```

## 備註

如果這個 Web App 是綁在 Google Sheet 裡建立的，`CONFIG.SPREADSHEET_ID` 可以保持空字串。若 Apps Script 是獨立專案，請填入 Google Sheet 網址中的 spreadsheet ID。
