# Fukuokatrip
## 記帳多人同步(Firebase)

`expense.html` 預設只存在各自手機。要多人同步,需要一個免費的 Firebase 專案:

1. 到 https://console.firebase.google.com 建立專案(不用開 Google Analytics)。
2. 左側「建構」→ **Firestore Database** → 建立資料庫(選離日本近的區域,例如 asia-northeast1)。
3. 「規則」分頁,貼上下面的規則並發布:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 只開放 trips/{旅程代碼}/ 底下的資料;不能列出所有旅程,所以要知道代碼才讀得到
    match /trips/{trip}/{coll}/{id} {
      allow read: if true;
      allow create, update: if request.resource.data.keys().size() < 12;
      allow delete: if true;
    }
  }
}
```

4. 專案設定 → 一般 → 「您的應用程式」→ 新增網頁應用程式(`</>`),複製 `firebaseConfig`。
5. 把它貼進 `firebase-config.js`,取代 `null`,commit 並 push。
6. 開啟記帳頁,按「複製邀請連結」傳給同行的人。連結裡的 `#t=代碼` 就是這趟旅程的房間。

注意:知道連結的人都能讀寫這趟旅程的帳,請只傳給同行的人。`apiKey` 放在前端是 Firebase 的正常設計,安全靠上面的規則。
