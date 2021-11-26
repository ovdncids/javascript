# Promise

## Promise란
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise

**언제 완료 될지 모르는 여러 쓰레드를 기다려다가 모두 완료 되면 처리하는 내장 함수**

## 기본 문법
```js
const promise1 = new Promise(function(resolve, reject) {
  // resolve('Resolved promise1');
  reject('Rejected promise1');
})
const promise2 = new Promise(function(resolve, reject) {
  // resolve('Resolved promise2');
  reject('Rejected promise2');
})
Promise.all([promise1, promise2]).then(function(result) {
  console.log(result);
}).catch(function(error) {
  console.error(error);
})
```

### Case1
하나의 프로미쓰라도 reject 메소드가 실행 되었다면 다른 프로미쓰를 기다리지 않고 바로 catch 메소드 실행됨

### Case2
모든 프로미쓰가 resolve 메소드를 실행 할 경우 then 메소드가 실행 됨

## Axios 대입
```js
const promises = [];
promises[0] = new Promise(function(resolve, reject) {
  axios.get('http://localhost:3100/api/v1/members').then(function(response) {
    resolve(response.data);
  }).catch(function(error) {
    reject(error);
  })
})
promises[1] = new Promise(function(resolve, reject) {
  axios.get('http://localhost:3100/api/v1/members').then(function(response) {
    resolve(response.data);
  }).catch(function(error) {
    reject(error);
  })
})
Promise.all(promises).then(function(result) {
  console.log(result);
}).catch(function(error) {
  console.error(error);
})
```

## Sleep 함수 만들기
* https://stackoverflow.com/questions/33289726/combination-of-async-function-await-settimeout
```js
const sleep = function(ms) {
  return new Promise(function(resolve) {setTimeout(resolve, ms)});
};
console.log('sleeping');
sleep(3000).then(function() {
  console.log('woke up');
});
```

## Promise Polyfill for IE11
```html
<script src="https://unpkg.com/core-js-bundle@3.6.0/minified.js"></script>
```
```sh
React: 적용 해야함 # 안되면 build 후에 확인
Vue.js: 불필요
Angular: TypeScript로 바뀌면서 IE11 지원 안 함
```
* ❕ `async await`는 `babel`이 자동으로 `Promise 패턴`으로 변경해준다. 따라서 `babel` 환경에서는 신경 쓰지 않고 사용해도 된다.


## Custom Promise
```js
class CustomPromise {
  state;
  returnValue;
  parentPromise;

  resolve = (returnValue) => {
    this.state = 'resolved';
    this.returnValue = returnValue;
    this.parentPromise && this.parentPromise.then(this.parentPromise._then);
  };

  reject = (returnValue) => {
    this.state = 'rejected';
    this.returnValue = returnValue;
    this.parentPromise && this.parentPromise.catch(this.parentPromise._catch);
  };

  constructor(callback) {
    callback && callback(this.resolve, this.reject);
  }

  static all = function(promises) {
    const parentPromise = {
      promises: promises,
      then: function(callback) {
        this._then = callback;
        const returnValues = [];
        for (let index in this.promises) {
          returnValues.push(this.promises[index].returnValue);
          if (this.promises[index].state !== 'resolved') {
            return this;
          }
        }
        this._then(returnValues);
        return this;
      },
      catch: function(callback) {
        this._catch = callback;
        for (let index in this.promises) {
          if (this.promises[index].state === 'rejected') {
            this._catch(this.promises[index].returnValue);
            break;
          }
        }
        return this;
      }
    };
    for (let index in promises) {
      promises[index].parentPromise = parentPromise;
    }
    return parentPromise;
  }
}
```
```js
const promises = [];
promises[0] = new CustomPromise(function(resolve, reject) {
  // resolve('Resolved promise1');
  reject('Rejected promise1');
});
promises[1] = new CustomPromise(function(resolve, reject) {
  // resolve('Resolved promise2');
  reject('Rejected promise2');
});
CustomPromise.all(promises).then(function(result) {
  console.log(result);
}).catch(function(error) {
  console.log(error);
});
```
