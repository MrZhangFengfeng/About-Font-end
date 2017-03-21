#  发布订阅模式（观察者模式）

* 可以简单的理解为普通市民订阅报纸（有多个需求不同报纸的人）。
* 报社分发报纸（有多家不同类型的报社）。

## 下面模拟一下场景

![](/img/1.png)

>  

    方法就是人员，报社列一张订阅表，对应的报社在分发的时候执行对应列表的方法即可
    人员1订阅了报纸1，报纸2
    people1.subscribe("publisher1").subscribe("publisher2");
    人员2订阅了报纸3，报纸4
    people2.subscribe("publisher3").asubscribed("publisher4");
    报社1 分发报纸1,2（触发事件）
    publisher1.deliver(data);
    报社2 分发报纸3,4(触发事件)
    publisher2.deliver(data);


## 模拟一个简单的订阅模式

>	
    /*
		Publisher 代表出版社构造器
		subscribers 代表投递报纸的列表
		deliver  分发
		subscribe 订阅报纸的接口
		publisher1  出版社实例对象
		observer1 实例化的读者
    */
    
    // 出版社（权力制高点）
    
	function Publisher() {

		// 订阅了本出版社的读者列表
		this.subscribers = [];
	}

	// 添加一个迭代方法，遍历所有投递列表 
	Publisher.prototype.deliver = function(data) {
		// 遍历当前出版社对象所有的订阅过的方法
		this.subscribers.forEach(function(fn) {
			//data用于传参数给内部方法
			fn.fire(data);
		});
		return this;
	}


	// 观察者
	function Observe(callback) {
		this.fire = callback;
	}

	// 给予订阅者阅读的能力
	Observe.prototype.subscribe = function(publisher) {

		var that = this;
		// some如果有一个返回值为true则为true
		var alreadyExists = publisher.subscribers.some(function(el){

			// 这里的el指的是函数对象
			return el.fire === that.fire;
		});

		// 如果存在这个函数对象则不进行添加
		if (!alreadyExists) {
			publisher.subscribers.push(this);
		}

		//订阅列表
		console.log(publisher.subscribers);
		return this;
	};

	// 给予观察者退订的能力
	Observe.prototype.unsubscribe = function(publisher) {

		var that = this;

		// 过滤，将返回值为true的重组成数组
		publisher.subscribers = publisher.subscribers.filter(function(el) {

			// 这里的el.fire指的是观察者传入的callback
			// that.fire就是当前对象对callback的引用
			return !(el.fire === that.fire);
		})
		console.log(publisher.subscribers)
		return this;
	};

	var publisher1 = new Publisher();

	// 实例化的读者，这个需要改进
	var observer1 = new Observe(function() {
			console.log(111)
		});

	// 读者订阅了一家报纸,在报社可以查询到该读者已经在订阅者列表了，
	// publisher1.subscribers->[observer1]
	observer1.subscribe(publisher1);
                                                                              
	// 分发报纸，执行所有内在方法
	publisher1.deliver(123);// 输出123

	// 退订
	observer1.unsubscribe(publisher1);
  
  - - -
  ## 订阅发布模式的实践
  
  ### 源码地址
  [源码链接](https://github.com/Kelichao/kit.js)
  
  ### 生成函数列表
  [Demo链接](https://github.com/Kelichao/kit.js/issues/3)
  
  > kit.Control中的回调列队方式
  
     callDemo.subscribe({
     // 普通函数回调列队，参数取argue数组
	   // 默认会先调用callback的函数队列
	   // 如果想优先调用queue中的队列可以单独写一个的queue函数
	   callback: ["bbb","ccc"],
	   // 特殊队列函数，参数自定义
	   queue: {
		   "aaa": [666]
	    }
     });
     
     
 ### Promise包装器
 [Demo链接](https://github.com/Kelichao/kit.js/issues/5)
