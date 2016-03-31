# JavaScript语言精粹笔记

##### 1. \__proto__(隐式原型)、prototype(显式原型)
  
\__proto__: 指向创建这个对象的函数的prototype, 每个对象特有的属性.

prototype: 指向函数的原型对象, 每个函数被创建后所产生的属性.

共同点：都可以用于实现基于原型的继承.	

基于原型继承的例子：

例子1(基于prototype的函数对象继承):
	
	function Employee (name) {
		//实例属性：姓名
		this.name = name;
	}
	Employee.prototype = {
		constructor: Employee,
		getName: function () {
			return this.name;
		}
	};
	function Manager (name, percentage) {
		//对象冒充, 实现属性继承
		Employee.apply(this, [name]);
		//实例属性：提成
		this.percentage = percentage;
	}
	
	//将父类的实例设置为子类的原型, 实现方法继承
	Manager.prototype = new Employee();
	//修改子类原型的constructor指向, 避免子类实例的constructor属性指向父类的构造函数
	Manager.prototype.constructor = Manager;
	//给子类添加新的实例方法
	Manager.prototype.getPercentage = function () {
		return this.percentage;
	};			
([关于继承的详解](http://web.jobbole.com/85460/))
	
例子2_1(基于__proto__的普通对象基础):

	var employee = {
		name: "Caden"
	};
	/*方法一*/
	var manager = {
		percentage: 1000
	};
	manager.__proto__ = employee;
	manager.name = "Jerk";
	
	/*方法二*/
	var manager = Object.create(employee);
	
	console.log(manager.name, manager.percentage); //输出: Jerk, 1000

##### 2. 方法、函数

- 方法: 当一个函数被保存为对象的一个属性时, 我们称它为一个方法.

	  例：
	  var myObj = {
	      value: 0,
	      getValue: function () {
	          return this.value;
	      }
	  };
	  
	  //如上，创建一个myObj对象，它有一个 value 属性和一个 getValue 方法
			
- 函数: 当一个函数并非一个对象的属性时, 那么它就是被当做一个函数来调用.
			
##### 3. 基本类型扩充

我们可以通过给 Function.prototype 增加方法来使得该方法对所有函数都可用:

	Function.prototype.method = function (name, func) {
		this.prototype[name] = func;
		return this;
	}

JavaScript 没有专门的整数类型, 但有时候的确只需要提取数字中的整数部分。JavaScript 本身提供的取整方法又有些丑陋。我们就可以通过给 Number.prototype 增加一个 integer 方法来改善它。

它会根据数字的正负来判断是使用 Math.ceiling 还是 Math.floor 。
	
	Number.method('integer', function () {
		return Math[this < 0 ? 'cell' : 'floor'](this);
	});
	
	document.writeln((-10/3).integer()); //-3
		
			