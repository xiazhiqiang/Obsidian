设计原则：单一职责、开放-封闭等。
> 设计原则是理论基础，设计模式是设计原则的具体实现

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




