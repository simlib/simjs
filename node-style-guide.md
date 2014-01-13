# Node.js编码规范中文版(Node.js Style Guide)

### 说明： 
> 1. 本指南由[Felix Geisendörfer](http://felixge.de/)编写，原链接地址为[https://github.com/felixge/node-style-guide](https://github.com/felixge/node-style-guide)。
> 2. 此中文版本由[esinger](http://weibo.com/esinger)翻译整理，根据个人意见有少许修改，Github地址为[http://github.com/node-style-guide](http://github.com/node-style-guide)。
> 3. 此中文版不是原文的翻译版，而是根据原文意思整理成中文版本，有少许精简及修改。
> 4. 如果想看原文的翻译版，请移步[dead_horse](http://deadhorse.me/)翻译的版本：[https://github.com/dead-horse/node-style-guide](https://github.com/dead-horse/node-style-guide)。
> 4. 欢迎大家批评指正。

1. **代码缩进** 使用2个空格缩进，不要使用Tab或空格和Tab混用。
2. **换行** 使用UNIX风格的换行`\n`，并且把`\n`作为一个文件的最后一个字符。在任何项目中都禁止使用Windows风格的换行符`\r\n`。
3. **行尾空格** 在你提交代码前，清除所有的行尾空格。
4. **使用分号** 为了避免可能的一些错误，请使用分号`;`。
5. **每行80字符** 尽量使用每行字符不超过80个字符，大显示屏可以分成多屏显示。
6. **引号** 除JSON文件外的字符串使用单引号。
> 正确的写法：
        var foo = 'bar';
> 错误的写法：
        var foo = "bar";
7. **大括号位置** 左大括号放在语句开始的这一行。条件语句前后各加一个空格。
> 正确的写法：
        if (true) {
            // ...
        }
> 错误的写法：
        if (true)
        {
            // ...
        }
8. 一行只写一个变量声明，每个变量声明前面都要使用关键字`var`。并且变量声明不要都写到最前面，要放到最有意义的地方。
> 正确的写法：
        var keys = ['foo', 'bar'];
        var values = [23, 42];
        var object = {};
        while (items.length) {
            var key = keys.pop();
            object[key] = values.pop();
        }
> 错误的写法：
        var keys = ['foo', 'bar'],
            values = [23, 42],
            object = {},
            key;
            while (items.length){
                key = keys.pop();
                object[key] = values.pop();
            }
9. **变量、属性和函数名采用小驼峰法命名** 所有变量、属性和函数名都采用小驼峰命名法`lowerCamelCase`。并且要使用描述性的有意义的词，避免使用不常用的缩略语。
> 正确的写法：
        var user;
        var userId;
> 错误的写法：
        var u;
        var user_id;
10. **类名采用大驼峰法命名** 所有的类名使用大驼峰命名法`UpperCamelCase`。
> 正确的写法：
        function BankAccount() {
        }
> 错误的写法：
        function bankAccount() {
        }
        function bank_account(){
        }
11. **常量使用大写字母** 所有常量形式的变量或类的属性，都使用全大写字母。不要使用`const`关键字。
> 正确的写法：
        var PI = 3.1415926;
        function File(){
        }
        File.FULL_PERMISSIONS = 0777;
> 错误的写法：
        const PI = 3.1415926;
        function File() {
        }
        File.fullPermissions = 0777;
12. **对象创建** 使用`{}`代替`new Object`来创建对象。对象的key不要使用引号包裹，除非编译器报错。一行只写一个键值对，值后跟尾随逗号。
> 正确的写法：
        var obj = {};
        var user = {
            name: 'esinger',
            'is married': false
        };
> 错误的写法：
        var obj = new Object();
        var user = {name: 'esinger', 'is married': false};
        var user = {
            'name': 'esinger',
            is married: 'esinger'
        };
13. **数组创建** 使用`[]`代替`new Array`，每个成员后跟尾随逗号，所有成员写在一行内。
> 正确的写法：
        var arr = [];
        var users = ['esinger', 'simphp'];
> 错误的写法：
        var arr = new Array();
        var users = [
            'esinger',
            'simphp'
        ];
14. **使用===操作符** 使用`===`代替`==`操作符来进行比较，避免出现意料之外的结果。
> 正确的写法：
        var i = 0;
        if (i === ''){
            console.log('winning');
        }
> 错误的写法：
        var i = 0;
        if (i == ''){
            console.log('losing');
        }
15. **三元操作符** 把三元操作符分成三行写。
> 正确的写法：
        var b = (x === y)
            ? true
            : false;
> 错误的写法：
        var b = (x === y) ? true : false;
16. **不要扩展内建对象** 不要扩展内建JavaScript对象的方法。
> 正确的写法：
        var a = [];
        if (!a.length) {
            // ...
        }
> 错误的写法：
        Array.prototype.empty = function() {
            return !this.length;
        }
        var a = [];
        if (a.empty()) {
            // ...
        }
17. **使用具有描述性的有意义的判断条件** 所有复杂的条件应该赋予一个有意义的描述性的变量或函数。
> 正确的写法：
        var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);
        if (isValidPassword) {
            // ...
        }
> 错误的写法：
        if (password.length >= 4) && /^(?=.*\d).{4,}$/.test(password)) {
            // ...
        }
18. **书写简短的函数** 使每个函数尽可能的短小精简。
19. **尽早的从函数中返回** 避免深层嵌套语句，尽可能早的从函数中返回。
> 正确的写法：
        function isPercentage(val) {
            if (val < 0) {
                return false;
            }
            if (val > 100) {
                return false;
            }
            return true;
        }
> 错误的写法：
        function isPercentage(val) {
            if (val >= 0) {
                if (val < 100) {
                    return true;
                } else {
                    return false;
                }
            } else {
                return false;
            }
> 针对这个示例，可以进一步精简优化：
        function isPercentage(val) {
            var isInRange = (val >= 0 && val <= 100);
            return isInRange;
        }
> 针对这个示例，个人建议下面这样写也可以：
        function isPercentage(val) {
            return val >= 0 && val <= 100;
        }
20. **给闭包命名** 尽量给闭包命名，会产生更好的堆栈跟踪和CPU信息。
> 正确的写法：
        req.on('end' function onEnd() {
            // ...
        });
> 错误的写法：
        req.on('end' function() {
            // ...
        });
21. **不要嵌套闭包** 合理使用闭包，不要嵌套闭包。
> 正确的写法：
        setTimeout(function() {
            client.connect(afterConnect);
        }, 1000);
        function afterConnect() {
            // ...
        }
> 错误的写法：
        setTimeout(function() {
            client.connect(function() {
                // ...
            });
        }, 1000);
22. **使用slashes注释风格** 无论单行注释或者多行注释，都使用slashes`//`注释。尽量在更高层次编写注释，对难以理解的代码注释。
> 正确的写法：
        // 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
        var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

        // This function has a nasty side effect where a failure to increment a
        // redis counter used for statistics will cause an exception. This needs
        // to be fixed in a later iteration.
        function loadUser(id, cb) {
          // ...
        }
        
        var isSessionValid = (session.expires < Date.now());
        if (isSessionValid) {
          // ...
        }
> 错误的写法：
        // Execute a regex
        var matches = item.match(/ID_([^\n]+)=([^\n]+)/));
        
        // Usage: loadUser(5, function() { ... })
        function loadUser(id, cb) {
          // ...
        }
        
        // Check if the session is valid
        var isSessionValid = (session.expires < Date.now());
        // If the session is valid
        if (isSessionValid) {
          // ...
        }


