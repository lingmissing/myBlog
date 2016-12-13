---
title: JavaScrip面向对象和原型继承 
date: 2016-05-28 12:27:35
tags: javascript
categories: 代码如诗，前端如画
---
> 佛曰：对象本无根，类型亦无形。本来无一物，何处惹尘埃！ 

### 理解对象 ###
- 一切事物皆对象；
- 对象具有封装和继承特性；

### 创建对象 ###
- 通过object创建

```javascript
var person = new object();
	person.name = “Nicholas”;
	person.age = 19;
	person.job = “Software Engineer”;
	person.sayName = function() {
		alert(this.name);
}
```
<!-- more -->
- 通过字面量创建

```javascript
var person = {
	name: "Nicholas",
	age: 19,
	job: "Software Engineer",
	sayName: function() {
		alert(this.name);
	}
}
```

<!--more-->

### 属性类型 ###
	
##### 数据属性 #####
- `Configurable` 能否删除、能否修改属性特性、能否把属性修改为访问器属性 默认为true
- `Enumerable` 能否通过for-in循环返回属性 默认为true
- `Writable` 能否修改属性的值 默认为true
- `Value` 包含属性的值。取值时从这个位置读，写入值时保存新值在这个位置。 默认值为 undefined
##### 访问器属性 #####
-  `Configurable`  同上
-  `Enumerable`  同上
-  `Get` 在读取属性时调用的函数。 默认值undefined
-  `Set`  在写入属性时调用的函数。默认值undefined

### 工厂模式 ###
```javascript
function createPerson(name, age, job) {
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function() {
		alert(this.name)
	};
	return o;
}
var person1 = createPerson('Nicholas',19,'Software Engineer');
var person2 = createPerson('Greg',20,'Doctor');
```

无数次调用函数createPerson都能够根据参数返回包含三个属性的的Person对象。缺点：怎样知道一个对象的类型
#### 构造函数模式
```javascript
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function() {
		alert(this.name)
	};
}

var person1 = new Person('Nicholas',19,'Software Engineer');
var person2 = new Person('Greg',20,'Doctor');
```


和工厂模式的不同之处：
- 没有显式地创建对象
- 直接将属性和方法赋给了this对象
- 没有return语句
- 函数的首字母大写

创建Person的新实例，必须使用的new操作符。
new的过程有4个步骤：
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（this指向了这个新对象）
3. 执行构造函数中的代码（为这个新对象添加属性）
4. 返回新对象

每个实例都有一个constructor（构造函数）属性，该属性指向Person。

使用instanceof检测：
```javascript
alert(person1 instanceof Object); //true
alert(person1 instanceof Person); //true
alert(person2 instanceof Object); //true
alert(person2 instanceof Person); //true
```

作为普通函数调用:

```javascript
// 作为普通函数调用
Person('Greg',20,'Doctor')；
window.sayName(); //'Greg'
```

构造函数的缺点：方法不共享，每个实例都有自己的方法。

临时解决方案：（把方法放在对象外面）
```javascript
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = sayName;
}

function sayName = function() {
	alert(this.name)
};
```


### 原型模式 ###

我们创建的每一个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。照字面意思理解，那么prototype就是通过调用构造函数而创建的那个对象实例的原型对象。好处：可以让所有对象实例共享它所包含的属性和方法。

示例：

```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 19;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
}

var person1 = new Person();
person1.sayName(); //"Nicholas"

var person2 = new Person();
person2.sayName(); //"Nicholas"

alert(person1.sayName == person2.sayName); //true
```

### 理解原型对象 ###
无论什么时候，只要创建一个新函数，就会根据一组特定规则为该函数创建一个prototype属性，这个属性指向函数的原型对象，在默认情况下所有原型对象都会自动获得一个constructor（构造函数）属性，这个属性包含一个指向prototype属性所在函数的指针。其实Person.prototype.constructor指向Person。

创建了自定义的构造函数之后，其原型对象默认只会取得constructor属性，其他方法都是从object继承而来的。
当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象。ECMA-262第5版中管这个指针叫`Prototype`，这个属性没有标准的访问方式，在执行过程中是不可见的。浏览器中调试可以看见其实就是\_proto\_ 。

要明确的真正重要的一点是：这个连接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。

