#《了不起的Node.js》读书笔记（1）

## 第二章 JavaScript概览
### 一、JavaScript基础
1. 数据类型
    1. 基本类型，包括 `number`、 `boolean`、 `string`、 `null`、 `undefined`。访问基础类型，访问的是值。
    2. 复杂类型，包括 `array`、 `function`、 `object`。访问复杂类型，访问的是值的引用。
    3. 一些特定的值。下面的一些值被认为等于`false`：`null`、 `undefined`、 `空格`、`0`。
            
            var a = 0;
            if (a){
                // 这里不会执行
            }
            a == false; // true
            a === false; // false
            typeof null == 'object'; // true
            typeof [] == 'object'; // true
            // 判断是否数组
            Object.prototype.toString.call([]) == '[object Array]';

2. 函数
    1. 在一个函数中，`this`的值是全局对象，在浏览器中，就是`window`对象。

            function fn(){
                console.log(window == this); // true
            }
    使用`call`和`apply`方法可以改变函数中`this`的值。

            function fn(){
                console.log(this.name); // zhang
            }
            fn.call({name: 'zhang'});
    `call`和`apply`的区别是，`call`接受参数列表，`apply`接受一个参数数组。

            function fn(arg1,arg2){
                console.log(arg1); // first
                console.log(arg2); // second
            }
            var obj = {};
            fn.call(obj, 'first', 'second');
            fn.apply(obj, ['first', 'second']);
    2. 函数有一个属性为`length`，返回函数声明时的参数数量。

            function fn(a, b, c){}
            console.log(fn.length); // 3
    3. 闭包。在某个作用域中定义的变量，只能在该作用域或其内部作用域中才能访问到。

            var a = 5;
            function woot(){
                console.log(a); // 5 实际测试中为undefined
                var a = 6;
                function test(){
                    console.log(a); // 6
                }
                test();
            }
            woot();

    自执行函数机制。声明和调用一个匿名函数，能够达到仅定义一个新作用域的作用。

            var a = 3;
            (function(){
                var a = 5;
                console.log(a); // 5
            })();
            console.log(a); // 3

3. 类
    1. 类的定义。通过函数定义。

            function Person($name){
                this.name = $name;
            }
    2. 类的实例。

            var p = new Person('esinger');
    3. 类的方法。指类的实例具有的方法。可以使用prototype属性来定义。

            Person.prototype.say = function(){
                console.log(this.name);
            }
    在方法内部，`this`关键字指向的是通过该类创建的实例对象。

            // 一个完整实例
            // 定义类
            function Person(name) {
                this.name = name;
            }
            // 定义类的方法
            Person.prototype.say = function () {
                console.log(this.name);
            }
            // 创建一个实例
            var p1 = new Person('zhang');
            p1.say(); // zhang
            // 创建另一个实例
            var p2 = new Person('liu');
            p2.say(); // liu

    4. 类的静态方法。
    5. 类的继承。基于原型模拟类继承。

            // 新建一个子类
            function Men(name) {
                this.name = name;
            }
            // 把父类的一个实例赋给字类的prototype
            Men.prototype = new Person();
            // 定义子类的属性
            Men.prototype.sex = 'male';
             // 重构方法
            Men.prototype.say = function(){
                Person.prototype.say.call(this);
                // 添加新的业务
                console.log(this.sex);
            }
            // 定义子类的方法
            Men.prototype.intro = function () {
                console.log(this.name + '/' + this.sex);
            }
            // 新建一个实例
            var m1 = new Men('wang');
            m1.say(); // wang
            m1.intro(); // wang/male

4. V8中的JavaScript
    1. `Object.keys`，获取对象上所有的自有键。

            var obj = {a: 'aa', b: 'bb'};
            console.log(Object.keys(obj); // ['a', 'b']
    2. `Array.isArray`，检查是否为数组。

            var arr = [];
            console.log(typeof arr); // object
            console.log(Array.isArray(arr)); // true
    3. `forEach`，遍历数组。

                [1, 2, 3].forEach(function (v) {
                    console.log(v); // 打印输出1,2,3
                });

    4. `filter`，过滤数组元素。

                var arr = [1, 2, 3];
                console.log(arr.filter(function (v) {
                    return v < 3;
                })); // 输出[1, 2]
                console.log(arr); // 输出[1, 2, 3]
    5. `map`，遍历数组，返回每个元素计算后的值组成的新数组。

                var arr = [1, 2, 3];
                console.log(arr.map(function (v) {
                    return v * 2;
                })); // 输出[2, 4, 6]
                console.log(arr); // 输出[1, 2, 3]
    6. 字符串方法 `trim`，移除字符串首末的空格。

                console.log(' hello  '.trim()); // hello
    5. JSON `JSON.stringify` 解码，把JSON对象转换成字符串 `JSON.parse` 编码，把字符串转换成JSON对象。

                var a = {a:1,b:2};
                var b = JSON.stringify(a);
                var c = JSON.parse(b);
                console.log(a); // Object {a: 1, b: 2}
                console.log(typeof a); // object
                console.log(b); // {"a":1,"b":2}
                console.log(typeof b); // string
                console.log(c); // Object {a: 1, b: 2}
                console.log(typeof c); // object
    6. 继承 `__proto__`

            function Person(name){
                this.name = name;
            }
        
            Person.prototype.say = function(){
                console.log(this.name);
            }
        
            var p = new Person('zhang');
            p.say(); // zhang
        
            function Men(name){
                this.name = name;
            }
        
            Men.prototype.__proto__ = Person.prototype;
        
            var m = new Men('wang');
            m.say(); // wang
    7. 存取器，通过调用方法来定义属性。
        1. `__defineGetter__`
        2. `__defineSetter__`

                Date.prototype.__defineGetter__('year', function(){
                    return this.getFullYear();
                });
            
                Date.prototype.__defineSetter__('year', function(value){
                    this.setFullYear(value);
                });
            
                var d = new Date();
                console.log(d.year); // 2014
                d.year = 2008;
                console.log(d.getFullYear()); // 2008
