> 设计原则是理论基础，设计模式是设计原则的具体实现。

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

简单理解：指定某个对象去做特定的事情；在JavaScript场景中，特定的事情可以通过函数来表达。
应用场景：命令发送者与执行命令的对象松耦合；在JavaScript场景中常用的方式是回调函数。

## 发布订阅模式