图示：
![enter image description here](http://s2.51offer.com/event/obj/img/2016-06-03/obj-pro_03.jpg)


可以通过isPrototypeOf()方法来确定对象之间是否存在这种关系。如果`Prototype`指向调用isPrototypeOf()方法对象（Person.prototype），那么方法返回true。

```javascript
alert(Person.prototype.isPrototypeOf(person1); //true
alert(Person.prototype.isPrototypeOf(person2); //true
```

ECMAScript5 新增方法object.getPrototypeOf()，此方法返回`Prototype`的值。

```javascript
alert(object.getPrototypeOf(person1) == Person.prototype); //true
alert(object.getPrototypeOf(person1).name); // Nicholas
```

多个对象实例共享原型对象的属性和方法的原理：
每当对象读取某个对象的某个属性是，都会执行一次搜索，目标就是给定名字的属性，搜索首先从对象实例本身开始，如果在实例中找到了具有给定名字的属性，则返回该属性的值。如果没找到，则继续搜索指针指向的原型对象，原型对象中查找给定名字的属性，如果在原型对象中查找到给定名字的属性，则返回该属性的值。如果原型对象中也没找到给定名字的属性，最后返回undefined。

原型最初只包含constructor，而该属性也是共享的，因此可以通过对象实例访问。

虽然对象实例可以访问原型对象中的值，但是对象实例是不可以重写原型对象中的值，只可以‘屏蔽’原型对象中同名属性。

```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 19;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
}

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";

alert(person1.name); //"Greg" 来自实例
alert(person2.name); //"Nicholas" 来自原型
```

使用delete可以完全删除实例属性，实例可以重新访问原型对象中属性。

```javascript
delete person1.name; 
alert(person1.name); //"Nicholas" 来自原型
```

使用hasOwnPrototype()方法可以检测一个属性是存在于实例中，还是存在于原型中。这个方法（从object继承来的）只在给定属性存在于对象实例中时，才会返回true。

```javascript
alert(person1.hasOwnPrototype('name')); //false

person1.name = "Greg";
alert(person1.name); //"Greg" 来自实例
alert(person1.hasOwnPrototype('name')); //true

alert(person2.name); //"Nicholas" 来自原型
alert(person2.hasOwnPrototype('name')); //false

delete person1.name; 
alert(person1.name); //"Nicholas" 来自原型
alert(person1.hasOwnPrototype('name')); //false
```

图示：
![enter image description here](http://s2.51offer.com/event/obj/2016-06-03/own-pro.png)

#### 原型与in操作符

有两种方式使用in操作符：单独使用和在for-in循环中使用。

在单独使用时，in操作符会在通过对象能够访问给定属性时返回true。（该属性无论存在实例中还是原型对象中）

```javascript
person1.name = "Greg";
alert(person1.name); //"Greg" 来自实例
alert("name" in person1); //true

alert(person2.name); //"Nicholas" 来自原型
alert("name" in person2); //true
```

同时使用hasOwnPrototype()和in操作符，就可以确定该属性到底存在实例中，还是原型对象中。
	
```javascript
function hasPrototypePrototy(object,name){
	return !object.hasOwnPrototype(name) && (name in object);
}

var person = new Person();
alert(hasOwnPrototype(person,"name")); //false

person.name = "Greg";
alert(hasOwnPrototype(person,"name")); //true
```

使用for-in循环时，返回的是所有能够通过对象访问的、可枚举的（enumerated）属性，包括实例和原型对象中的属性。获取可枚举的实例属性，可以使用ECMAScript 5的Object.keys()方法。这个方法接收一个对象参数，返回一个包含所有可枚举属性的字符串数组。

```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 19;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
}

var keys = Object.keys(Person.prototype);
alert(keys); // name, age, job, sayName

var person1 = new Person();
person1.name = "Greg";
person1.age = 19;

var p1keys = Object.keys(person1);
alert(p1keys); // name, age
```

如果想要得到所有实例属性，无论它是否可以枚举，都可以使用Object.getOwnPropertyNames()方法。

```javascript
var keys = Object.getOwnPropertyNames(Person.prototype);
alert(keys); //constructor, name, age, job, sayName
```

结果中包含了可枚举的constructor属性。Object.keys()和Object.getOwnPropertyNames()都可以替代for-in循环。


### 更简单原型语法 ###

用包含所有属性和方法的对象字面量来重写整个原型对象：

```javascript
function Person(){
}

Person.prototype = {
	name: "Nicholas",
	age: 19,
	job: "Software Engineer",
	sayName: function() {
		alert(this.name);
	}
}
```

最终结果相同但是有一个例外，constructor属性不再指向Person了，这种语法本质上完全重写了默认的prototype对象。因此constructor属性也就变成了新对象的constructor属性（指向Object构造函数），不再指向Person构造函数。尽管instanceof操作符还能返回正确的结果，但通过constructor已经无法确定对象类型了。

```javascript
var friend = new Person();

alert(friend instanceof Object); //true
alert(friend instanceof Person; //true

alert(friend.constructor == Person); //false
alert(friend.constructor == Object); //true
```
可以特意将constructor属性设置回适当的值：

```javascript
function Person(){

}

Person.prototype = {
	constructor: Person,
	name: "Nicholas",
	age: 19,
	job: "Software Engineer",
	sayName: function() {
		alert(this.name);
	}
}
```

以这种方式重设constructor属性会导致它的`Enumerable`特性被设置为true。默认情况下，原生的constructor属性是不可枚举的。

ECMAScript5的JavaScript引擎，可以用Object.defineProperty()重置`Enumerable`特性。

```javascript
Object.defineProperty(Person.prototype, "constructor", {
	enumerable: false,
	value: Person
});
```

### 原型的动态性 ###

由于在原型中查找值得过程是一次搜索，因此我们对原型对象所做的任何修改都能立即从实例上反应出来。先创建实例后修改原型也照样如此：

```javascript
var friend = new Person();
Person.prototype.sayHi = function(){
	alert('Hi');
}

friend.sayHi(); // Hi(没有问题)
```

我们调用sayHi()时，首先去实例中搜索名为sayHi属性，找不到就会继续搜索原型对象。实例和原型对象之间的连接只是一个指针，而非一个副本，因此就可以在原型对象中找到新的sayHi属性，并返回保存在那里的函数。

如果我们重写了原型对象，那么情况就不一样了。我们知道，调用函数时会为函数添加一个指向原型对象的`Prototype`指针（即\_proto\_），而把原型指向另一个对象就等于切断了构造函数与原型之间的联系。请记住：实例中的指针仅指向原型，而不指向构造函数。

```javascript
function Person() {

}

var friend = new Person();

Person.prototype = {
	constructor: Person,
	name: "Nicholas",
	age: 19,
	job: "Software Engineer",
	sayName: function() {
		alert(this.name);
	}
}

friend.sayName(); //error
```

这个例子中，我们先创建了Person的一个实例，然后又重写了其原型对象。然后在调用friend.sayName()时发生了错误，因为friend指向的原型中不包含以该名字命名的属性。

![enter image description here](http://s2.51offer.com/event/obj/2016-06-03/re-pro.png)

从图上看，重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系；它们引用的仍然是最初的原型。

### 原型对象的缺点 ###

原型中的所有属性是被很多实例共享，这种共享对于函数非常合适。对于引用类型的属性就会导致多个实例共享同一个引用类型的属性，其中一个实例修改了这个属性，这个属性也会反映到其他实例中。实例一般都是要有属于自己的全部属性的。

```javascript
function Person() {
}

Person.prototype = {
	constructor: Person,
	name: "Nicholas",
	age: 19,
	job: "Software Engineer",
	friends:["Shelby","Court"],
	sayName: function() {
		alert(this.name);
	}
}

var person1 = new Person();
var person2 = new Person();

person1.friends.push("Van");

alert(person1.friends); // Shelby, Court, Van
alert(person2.friends); // Shelby, Court, Van
alert(person1.friends === person2.friends); //true
```

#### 组合使用构造函数模式和原型模式

创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。每一个实例都会有自己的一份实例副本，但同时共享着对方法的引用，节省内存，支持向构造函数传递参数；可谓是集两种模式之长。

```javascript
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.friends = ["Shelby","Court"];
}

Person.prototype = {
	constructor: Person,
	sayName: function() {
		alert(this.name);
	}
}

var person1 = new Person("Nicholas",19,"Software Engineer");
var person2 = new Person("Greg",18,"Doctor");

person1.friends.push("Van");

alert(person1.friends); // Shelby, Court, Van
alert(person2.friends); // Shelby, Court
alert(person1.friends === person2.friends); //false
alert(person1.sayName === person2.sayName); //true
```

### 动态原型模式 ###

这个模式是把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。

```javascript
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.friends = ["Shelby","Court"];
	if(typeof this.sayName != "function"){
		Person.prototype.sayName = function() {
			alert(this.name);
		}
	}
}

var friend = new Person("Nicholas",19,"Software Engineer");

friend.sayName();
```

使用动态原型模式时，不能使用对象字面量重写原型。前面已经解释过了，如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。

### 寄生构造函数模式 ###

```javascript
function Person(name, age, job) {
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function() {
		alert(this.name);
	}
	return o;
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName(); // "Nicholas"
```

Person函数创建了一个新对象，并以相应的属性和方法初始化该对象，然后又返回了这个对象。

### 继承 ###

OO语言都支持两种继承方式：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。由于函数没有签名，所以ECMAScript只支持实现继承，而且其实现继承主要依靠原型链来实现的。

#### 原型链
基本思想就是利用原型让一个引用类型继承另一个引用类型的属性和方法。
原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针`constructor`，而实例都包含一个指向原型内部的指针。假如让原型对象等于另一个类型的实例，原型对象将包含一个指向另一个原型的指针，相应地，另一个原型也包含一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。

```javascript
function SuperType(){
	this.property = true;
}
SuperType.prototype.getSuperValue = function(){
	return this.property;
}
function SubType(){
	this.subproperty = false;
}

// 继承了SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function(){
	return this.subproperty;
}

var instance = new SubType();
alert(instance.getSuperValue()); //true
```

继承是通过创建SuperType的实例，并将该实例赋给SubType.prototype实现的。实现的本质是重写原型对象，代之以一个新类型的实例。原来存在于SuperType的实例中的所有属性和方法，现在也存在于SubType.prototype中了。

![enter image description here](http://s2.51offer.com/event/prototype/2016-06-15/proto-img.png)

所有引用类型默认都继承了Object，而这个继承也是通过原型实现的。

![enter image description here](http://s2.51offer.com/event/obj/2016-06-16/obj-img.jpg)
