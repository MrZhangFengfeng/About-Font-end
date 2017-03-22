Promise 对象用于异步计算。一个 Promise 表示一个现在、将来或永不可能可用的值

## 语法

>    
    new Promise(
      /* executor */ 
      function(resolve, reject){...}
    );
    
#### 参数
executor
   一个与参数resolve和 reject一起传递的函数 。执行器函数由Promise实现立即执行，传递
resolve和reject函数. (在Promise构造函数之前调用执行器甚至返回创建的对象) 
   在 executor 内部，如果 resolve 被调用，代表该Promise被成功解析（resolve），而当reject
   被调用时，代表该Promise的值不能用于后续处理了，也就是被拒绝（reject）了。executor
主要用于初始化异步代码， 一旦异步代码调用完成，要么调用 resolve 方法来表示Promise被成功
解析，或是调用 reject 方法，表示初始化的异步代码调用失败，整个promise被拒绝。
  如果在executor 方法的执行过程中抛出了任何异常，那么promise立即被拒绝（即相当于reject
  方法被调用），executor 的返回值也就会被忽略。
  
## 描述
  **Promise** 对象是一个代理对象（代理一个值），被代理的值在Promise对象创建时可能是未知
的。它允许你为异步代码执行结果的成功和失败分别绑定相应的处理方法（handlers ）。 这让
异步方法可以像同步方法那样返回值，但是并非立即返回执行的结果，因为毕竟执行的是异步代
码，因此，它会返回一个Promise对象，如前所说，它是一个代理的对象，代理了最终返回的
值，可以在后期使用

Promise有以下几种状态
  * *pending*:初始状态，初始状态，未完成或拒绝\n
  * *fulfilled*: 意味操作成功\n
  * *rejected*: 意味操作失败
  
pending状态的Promise对象可能被填充了（fulfilled）值，也可能被某种理由（异常信息）拒
绝（reject）了。当其中任一种情况出现时，Promise对象的then方法绑定的处理方法（handlers）
就会被调用 (then方法包含两个参数：onfulfilled和onrejected，它们都是Function类型。当值被
填充时，调用then的onfulfilled方法，当Promise被拒绝时，调用then的onrejected方法， 所以在
异步操作的完成和绑定处理方法之间不存在竞争)

因为[Promise.prototype.then](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
和 [Promise.prototype.catch](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)
方法返回 promises对象自身， 所以它们可以被链式调用。


！[](/img/promise.jpg)
>    
    注意： 有一些语言中有惰性求值和延时计算的特性, 它们也被称为"promises" --例如Scheme. Javascript中的promise代表
    一种已经发生的状    态, 而且可以通过回调方法链    在一起. 如果你想要的是表达式的延时计算, 考虑无参数的"箭头方法":
    f = () => 表达式 创建惰性求值的表达式, 使用 f() 求值.
    
    注意： 如果一个promise对象处在fulfilled或rejected状态而不是pending状态，那么它也    可以被称为settled状态。你
    可能也会听到一个术语resolved ，它表示promise对象处于    settled状态，或者promise对象被锁定在了调用链中。关于
    promise的状态， Domenic Denicola    的 States and fates 有更多详情可供参考。

- - -

## 属性
**Promise.length**

    长度属性，其值为 1 (构造器参数的数目).
   
[Promise.prototype](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/prototype)

表示 Promise 构造器的原型.   

- - -

## 方法

[Promise.all(iterable)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

  这个方法返回一个新的promise对象，该promise对象在iterable里所有的promise对象都成功的时候才会触发
  成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。这个新的promise
  对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟
  iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的
  promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。
  （可以参考jQuery.when方法---译者注）
  

[Promise.race(iterable)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

  当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败
  详情作为参数调用父promise绑定的相应句柄，并返回该promise对象。
  
[Promise.reject(reason)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)

  调用Promise的rejected句柄，并返回这个Promise对象。
  
[Promise.resolve(value)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

  用成功值value完成一个Promise对象。如果该value为可继续的（thenable，即带有then方法），返回的Promise对
  象会“跟随”这个value，采用这个value的最终状态；否则的话返回值会用这个value满足（fullfil）返回的Promise对象。
  
  
## Promise原型

#### 属性

Promise.prototype.constructor

返回创建了实例原型的函数.  默认为 Promise 函数.

#### 方法

[Promise.prototype.catch(onRejected)]()

  添加一个否定(rejection) 回调到当前 promise, 返回一个新的promise。如果这个回调被调用，新 promise 将以它
  的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的肯定结果作为新promise的
  肯定结果.
  
[Promise.prototype.then(onFulfilled, onRejected)]()

  添加肯定和否定回调到当前 promise, 返回一个新的 promise, 将以回调的返回值 来resolve。
  
  
## 示例

#### 非常简单的例子(就10行！)

>    
    var myFirstPromise = new Promise(function(resolve, reject){
        //当异步代码执行成功时，我们才会调用resolve(...), 当异步代码失败时就会调用reject(...)
        //在本例中，我们使用setTimeout(...)来模拟异步代码，实际编码时可能是XHR请求或是HTML5的一些API方法.
        setTimeout(function(){
            resolve("成功!"); //代码正常执行！
        }, 250);
    });

    myFirstPromise.then(function(successMessage){
        //successMessage的值是上面调用resolve(...)方法传入的值.
        //successMessage参数不一定非要是字符串类型，这里只是举个例子
        console.log("Yay! " + successMessage);
    });
    












