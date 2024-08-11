> 设计原则是理论基础，设计模式是设计原则的具体实现。
> **设计模式总是把不变的事物和变化的事物分离开来。**

常用设计原则：单一职责、开放-封闭等。

## 单例模式

单例模式的核心是确保在全局中只有一个实例，例如全局缓存，window对象，也可以定义一些常量来实现，但是都是在未使用时创建。
如何做到在真正使用时才创建对象，且后续返回的都是这一个相同对象呢？——==惰性单例==

```javascript
const getSingle = function(fn) {
	let ret;
	return function() {
		return typeof ret !== 'undefined' ? ret : fn.apply(this, arguments);
	};
};

const getSingle2 = function(Cl) {
	let ret;
	return function() {
		return typeof ret !== 'undefined' ? ret : new Cl(...arguments);
	};
};
```

## 策略模式

策略模式就是将使用和实现分离。

```javascript
// 不同策略
const strategies = {
	A: function(score) {
		return score * 2;
	},
	B: function(score) {
		return score * 3;
	},
};

// 使用策略
const strategyRun = function(level, score) {
	return strategies[level](score);
};

// 运行
strategyRun('A', 10);
```

## 迭代器模式


## 命令行模式

应用场景：有时候需要向某些对象发送请求，但是并不知道请求的接收者是谁，也不知道被请求的操作是什么，此时希望用一种松耦合的方式来设计软件，使得请求发送者和请求接收者能够消除彼此之间的耦合关系。
命令模式都会在 command 对象中保存一个接收者来负责真正执行客户的请求。

```javascript
// 目标对象
const Menu = {
	refresh: function() {
		console.log('刷新菜单');
	}
};
// 目标对象
const Sub = {
	run: function() {
		console.log('run');
	}
};

// 命令模式，解耦发起者
const RefreshCommand = function(receiver) {
	return {
		execute: function() {
			receiver.refresh();
		}
	};
};
const RunCommand = function(receiver) {
	return {
		execute: function() {
			receiver.run();
		}
	};
};

// 发起者执行命令，解耦执行者
const execute = function(sendObj, command) {
	sendObj.onclick = function() {
		command.execute();
	};
};

execute(document.getElementById('btn'), RefreshCommand(Menu));
execute(document.getElementById('btn'), RunCommand(Sub));
```

## 发布订阅模式

发布—订阅模式又叫观察者模式，它定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。
在JavaScript中通常用事件模型及回调函数代替发布订阅模式。

```javascript
class Evt {
	constructor() {
		this.subscriberList = {};
	}
	listen(key, fn) {
		if (!this.subscriberList[key]) {
			this.subscriberList[key] = [];
		}
		this.subscriberList[key].push(fn);
	}
	trigger(key, ...p) {
		for (let fns = this.subscriberList[key], len = fns.length; i < len; i++) {
			if (fns[i]) {
				fns[i].apply(this, p);
			}
		}
	}
	remove(key, fn) {
		this.subscriberList[key] = (this.subscriberList[key] || []).filter(item => {
			return item !== fn;
		});
	}
}

const event = new Evt();

// 订阅事件
event.listen('click', function(a, b) {
	console.log('add', a + b);
});
// 订阅事件
event.listen('click', function(a, b) {
	console.log('div', a / b);
});

// 触发事件
event.trigger('click', 1, 2);
```

缺点：需要先订阅后触发，否则会丢失。在某些情况下，我们需要先将这条消息保存下来，等到有对象来订阅它的时候，再重新把消 息发布给订阅者。
