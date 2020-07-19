# 面向对象
类、继承、多态
### 一、js 中的类
1. 构造函数定义法
构造函数是es6之前最常用的定义类方法
```javascript
  function Person(){
		this.name = 'michaelqin';
	}
	Person.prototype.sayName = function(){
		alert(this.name);
	}

	var person = new Person();
	person.sayName();
```
2. 对象创建方法定义类
`Object.create(obj)`可以创建一个新的对象，且跟原对象不共享地址。关于`Object.create()`和`new Object()`的区别：[Object.create()和深拷贝](https://www.cnblogs.com/zhilin/p/11402064.html)
```javascript
  var Person = {
		name: 'michaelqin',
		sayName: function(){ alert(this.name); }
	};

	var person = Object.create(Person);
	person.sayName();
```
3. es6 中的类
es6 中新增了关于类和模块的定义。
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayName() {
    console.log(this.name);
  }
}
let p = new Person('kathy');
p.sayName()
```
### 2. 继承
- 原型链继承
- 构造函数继承
- 寄生组合继承
- 属性复制继承：枚举父类属性，创建子类复制属性。
其中属性复制不常见，它的形式是这样的
```javascript
  function Animal() {
		this.name = 'animal';
	}
	Animal.prototype.sayName = function() {
		alert(this.name);
	};

	function Person() {}

	for(prop in Animal.prototype) {
		Person.prototype[prop] = Animal.prototype[prop];
	} // 复制动物的所有属性到人量边
	Person.prototype.constructor = 'Person'; // 更新构造函数为人
```
### 3. 多态
多态即多重继承的意思，可以通过类继承方法中的 属性复制法 实现。

