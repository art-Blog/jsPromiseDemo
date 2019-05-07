# js 非同步事件練習

## 01 Promise

使用 `new Promise(resolve,reject)` 去模擬三個事件的執行順序，可以想見若未來還有其他事件要增加，最終的寫法肯定會越來越像氣功波，且難以維護

```js
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

使用語法糖來改寫，程式碼階層數量不再受到事件數量影響，便於閱讀也容易撰寫

```js
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
