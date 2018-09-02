<a name="top"></a>
## 🔥 Effective Dart: Usage 🔥 

- 库
    - [DO] [`part of`指令之后使用字符串](#1-1)
    - [DON'T] [不要在你的库的src目录下引入其他库](#1-2)
    - [PREFER] [库lib目录下的引入请使用相对路径](#1-3)
- 字符串
    - [DO] [使用`adjacent strings`串联字符串而不是使用`+`号](#2-1)
    - [PREFER] [使用模板字符串来拼接值和字符而不是用`+`拼接](#2-2)
    - [AVOID]  [在不需要使用大括号时省略大括号](#2-3)
- 集合
    - [DO] [尽量使用字面量定义集合](#3-1)
    - [DON'T] [不要使用`.length`去判断集合是否为空](#3-2)
    - [CONSIDER] [使用高阶函数对集合进行转换处理](#3-3)
    - [AVOID] [避免在`Iterable.forEach()`里写函数](#3-4)
    - [DON'T] [不要使用`List.from()`除非你想转换集合类型](#3-5)
    - [DO] [使用`whereType()`去过滤集合类型](#3-6)
    - [DON'T] [当其他操作符可以转换类型时不要使用`cast()`](#3-7)
    - [AVOID] [避免使用`cast()`](#3-8)
- 函数
    - [DO] [直接声明函数而不是将lambda函数赋值给一个变量](#4-1)
    - [DON'T] [`lambda`表达式尽量简洁](#4-2)
- 参数
    - [DO] [使用`=`为吗命名参数设置默认值](#5-1)
    - [DON'T] [不要将默认值显式设置为`null`](#5-2)
- 变量
    - [DON'T] [不要将初始化变量显式设置为`null`](#6-1)
    - [AVOID] [避免存储你可以计算的值](#6-2)
- 成员
    - [DON'T] [不要在不必要的时候设置`getter`和`setter`](#7-1)
    - [PREFER] [使用`final`声明一个只读属性](#7-2)
    - [CONSIDER] [对于一个简单属性的获取使用`=>`](#7-3)
    - [DON'T] [不要在不必要的时候使用`this`](#7-4)
    - [DO] [尽量在初始值声明时赋值初始值](#7-5)
- 构造函数
    - [DO] [尽量使用简洁的构造函数声明方式](#8-1)
    - [DON'T] [不要为构造函数参数声明类型](#8-2)
    - [DO] [构造函数body为空时使用`;`而不是`{}`](#8-3)
    - [DON'T] [不要使用`new`关键字声明实例](#8-4)
    - [DON'T] [不要重复冗余的声明`const`](#8-5)
- 错误处理
    - [AVOID] [避免在没有条件控制下捕捉错误](#9-1)
    - [DON'T] [不要忽略错误](#9-2)
    - [DO] [仅仅在语法错误的情况下抛出实现`Error`的类](#9-3)
    - [DON'T] [开发时不要对错误做处理，let's crash](#9-4)
    - [DO] [使用`rethrow`关键词重新抛出无法处理的异常](#9-5)
- 异步
    - [PREFER] [使用`async/await`优于传统的`Future`](#10-1)
    - [DON'T] [不要在`async`没有任何作用时使用它](#10-2)
    - [CONSIDER] [使用高阶函数处理转换流`stream`](#10-3)
    - [AVOID] [避免直接使用`Completer`类](#10-4)
    - [DO] [当参数声明类型为`Future<T>`时候，参数可能为`Object`的情况下请用`Future<T>`做类型判断](#10-5)
### 库
> 下面的建议可以帮助你以一致的可维护的方式在多个文件中编写程序

<a name="1-1"></a>
#### [DO] `part of`指令之后使用字符串
> 许多dart开发者完全不使用`part`，因为他们发现当他们的库源文件是单文件时很容易读懂整个代码。如果你选择使用`part`来拆分你的库文件，Dart要求其他文件需要使用`part of`显式声明所属库。因为遗留原因Dart允许`part of`参数为库名，这让工具很难识别库的主文件，并且容易产生歧义。
> 更建议的是使用URI字符串的方式声明库主文件，就像你在其他诸如`import`指令一样，下面是一个例子：

```dart
    // good
    library my_library;
    part "some/other/file.dart";

    // good
    // your part file
    part of "../../my_library.dart";

    //bad
    part of my_library;
```
<a name="1-2"></a>
#### [DON'T] 不要引入第三方库`src`目录下的文件
> `lib`目录下的`src`目录所包含的源代码对于库来说是私有实现，包维护者对其包版本应该考虑这种约定，私有实现可以随意更改而不会对包产生破坏性更新。

> 这意味着如果你引入了其他包的私有库/文件，非破坏性的更新也会破坏你的代码。

<a name="1-3"></a>
#### [DO] 库lib目录下的引入请使用相对路径
```markdown
    my_package
        └─ lib
            ├─src
            |   └─ utils.dart
            └─api.dart
```
如果api.dart想要导入utils.dart，那么应该这么做：
```dart
    // good
    import 'src/utils.dart';

    // bad
    import 'package:my_package/src/utils.dart'
```
> 其实并没有很特别的理由选择前者，主要是前者描述短一点并且我们希望保持一致

**[⬆ back to top](#top)**

## 字符串
> 下面是一些Dart语言中处理字符串的最佳实践

<a name="2-1"></a>
##### [DO] 使用`adjacent strings`串联字符串而不是使用`+`号
> Dart中你可以使用如下的方式(相邻字符串)串联字符串，这种方式可以很容易将一个超长字符串分割多行且不用一直用`+`
```dart
    // good
    show(
        'what happend in the dartlang world'
        'and what can we do now ?');

    // bad

    show('what happend in the dartlang world'+
        'and what can we do now ?');
```
<a name="2-2"></a>
##### [PREFER] 使用模板字符串来拼接值和字符而不是用+拼接
> 如果你有ES6使用经验，相信你一定不会对模板字符串感到陌生，Dart也提供相同的功能

```dart
    // good
    'Hello , $name ! you are ${year - birth} years old';

    // bad
    'Hello ,'+name+' you are '+(year - birth).toString()+' years old';
```
<a name="2-3"></a>
##### [AVOID] 在不需要使用大括号时省略大括号
```dart
    // goods
    'Hi , $name'
    'Wear your wildest $decade's outfit'

    // bad
    'Hi, ${name}'
    "Wear your wildest ${decade}`s outfit"
```
**[⬆ back to top](#top)**

## 集合
> Dart提供开箱即用的集合类型：Maps,Sets,lists and queues,下面是一些最佳实践。

<a name="3-1"></a>
##### [DO] 尽量使用字面量定义集合
> 有两种方式定义一个空数组：`[]`和`List()`，同样的有三种方式定义Linked HashMap:`{}`,`Map()`和`LinkedHashMap()`
> 如果你想生成固定长度集合或者一些自定义类型集合请使用构造器，其他情况使用字面量语法。

```dart
    // good
    var points = [];
    var addresses = {};

    // bad
    var points = List();
    var addresses = Map();
```
> 必要时你可以申明集合类型
```dart
    // good
    var points = <Point>[];
    var addresses = <String, Addresses>{};

    // bad
    var points = List<Point>();
    var addresses = Map<String, Addresses>();
```
> 注意这些建议不适用于这些类的命名构造函数`List.from()`，`Map.fromIterable()`，这些方法有他们自己的用途。例如如果你想使用`List()`创建已知内容的集合
你可以使用他们

<a name="3-2"></a>
##### [DON'T] 不要使.length去判断集合是否为空
> 相比于使用`.length`去判定一个集合是否为空，更建议使用阅读性更强的`.isEmpty`和`.isNotEmpty`。
```dart
    // good 
    if ( list.isEmpty ) return 'this is a empty list';
    if ( array.isNotEmpty ) return 'wooo, a non-empty array';

    // bad
    if( list.length == 0 ) return 'this is a empty list';
    if( !array.isEmpty ) return 'wooo, a non-empty array';
```

<a name="3-3"></a>
##### [CONSIDER]使用高级函数去转换序列，也就是我们常说的函数式的写法
> 如果你想转换集合生成新集合，请使用诸如`.map()`，`.where()`等基于`Iterable`的函数
> 如果使用`for loop`方式会显得冗余并且容易产生副作用
```dart
    // good
    var coolBoy = Boys
        .where((boy) => boy.isRich)
        .where((boy) => boy.isTall)
        .map((boy) => boy.name);
```

<a name="3-4"></a>
##### [AVOID] 避免在Iterable.forEach()里写函数
> `forEach()`函数在JS中应用广泛，不过在Dart中想要遍历一个对象惯用的方法是使用`for-in`的方式
```dart
    // good
    for ( var i in people ) {
        // your function here
    }

    // bad
    people.forEach((i) {
        // your function here
    });
```
> 有一种情况例外那就是当我们的处理函数已存在（无需再次申明），并可以接受元素作为参数
```dart
    // good
    people.forEach(print);
```
<a name="3-5"></a>
##### [DON'T] 不要使用List.from()除非你想改变集合的类型
> 给你一个`Iterable`,这里有两种方式生成新的`List`(包含一样的子元素)
```dart
    var copy1 = iterable.toList();
    var copy2 = List.from(iterable);
```
> 上面两种方式明显的区别是第一种方式简短一点，重要的不同之处是第一种会保留参数类型
```dart
    // good

    // Creates a List<int>
    var iterable = [1,2,3]
    // Prints "List<int>"
    print(iterable.toList().runtimeType);

    // bad

    // Creates a List<int>
    var iterable = [1, 2, 3];
    // Prints "List<dynamic>":
    print(List.from(iterable).runtimeType);
```
> 如果你想改变类型，使用List.from()是很有用的
```dart
    var numbers = [1, 2.3, 4]; // List<num>.
    numbers.removeAt(1); // Now it only contains integers.
    var ints = List<int>.from(numbers); // List<int>
```
<a name="3-6"></a>
##### [DO]使用whereType()去过滤集合类型（Dart 2.X only）
> 如果你的集合包含多种类型，你只想获取int类型,你可以使用`.where()`
```dart
    // bad
    var objs = [1, '2', 3, '4'];
    var ints = objects.where((e) => e is int);
```    
> 有时候返回的类型可能不是你想要的，你会使用`.cast()`转换类型
```dart
    // bad
    var objs = [1, '2', 3, '4'];
    var ints = objs.where((e) => e is int).cast<int>();
```
> 上面的方式虽然解决了问题，却使用了两层处理产生了冗余的运行时判断，幸运的是Dart核心库现在提供了
`whereType()`方法解决这个问题。
```dart
    // good
    var objs = [1, '2', 3, '4'];
    var ints = objs.whereType<int>();
```
> 使用`whereType()`很简洁，可以生成自己想要的类型而不用多做一层处理

<a name="3-7"></a>
##### [DON'T] 当其他操作符可以转换类型时不要使用`cast()`
> 我们在处理`iterable`或者`stream`时经常需要做类型转换，经理不要使用`cast()`做类型转换
```dart
    // good
    var stuff = <dynamic>[1,2];
    var ints = List<int>.from(stuff)

    // bad
    var stuff = <dynamic>[1,2];
    var ints = stuff.toList().cast<int>();
```
> 在使用`map()`等方法时也可以省略掉`cast()`的使用
```dart
    // good
    var stuff = <dynamic>[1,2];
    var re = stuff.map<double>((n) => 1 / n);

    // bad
    var stuff = <dynamic>[1,2];
    var re = stuff.map((n) => 1 / n).cast<double>();
```
<a name="3-8"></a>
##### [AVOID] 避免使用cast()
> 避免使用`cast()`，用以下方式代替
- *声明正确的类型* 在集合声明时就指定正确的类型
- *在获取元素时转换类型* 如果你在遍历元素，在处理元素之前就使用`as`转换类型
- *使用`List.from()`做转换* 如果你需要获取集合中的大多数元素，请使用`List.from()`

> *声明正确的类型*的例子
```dart
    // good
    List<int> singletonList(int value) {
        var list = <int>[];
        list.add(value);
        return list;
    }

    //bad
    List<int> singletonList(int value) {
        var list = [];
        list.add(value);
        return list.cast<int>();
    }
```
> *在获取元素时转换类型*的例子
```dart
    // good
    void printEvens(List<Object> objects) {
        for (var n in objects) {
            if((n as int).isEven) print(n);
        }
    }

    // bad
    void printEvens(List<Object> objects) {
        for (var n in objects.cast<int>()) {
            if (n.isEven) print(n);
        }
    }
```
> *使用`List.from()`做转换*的例子
```dart
    // good
    int median(List<Object> objects) {
        var ints = List<int>.from(objects);
        ints.sort();
        return ints[ints.length ~/ 2];
    }

    // bad
    int median(List<Object> objects) {
        var ints = objects.cast<int>();
        inst.sort(); 
        return ints[ints.length ~/ 2];
    }
```
> 有时候`cast()`也是正确选择，但是考虑到这个方法使用有一定风险-操作可能会很慢且有时候会在运行时失败，不建议使用

**[⬆ back to top](#top)**

## 函数
> 在Dart中函数也是对象(Object)

<a name="4-1"></a>
##### [DO]直接声明函数而不是将lambda函数赋值给一个变量
> 现代语言都会提到嵌套函数和闭包的重要性，在一个函数中定义另一个函数是很常见的，在很多实例中这种类型的函数会被作为回调函数立即使用
且声明时不用命名。但是请直接声明函数而不是将lambda函数赋值给一个变量。
```dart
    // good
    void main() {
        localFunction() {
            // ...
        }
    }

    // bad
    void main() {
        var localFunction = () {
            ...
        };
    }
```
<a name="4-2"></a>
##### [DON'T] `lambda`表达式尽量简洁
> 使用已有功能的函数作为closure，而不是再一次去重复实现该功能
```dart
    // good
    names.forEach(print);

    // bad
    names.forEach((name) {
        print(name);
    })
```
**[⬆ back to top](#top)**
## 参数

<a name="5-1"></a>
##### [DO]使用=符号为命名参数设置默认值
> 因为历史遗留原因，Dart允许`:`和`=`为命名参数，为了和可选位置参数保持一致，请使用`=`
```dart
    // good
    void insert(Object item, {int at = 0}) { ... }

    // bad 
    void insert(Object item, {int at: 0}) { ... }
```

<a name="5-2"></a>
##### [DON'T] 不要将默认值显式设置为`null`
> 如果你创建了一个可选参数但是没有给予默认值，Dart会为你的参数设置默认值为`null`，所以你不用再做处理
```dart
    // good
    void error([String message]) {
        stderr.write(message ?? '\n');
    }

    // bad
    void error([String messgae = null]) {
        stderr.write(messgae ?? '\n');
    }
```
**[⬆ back to top](#top)**

## 变量
> 下面是一些在Dart中如何使用变量的最佳实践

<a name="6-1"></a>
##### [DON'T] 不要将初始值设置为null
> 在Dart中未赋值的变量都会被初始化为`null`，所以添加`= null`是多余不必要的。
```dart
    // good
    int _nextId;

    class LazyId {
        int _id;

        int get id {
            if (_nextId == null) _nextId = 0;
            if (_id == null) _id = _nextId++;

            return _id;
        }
    }

    // bad
    int _nextId = null;

    class LazyId {
        int _id = null;

        int get id {
            if (_nextId == null) _nextId = 0;
            if (_id == null) _id = _nextId++;

            return _id;
        }
    }
```
<a name="6-2"></a>
#### [AVOID] 不要存储你可以计算的变量
>当设计一个类时，你可能经常会在初始化时计算所有的属性并存储它们

```dart
    // bad
    class Circle {
        num radius;
        num area;
        num circumference;

        Circle(num radius)
            : radius = radius,
                area = pi * radius * radius,
                circumference = pi * 2.0 * radius;
    }
```
> 上面的代码有两个糟糕的地方：首先这很消耗内存，更糟糕的是Circle类的`radius`是可变的，当改变`radius`值后我们获取的`area`和`circumference`
还是之前的计算值，这就会导致错误。为了保证准确性我们可能会像下面这样做：
```dart
    // bad
    class Circle {
        num _radius;
        num get radius => _radius;
        set radius(num value) {
            _radius = value;
            _recalculate();
        }

        num _area;
        num get area => _area;

        num _circumference;
        num get circumference => _circumference;

        Circle(this._radius) {
            _recalculate();
        }

        void _recalculate() {
            _area = pi * _radius * _radius;
            _circumference = pi * 2.0 * _radius;
        }
    }
```
> 上面的解决方法难以阅读，表达性极差，取而代之的实现方式应该如下：

```dart
    // good
    class Circle {
        num radius;

        Circle(this.radius);

        num get area => pi * radius * radius;
        num get circumference => pi * 2.0 * radius;
    }
```
> 这样代码就显得很简洁，更少的内存占用，更少的错误产生。它只存储了必要的数据。在一些案例里，你可能需要
存储一些运算量较大的值，请谨慎这么做并写上注释解释为什么需要这么做优化。

**[⬆ back to top](#top)**

##成员
> 在Dart中Object可以有函数（方法）和数据（实例变量）类型的成员，下面是一些最佳实践

<a name="7-1"></a>
#### [DON'T] 不要在不必要的时候设置`getter`和`setter`
> 在Java或者C#中，成员变量通常是隐藏在`getter`和`setter`之后的，你需要写很多get/set的样板代码。Dart没有这样的限制，
声明的变量会自动设置`getter`和`setter`。
```dart
    // good
    class Box {
        var contents;
    }

    // bad
    class Box {
        var _contents;
        get contents => _contents;
        set contents(value) {
            _contents = value;
        }
    }
```
<a name="7-2"></a>
#### [PREFER] 使用`final`声明一个只读属性
> 如果你想设置一个只读属性变量，一个简单的解决办法是使用`final`标识变量
```dart
    // good
    class Box {
        final contents = [];
    }

    // bad
    class Box {
        var _contents;
        get contents => _contents;
    }
```
> 当然你或许需要在类的构造器外部去设置变量值，你可能需要`private field public getter`设计模式，这种情况下
第二种方法更合适你

<a name="7-3"></a>
#### [CONSIDER] 对于一个简单属性的获取使用`=>`
> 当计算表达式足够简单时使用`=>`,这种做法非常适合获取只需要简单计算的成员变量
```dart
    // good
    double get area => (right - left) * (bottom - top);

    bool isReady(num time) => minTime == null || minTime <= time;

    String capitalize(String name) =>
        '${name[0].toUpperCase()}${name.substring(1)}';
```

> 当我们的处理较为复杂的时候，不建议使用箭头函数这会让代码难以阅读。
```dart
    // good
    Treasure openChest(Chest chest, Point where) {
        if (_opened.containsKey(chest)) return null;

        var treasure = Treasure(where);
        treasure.addAll(chest.contents);
        _opened[chest] = treasure;
        return treasure;
    }

    // bad
    Treasure openChest(Chest chest, Point where) =>
    _opened.containsKey(chest) ? null : _opened[chest] = Treasure(where)
      ..addAll(chest.contents);
```
> 你也可以在设置`setter`时使用`=>`
```dart
    // good
    num get x => center.x;
    set x(num value) => center = Point(value, center.y);
```

<a name="7-4"></a>
#### [DON'T] 不要在不必要的时候使用`this`
> JS必须使用`this`来指向类去获取成员，不过Dart和Java C++等语言一样没有这种限制
> 唯一需要使用`this`的情况是你在成员函数里需要获取成员变量的时候

```dart
    // good
    class Box {
        var value;

        void clear() {
            update(null);
        }

        void update(value) {
            this.value = value;
        }
    }

    // bad
    class Box {
        var value;

        void clear() {
            this.update(null);
        }

        void update(value) {
            this.value = value;
        }
    }
```
> 注意构造初始化时参数赋值是不需要`this`的
```dart
    // good
    class Box extends BaseBox {
        var value;

        Box(value)
            : value = value,
                super(value);
    }
```
> 这看起来很意外，但是这种语法是可以正常工作的。

<a name="7-5"></a>
#### [DO] 尽量在初始值声明时赋值初始值
> 如果成员变量不依赖构造函数，那么它可以且应该在声明时初始化，这会让代码更加简洁并且避免了在多构造函数情况下忘记初始化
```dart
    // good
    class Folder {
        final String name;
        final List<Document> contents = [];

        Folder(this.name);
        Folder.temp() : name = 'temporary';
    }

    // bad
    class Folder {
        final String name;
        final List<Document> contents;

        Folder(this.name) : contents = [];
        Folder.temp() : name = 'temporary'; // Oops! Forgot contents.
    }
```
> 当然如果成员变量依赖构造函数参数，上面所说的就不适用了。

**[⬆ back to top](#top)**

## 构造函数

<a name="8-1"></a>
#### [DO] 尽量使用简洁的构造函数声明方式
> 成员变量初始化可以直接利用构造参数
```dart
    // good
    class Point {
        num x, y;
        Point(this.x, this.y);
    }

    // bad
    class Point {
        num x, y;
        Point(num x, num y) {
            this.x = x;
            this.y = y;
        }
    }
```

<a name="8-2"></a>
##### [DON'T] 不要为构造函数参数声明类型
> 如果构造函数参数使用了`this`去初始化成员变量，参数类型会自动匹配成员变量类型
```dart
    // good
    class Point {
        int x, y;
        Point(this.x, this.y);
    }

    // bad
    class Point {
        int x, y;
        Point(int this.x, int this.y);
    }
```
<a name="8-3"></a>
#### [DO] 构造函数body为空时使用`;`而不是`{}`
> 在Dart中，如果构造函数体为空请用冒号结尾
```dart
    // good
    class Point {
        int x, y;
        Point(this.x, this.y);
    }

    // bad
    class Point {
        int x, y;
        Point(this.x, this.y) {}
    }
```

<a name="8-4"></a>
#### [DON'T] 不要使用`new`关键字声明实例
> Dart2让`new`关键词成为可选项，这样让代码更加简洁
```dart
    // good
    Widget build(BuildContext context) {
        return Row(
            children: [
            RaisedButton(
                child: Text('Increment'),
            ),
            Text('Click!'),
            ],
        );
    }

    // bad
    Widget build(BuildContext context) {
        return new Row(
            children: [
            new RaisedButton(
                child: new Text('Increment'),
            ),
            new Text('Click!'),
            ],
        );
    }
```

<a name="8-5"></a>
#### [DON'T] 不要重复冗余的声明`const`
> 在状态为`const`的上下文环境中，`const`是隐式的可以省略
- `const`状态的集合
- `const`状态的构造函数
- 元数据注解
- `const`变量的初始化构造方法
- `switch`表达式`case`和`:`之间的区域
```dart
    // good
    const primaryColors = [
        Color("red", [255, 0, 0]),
        Color("green", [0, 255, 0]),
        Color("blue", [0, 0, 255]),
    ];

    // bad
    const primaryColors = const [
        const Color("red", const [255, 0, 0]),
        const Color("green", const [0, 255, 0]),
        const Color("blue", const [0, 0, 255]),
    ];
```
**[⬆ back to top](#top)**
## 错误处理
> Dart使用异常描述你的程序错误，下面是一些捕获和处理异常的最佳实践

<a name="9-1"></a>
#### [AVOID] 避免在没有条件控制下捕捉错误
> [TODO]

<a name="9-2"></a>
#### [DON'T] 不要忽略错误
> [TODO]

<a name="9-3"></a>
#### [DO] 仅仅在语法错误的情况下抛出实现`Error`的类
> [TODO]

<a name="9-4"></a>
#### [DON'T] 开发时不要对错误做处理，let's crash
> [TODO]

<a name="9-5"></a>
#### [DO] 使用`rethrow`关键词重新抛出无法处理的异常
> 当捕捉到的异常无法处理想要重新抛出异常时，请使用`rethrow`关键词而不是`throw`，因为`rethrow`会提供完整的异常调用栈。
而`throw`只提供抛出位置的调用栈。
```dart
    // good
    try {
        somethingRisky();
    } catch (e) {
        if (!canHandle(e)) rethrow;
        handle(e);
    }

    // bad
    try {
        somethingRisky();
    } catch (e) {
        if (!canHandle(e)) throw e;
        handle(e);
    }
```
**[⬆ back to top](#top)**
#### 异步
> Dart原生支持异步编程，一下是一些异步编程的最佳实践

<a name="10-1"></a>
#### [PREFER] 使用`async/await`优于传统的`Future`
> 异步代码是众所周知的难以调试，即使是使用了一些比较好的抽象例如`Future`。使用`async/await`语法可以提高代码的阅读性，
作用类似于JS中`async/await`之余`Promise`。

```dart
    // good
    Future<int> countActivePlayers(String teamName) async {
        try {
            var team = await downloadTeam(teamName);
            if (team == null) return 0;

            var players = await team.roster;
            return players.where((player) => player.isActive).length;
        } catch (e) {
            log.error(e);
            return 0;
        }
    }

    // bad
    Future<int> countActivePlayers(String teamName) {
        return downloadTeam(teamName).then((team) {
            if (team == null) return Future.value(0);

            return team.roster.then((players) {
            return players.where((player) => player.isActive).length;
            });
        }).catchError((e) {
            log.error(e);
            return 0;
        });
    }
```

<a name="10-2"></a>
#### [DON'T] 不要在`async`没有任何作用时使用它
> 任何函数都可以用`async`标识使之拥有异步能力，不过不能滥用`async`，在不需要的时候不要使用它
```dart
    // good
    Future afterTwoThings(Future first, Future second) {
         return Future.wait([first, second]);
    }

    // bad
    Future afterTwoThings(Future first, Future second) async {
        return Future.wait([first, second]);
    }
```
> `async`在以下几种情况下是很有用的：
- 你需要使用`await`
- 你需要返回一个异步错误，`async`加`throw`要比`return Future.error(...)`简洁
- 你需要返回值且值被隐式的包装为`Future`,`async`要比`Future.value(...)`简洁

```dart
    // good
    Future usesAwait(Future later) async {
        print(await later);
    }

    Future asyncError() async {
        throw 'Error!';
    }

    Future asyncValue() async => 'value';
```

<a name="10-3"></a>
#### [CONSIDER] 使用高阶函数处理转换流`stream`
> 和对`Iterable`的处理建议一样，`stream`也提供很多同样的方法，并且可以准确处理传输失败，关闭等事件

<a name="10-4"></a>
#### [AVOID] 避免直接使用`Completer`类
> 许多异步编程新手想要创建`Future`,Future构造函数貌似不能满足他们的需求，他们最终使用`Cpmpleter`来完成任务。
```dart
    // bad
    Future<bool> fileContainsBear(String path) {
        var completer = Completer<bool>();

        File(path).readAsString().then((contents) {
            completer.complete(contents.contains('bear'));
        });

        return completer.future;
    }
```
> 相较于使用`Future.then()`或者`async/await`，Completer看起来更加复杂和难以处理
```dart
    // good
    Future<bool> fileContainsBear(String path) {
        return File(path).readAsString().then((contents) {
            return contents.contains('bear');
        });
    }
```

```dart
    // good
    Future<bool> fileContainsBear(String path) async {
        var contents = await File(path).readAsString();
        return contents.contains('bear');
    }
```

<a name="10-5"></a>
#### [DO] 当参数声明类型为`FutureOr<T>`时候，参数可能为`Object`的情况下请用`Future<T>`做类型判断
> [TODO]
```dart
    // good
    Future<T> logValue<T>(FutureOr<T> value) async {
        if (value is Future<T>) {
            var result = await value;
            print(result);
            return result;
        } else {
            print(value);
            return value as T;
        }
    }

    // bad
    Future<T> logValue<T>(FutureOr<T> value) async {
        if (value is T) {
            print(value);
            return value;
        } else {
            var result = await value;
            print(result);
            return result;
        }
    }   
```
**[⬆ back to top](#top)**