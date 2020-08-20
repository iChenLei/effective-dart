<p align="center">
                <img src="https://jaywcjlove.github.io/sb/lang/chinese.svg" />
		<img src="https://img.shields.io/badge/ComputerLanguage-dartlang-green.svg?style=flat-square"/>
		<img src="https://img.shields.io/badge/status-WIP-red.svg?style=flat-square"/>
		<a href="www.chenleiblogs.com"><img src="https://img.shields.io/badge/author-@iChenLei-green.svg?style=flat-square"/></a>
		<img src="https://img.shields.io/badge/welcome-contributing-blue.svg?style=flat-square"/>
</p>

| Ⅰ | Ⅱ | Ⅲ | Ⅳ | 
| :--------: | :---------: | :---------: | :---------: |
| [Style Guide](style.md):nail_care: | [Usage Guide](usage.md):bookmark:|[Documentation Guide](documentation.md):page_with_curl: | [Design Guide](design.md):gem: |

### What is the project ?
```dart
	// dart language 
	void main() {
		Map repo = new Map()
		  ..addAll({'name':'Effective Dart 中文版'})
		  ..addAll({'author':'雷仔'})
                  ..allAll({'lang':'中文'})
		  ..adddAll({'status':'work in progress'});
		print(repo);
	}
```
## Table of contents
## 🔥 Effective Dart: Style 🔥  

