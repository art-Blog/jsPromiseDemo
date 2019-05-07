# js 非同步事件練習

## memo

1. [Vuejs](https://vuejs.org/)
1. [dayjs](https://github.com/iamkun/dayjs)

## 01 Promise

[Sample Page 01](promiseSample01.html)

使用 `new Promise(resolve,reject)` 去模擬三個事件的執行順序，可以想見若未來還有其他事件要增加，最終的寫法肯定會越來越像氣功波，且難以維護

```js
// 事件範例
work01(ms) {
  return new Promise((resolve, reject) => {
    console.log(`work01 start- ${dayjs(new Date()).format("YYYY/MM/DD hh:mm:ss")}`);
    setTimeout(ms => {
      let time = new Date();
      console.log(`work01 end- ${dayjs(new Date()).format("YYYY/MM/DD hh:mm:ss")}`);
      resolve({ title, time });
    }, ms);
  });
},

// 執行事件範例
this.work01(3000).then(info => {
  this.showInfo(info);
  this.work02(2000).then(info => {
    this.showInfo(info);
    this.work03(1000).then(info => {
      this.showInfo(info);
    });
  });
});

```

## 02 async / await

[Sample Page 02](promiseSample02.html)

使用語法糖來改寫，程式碼階層數量不再受到事件數量影響，便於閱讀也容易撰寫

```js
// 以 async / await 改寫執行事件
async doJob() {
  const r1 = await this.work01(3000);
  this.showInfo(r1);
  const r2 = await this.work02(2000);
  this.showInfo(r2);
  const r3 = await this.work03(1000);
  this.showInfo(r3);
}
// or 使用inline method 凸顯程式意圖
async doJob() {
  this.showInfo(await this.work01(3000));
  this.showInfo(await this.work02(2000));
  this.showInfo(await this.work03(1000));
}
```

## 03 Promise catch error

[Sample Page 03](promiseSample03.html)

若要加上錯誤處理，則是添加上`reject()`區段，並於執行事件的方法中加上`catch()`來捕捉例外

```js
ajaxPromise(params) {
  return new Promise((resovle, reject) => {
    $.ajax({
      type: params.type || "get",
      async: params.async || true,
      url: params.url,
      data: params.data || "",
      success: res => {
        resovle(res);
      },
      error: err => {
        reject(err);
      }
    });
  });
},
setData(data) {
  this.infos = data;
},
async doJob() {
  this.ajaxPromise({ url: "dataNotExist.json" })
    .then(data => {
      this.setData(data);
    })
    .catch(err => {
      console.log(err);
    });
}
```

## 04 async / await catch error

若改寫成 `async / await`，`Promise` 發生錯誤時會拋出`reject()`的異常，因此在執行事件中需要透過`try...catch...`來處理

```js
async doJob() {
  try {
    let data = await this.ajaxPromise({ url: "dataNotExist.json" });
    this.setData(data);
  } catch (error) {
    console.log(error);
  }
}
```