1. **标识符命名**  
    - [DO] [类型命名使用大写驼峰风格](style.md#1-1)  
    - [DO] [库个源文件命名使用小写加下划线风格](style.md#1-2)  
    - [DO] [import前缀命名使用小写加下划线风格](style.md#1-3)  
    - [DO] [其他命名使用小写驼峰风格](style.md#1-4)  
    - [PREFER] [常量命名使用小写驼峰风格](style.md#1-5)  
    - [DO]  [缩略词大写](style.md#1-6)  
    - [DON'T] [不要使用前缀字符](style.md#1-7)  
2. **代码顺序** 
    - [DO] [将`dart:xx`导入代码放在其他导入之前](style.md#2-1)
    - [DO] [将`package:xx`导入代码放在相对文件导入之前](style.md#2-2)  
    - [PREFER] [将自己的包的引入代码放在其他三方包引入之后](style.md#2-3)  
    - [DO] [导出代码应该放在所有导入代码之后](style.md#2-4) 
    - [DO] [同一类导入代码顺序根据文件或者包名的首字母排序](style.md#2-5) 
3. **格式化**      
    - [DO] [使用`dartfmt`工具格式化你的代码](style.md#3-1)  
    - [CONSIDER] [修改你的代码使之变得对格式化工具友好](style.md#3-2)  
    - [AVOID] [单行代码不超过80个字符](style.md#3-3)  
    - [DO] [使用大括号包裹你的控制流结构](style.md#3-4)

### Effective Dart: Usage

- 库
    - [DO] [`part of`指令之后使用字符串](usage.md#1-1)
    - [DON'T] [不要在你的库的src目录下引入其他库](usage.md#1-2)
    - [PREFER] [库lib目录下的引入请使用相对路径](usage.md#1-3)
- 字符串
    - [DO] [使用`adjacent strings`串联字符串而不是使用`+`号](usage.md#2-1)
    - [PREFER] [使用模板字符串来拼接值和字符而不是用`+`拼接](usage.md#2-2)
    - [AVOID]  [在不需要使用大括号时省略大括号](usage.md#2-3)
- 集合
    - [DO] [尽量使用字面量定义集合](usage.md#3-1)
    - [DON'T] [不要使用`.length`去判断集合是否为空](usage.md#3-2)
    - [CONSIDER] [使用高阶函数对集合进行转换处理](usage.md#3-3)
    - [AVOID] [避免在`Iterable.forEach()`里写函数](usage.md#3-4)
    - [DON'T] [不要使用`List.from()`除非你想转换集合类型](usage.md#3-5)
    - [DO] [使用`whereType()`去过滤集合类型](usage.md#3-6)
    - [DON'T] [当其他操作符可以转换类型时不要使用`cast()`](usage.md#3-7)
    - [AVOID] [避免使用`cast()`](usage.md#3-8)
- 函数
    - [DO] [直接声明函数而不是将lambda函数赋值给一个变量](usage.md#4-1)
    - [DON'T] [`lambda`表达式尽量简洁](usage.md#4-2)
- 参数
    - [DO] [使用`=`为吗命名参数设置默认值](usage.md#5-1)
    - [DON'T] [不要将默认值显式设置为`null`](usage.md#5-2)
- 变量
    - [DON'T] [不要将初始化变量显式设置为`null`](usage.md#6-1)
    - [AVOID] [避免存储你可以计算的值](usage.md#6-2)
- 成员
    - [DON'T] [不要在不必要的时候设置`getter`和`setter`](usage.md#7-1)
    - [PREFER] [使用`final`声明一个只读属性](usage.md#7-2)
    - [CONSIDER] [对于一个简单属性的获取使用`=>`](usage.md#7-3)
    - [DON'T] [不要在不必要的时候使用`this`](usage.md#7-4)
    - [DO] [尽量在初始值声明时赋值初始值](usage.md#7-5)
- 构造函数
    - [DO] [尽量使用简洁的构造函数声明方式](usage.md#8-1)
    - [DON'T] [不要为构造函数参数声明类型](usage.md#8-2)
    - [DO] [构造函数body为空时使用`;`而不是`{}`](usage.md#8-3)
    - [DON'T] [不要使用`new`关键字声明实例](usage.md#8-4)
    - [DON'T] [不要重复冗余的声明`const`](usage.md#8-5)
- 错误处理
    - [AVOID] [避免在没有条件控制下捕捉错误](usage.md#9-1)
    - [DON'T] [不要忽略错误](usage.md#9-2)
    - [DO] [仅仅在语法错误的情况下抛出实现`Error`的类](usage.md#9-3)
    - [DON'T] [开发时不要对错误做处理，let's crash](usage.md#9-4)
    - [DO] [使用`rethrow`关键词重新抛出无法处理的异常](usage.md#9-5)
- 异步
    - [PREFER] [使用`async/await`优于传统的`Future`](usage.md#10-1)
    - [DON'T] [不要在`async`没有任何作用时使用它](usage.md#10-2)
    - [CONSIDER] [使用高阶函数处理转换流`stream`](usage.md#10-3)
    - [AVOID] [避免直接使用`Completer`类](usage.md#10-4)
    - [DO] [当参数声明类型为`Future<T>`时候，参数可能为`Object`的情况下请用`Future<T>`做类型判断](usage.md#10-5)

## Effective Dart: Documentation 

- 注释
    - [DO] [用描述性的语句写注释](documentation.md#1-1)
    - [DON'T] [使用块注释作为文档](documentation.md#1-2)
- 文档注释
    - [DO] [使用`\\\`文档注释去注释成员和类型](documentation.md#2-1)
    - [PREFER] [为公共API写文档注释](documentation.md#2-2)
    - [CONSIDER] [编写库级别的文档注释](documentation.md#2-3)
    - [CONSIDER] [为私有API编写文档注释](documentation.md#2-4)
    - [DO] [使用一句话完成文档总结](documentation.md#2-5)
    - [DO] [将文档注释段落的第一句话分离出来](documentation.md#2-6)
    - [AVOID] [注释避免冗余](documentation.md#2-7)
    - [PREFER] [注释函数或者方法时使用第三人称动词](documentation.md#2-8)
    - [PREFER] [注释变量，getter/setter时使用名词短语](documentation.md#2-9)
    - [PREFER] [注释库或者类型时使用名词短语](documentation.md#2-10)
    - [CONSIDER] [在注释中提供代码例子](documentation.md#2-11)
    - [DO] [在文档注释中使用方括号突出作用域內标识符](documentation.md#2-12)
    - [DO] [使用简介的描述来注释参数，返回值和抛出的异常](documentation.md#2-13)
    - [DO] [将文档注释放在元数据注解`@`之前](documentation.md#2-14)
- Markdown
    - [AVOID] [避免过度使用Markdown](documentation.md#3-1)
    - [AVOID] [避免使用HTML作为注释](documentation.md#3-2)
    - [PREFER] [使用反引号来区分代码块](documentation.md#3-3)
- 书写
    - [PREFER] [尽量简短](documentation.md#4-1)
    - [AVOID] [尽量避免使用缩略词除非它们很常见](documentation.md#4-2)
    - [PREFER] [使用`this`而不是`the`去指代实例本身](documentation.md#4-3)

## Effective Dart: Design
> WIP (work in progress) 

- 命名
    - [DO] [使用统一的命名策略](design.md#1-1)
    - [AVOID] [避免使用缩写](design.md#1-2)
    - [PREFER] [将最有意义的描述名词放在最后](design.md#1-3)
    - [CONSIDER] [增加代码可读性，使之语义化](design.md#1-4)
    - [PREFER] [非布尔值的属性或者变量命名使用名词短语](design.md#1-5)
    - [PREFER] [布尔值属性或者变量命名使用非命令性动词](design.md#1-6)
    - [CONSIDER] [布尔值命名参数请省略动词](design.md#1-7)
    - [PREFER] [布尔值属性或者变量命名时使用正面词汇命名](design.md#1-8)
    - [PREFER] [当函数或者方法会产生副作用时用动词短语命名](design.md#1-9)
    - [PREFER] [当函数或者方法有返回值时用名词短语命名](design.md#1-10)
    - [CONSIDER] [如果你想强调函数工作内容请用动词短语命名](design.md#1-11)
    - [AVOID] [避免使用get作为函数方法名字的开头](design.md#1-12)
    - [PREFER] [如果是将一个对象状态复制到另一个对象的方法请使用`to_()`格式命名](design.md#1-13)
    - [PREFER] [改变对象类型使用`as_()`格式命名](design.md#1-14)
    - [AVOID] [不要在函数方法名中出现参数名](design.md#1-15)
    - [DO] [当给类型参数命名时请遵循如下的助记符约定](design.md#1-16)
- 库
    - [PREFER] [声明为私有](design.md#2-1)
    - [CONSIDER] [可以在同一个库里声明多个类](design.md#2-2)
- 类
    - [AVOID] [避免定义单成员抽象类使用函数替代](design.md#3-1)
    - [AVOID] [避免定义只含一个静态成员的类](design.md#3-2)
    - [AVOID] [避免继承不想拥有子类的类](design.md#3-3)
    - [DO] [如果你的类支持继承请在文档里说明](design.md#3-4)
    - [AVOID] [避免实现不支持作为接口的类](design.md#3-5)
    - [DO] [如果你的类支持作为借口请在文档里说明](design.md#3-6)
    - [AVOID] [避免混合`mixin`不支持作为`mixin`的类](design.md#3-7)
    - [DO] [如果你的类支持作为`mixin`请在文档里说明](design.md#3-8)
- 构造函数
    - [PREFER] [建议定义构造函数而不是静态方法去生成实例](design.md#4-1)
    - [CONSIDER] [如果类支持请将构造函数声明为`const`](design.md#4-2)
- 类成员
    - [PREFER] [将顶级作用域的变量声明为`final`](design.md#4-1)
    - [DO] [为可获得的属性添加`getters`](design.md#4-2)
    - [DO] [为可设置的属性添加`setters`](design.md#4-3)
    - [DON'T] [不要定义`setters`时不定义对应的`getters`](design.md#4-4)  
    - [AVOID] [避免返回值返回`null`](design.md#4-5)
    - [AVOID] [避免直接返回`this`,链式调用请用cascade也就是`..`运算符](design.md#4-6)
- 类型
    - [PREFER] [声明顶级作用域变量类型](design.md#5-1)
    - [CONSIDER]  [声明私有作用域变量类型](design.md#5-2)
    - [AVOID] [避免声明初始化本地变量类型](design.md#5-3)
    - [AVOID] [避免在闭包中声明推断参数的类型](design.md#5-4)
    - [AVOID] [避免在使用泛型时重复冗余的声明类型](design.md#5-5)
    - [DO] [当推断类型时确定类型时请声明类型](design.md#5-6)
    - [PREFER] [使用`dynamic`声明类型而不是让推断直接fail掉](design.md#5-7) 
    - [PREFER] [当函数作为参数时请声明函数类型](design.md#5-8)
    - [DON'T] [不要为`setter`声明类型](design.md#5-9)
    - [DON'T] [不要使用遗留版本的`typedef`语法](design.md#5-10)
    - [PREFER] [使用函数类型声明而不是`typedef`](design.md#5-11) 
    - [CONSIDER] [函数作为参数时使用函数类型语法](design.md#5-12) 
    - [DO] [当参数可以是任意类型时用`Object`声明而不是`dynamic`](design.md#5-13)
    - [DO] [当异步函数不返回值时使用`Future<void>`声明](design.md#5-14)
    - [AVOID] [避免使用`FutureOr<T>`作为返回类型](design.md#5-15)
- 参数
    - [AVOID] [避免布尔值类型位置参数](design.md#6-1)
    - [AVOID] [如果你想省略一些参数请请避免使用位置参数](design.md#6-2)
    - [AVOID] [避免强制性参数当参数可以省略时](design.md#6-3)
    - [DO] [获取范围时将参数设置为左闭右开](design.md#6-4)
- 相等符
    - [DO] [如果你重载`==`请一并重载`hashcode`](design.md#7-1)
    - [DO] [请让你的`==`运算符遵守数学上的相等](design.md#7-2)
    - [AVOID] [避免为可变类定义常规意义上的相等](design.md#7-3)
    - [DON'T] [不需要在重载`==`运算符时判断类型是否为`null`](design.md#7-4)

#### Find a job?（:office: : Shanghai,China）:point_down:
> 上海寻梦科技（[拼多多](http://www.pinduoduo.com/social.html)） 高速上升期，招聘算法-前端-客户端-Java开发-Python开发-Golang开发等
> 如果你正在寻找合适的工作，内推请联系我投递简历（E-Mail: eXVubGVpQHBpbmR1b2R1by5jb20=）


###  License

[MIT](http://opensource.org/licenses/MIT)

Copyright (c) 2018-present, @iChenLei
