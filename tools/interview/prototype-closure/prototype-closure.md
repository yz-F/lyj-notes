##理解javascript原型和作用域系列（1）——一切都是对象

1. 值类型不是对象，
2. typeof 检测值类型 undefined, number, string, boolean；剩下的都是引用类型，包括函数、数组、对象、null；其中 function 可以被 typeof 识别；最后还有一种可以识别的是 es6 symbol
3. 函数也是一种对象。他也是属性的集合，你也可以对函数进行自定义属性

##深入理解javascript原型和闭包（2）——函数和对象的关系
1. 对象是函数创建的，而函数却又是一种对象


    var fn = function () { };
    console.log(fn instanceof Object);  // true
    //对象可以通过函数来创建
    function Fn() {
                this.name = '王福朋';
                this.year = 1988;
            }
            var fn1 = new Fn();

#深入理解javascript原型和闭包（3）——prototype原型

1. 函数也是一种对象。他也是属性的集合，你也可以对函数进行自定义属性


    var obj = {
        a:10,
        b:function(x){
            alert(this.a+x);
        };
        c:{
            name:'王福朋',
            year:1988
        }
    }



    var fn = function () {
                alert(100);
            };
            fn.a = 10;
            fn.b = function () {
                alert(123);
            };
            fn.c = {
                name: "王福朋",
                year: 1988
            };


2. js就默认的给函数一个属性——prototype。对，每个函数都有一个属性叫做prototype。
> 每个函数都有一个属性叫做prototype。这个prototype的属性值是一个对象（属性的集合，再次强调！），默认的只有一个叫做constructor的属性，指向这个函数本身。

![prototype1](../../../image/closure/prototype1.png)
 如上图，SuperType是是一个函数，右侧的方框就是它的原型。

3.  原型既然作为对象，属性的集合，不可能就只弄个constructor来玩玩，肯定可以自定义的增加许多属性。例如这位Object大哥，人家的prototype里面，就有好几个其他属性。

![prototype1](../../../image/closure/prototype2.png)

>你也可以在自己自定义的方法的prototype中新增自己的属性


    function Fn() { }
            Fn.prototype.name = '王福朋';
            Fn.prototype.getYear = function () {
                return 1988;
            };


![prototype1](../../../image/closure/prototype3.png)       

4. 但是，这样做有何用呢？ —— 解决这个问题，咱们还是先说说jQuery吧。


        var $div = $('div');
        $div.attr('myName', '王福朋');


- 以上代码中，$('div')返回的是一个对象，对象——被函数创建的。假设创建这一对象的函数是 myjQuery。它其实是这样实现的。<br>


         myjQuery.prototype.attr = function () {
         //……
    };
    $('div') = new myjQuery();





> 即，Fn是一个函数，fn对象是从Fn函数new出来的，这样fn对象就可以调用Fn.prototype中的属性。



         function Fn() { }
        Fn.prototype.name = '王福朋';
        Fn.prototype.getYear = function () {
            return 1988;
        };

        var fn = new Fn();
        console.log(fn.name);
        console.log(fn.getYear());

>因为每个对象都有一个隐藏的属性——“__proto__”，这个 属 性引用了创建这个对象的函数的prototype。即：   fn.__proto__ === Fn.prototype
 这里的"__proto__"成为“隐式原型”        

### 深入理解javascript原型和闭包（4）——隐式原型
>obj.__proto__和Object.prototype
 每个函数function都有一个prototype，即原型。这里再加一句话——每个对象都有一个__proto__，可成为隐式原型。
 var obj = {}'
 console.log(obj.__proto__);
![prototype1](../../../image/closure/prototype4.png) 
 **obj.__proto__和Object.prototype的属性一样**

 obj这个对象本质上是被Object函数创建的，因此obj.__proto__=== Object.prototype。我们可以用一个图来表示。
![prototype1](../../../image/closure/prototype5.png) 
 **即，每个对象都有一个__proto__属性，指向创建该对象的函数的prototype**


>那么上图中的“Object prototype”也是一个对象，它的__proto__指向哪里？
 在说明“Object prototype”之前，先说一下自定义函数的prototype。自定义函数的prototype本质上就是和 var obj = {} 是一样的，都是被Object创建，所以它的__proto__指向的就是Object.prototype。
 **但是Object.prototype确实一个特例——它的__proto__指向的是null，切记切记！**
![prototype1](../../../image/closure/prototype6.png)

>函数也是一种对象，函数也有__proto__吗
 又一个好问题！——当然有。

 函数也不是从石头缝里蹦出来的，函数也是被创建出来的。谁创建了函数呢？——Function——注意这个大写的“F”。

 且看如下代码。

 function fn(x,y){
     return x+y;
 }
 console.log(fn(10,20));
 var fn1 = new Function("x","y","return x + y;");
 console.log(fn1(5,6));
 首先根本不推荐用第二种方式。

>根据上面说的一句话——对象的__proto__指向的是创建它的函数的prototype，就会出现：Object.__proto__ === Function.prototype。用一个图来表示。

 **但是Object.prototype确实一个特例——它的__proto__指向的是null，切记切记！**
![prototype1](../../../image/closure/prototype7.png)

 上图中，很明显的标出了：自定义函数Foo.__proto__指向Function.prototype，Object.__proto__指向Function.prototype，唉，怎么还有一个……Function.__proto__指向Function.prototype？这不成了循环引用了？

 对！是一个环形结构。

 其实稍微想一下就明白了。Function也是一个函数，函数是一种对象，也有__proto__属性。既然是函数，那么它一定是被Function创建。所以——Function是被自身创建的。所以它的__proto__指向了自身的Prototype。

 >Function.prototype指向的对象，它的__proto__是不是也指向Object.prototype？
 答案是肯定的。因为Function.prototype指向的对象也是一个普通的被Object创建的对象，所以也遵循基本的规则。
![prototype](../../../image/closure/prototype8.png)

 ### 深入理解javascript原型和闭包（5）——instanceof

>对于值类型，你可以通过typeof判断，string/number/boolean都很清楚，但是typeof在判断到引用类型的时候，返回值只有object/function，你不知道它到底是一个object对象，还是数组，还是new Number等等。

 这个时候就需要用到instanceof。例如：
 function Foo(){}
 var f1 = new Foo();

 console.log(f1 instanceof Foo);//true
 console.log(f1 instanceof Object);//true

 上图中，f1这个对象是被Foo创建，但是“f1 instanceof Object”为什么是true呢？

 至于为什么过会儿再说，先把instanceof判断的规则告诉大家。根据以上代码看下图：

![prototype](../../../image/closure/prototype9.png)

 Instanceof运算符的第一个变量是一个对象，暂时称为A；第二个变量一般是一个函数，暂时称为B

 **Instanceof的判断队则是：沿着A的__proto__这条线来找，同时沿着B的prototype这条线来找，如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。**

 按照以上规则，大家看看“ f1 instanceof Object ”这句代码是不是true？ 根据上图很容易就能看出来，就是true。

 因此：

 console.log(Object instanceof Function) //true
 console.log(Fuction instanceof Object) //true
 console.log(Function instanceof Function) //true

![prototype](../../../image/closure/prototype10.png)


>instanceof这样设计，到底有什么用？
**重点就这样被这位老朋友给引出来了——继承——原型链。**

**即，instanceof表示的就是一种继承关系，或者原型链的结构。**

 ### 深入理解javascript原型和闭包（6）——继承

>javascript中的继承是通过原型链来体现的。先看几句代码
 fuction Foo(){}
 var f1 = new Foo();
 f1.a = 10;
 Foo.prototype.a = 100;
 Foo.prototype.b = 200;
 console.log(f1.a); //10
 console.log(f1.b); //200

 以上代码中，f1是Foo函数new出来的对象，f1.a是f1对象的基本属性，f1.b是怎么来的呢？——从Foo.prototype得来，因为f1.__proto__指向的是Foo.prototype

 **访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着__proto__这条链向上找，这就是原型链。**
![prototype](../../../image/closure/prototype11.png)

 >那么我们在实际应用中如何区分一个属性到底是基本的还是从原型中找到的呢？大家可能都知道答案了——hasOwnProperty，特别是在for…in…循环中，一定要注意。

![prototype](../../../image/closure/prototype12.png)

>   f1的这个hasOwnProperty方法是从哪里来的？ f1本身没有，Foo.prototype中也没有，哪儿来的？

 好问题。

 它是从Object.prototype中来的，请看图

![prototype](../../../image/closure/prototype13.png)

**对象的原型链是沿着__proto__这条线走的，因此在查找f1.hasOwnProperty属性时，就会顺着原型链一直查找到Object.prototype。**

**由于所有的对象的原型链都会找到Object.prototype，因此所有的对象都会有Object.prototype的方法。这就是所谓的“继承”。**

>你可以自定义函数和对象来实现自己的继承。

 说一个函数的例子吧。

 我们都知道每个函数都有call，apply方法，都有length，arguments，caller等属性。为什么每个函数都有？这肯定是“继承”的。函数由Function函数创建，因此继承的Function.prototype中的方法。不信可以请微软的Visual Studio老师给我们验证一下：

![prototype](../../../image/closure/prototype14.png)

 看到了吧，有call、length等这些属性。

 那怎么还有hasOwnProperty呢？——那是Function.prototype继承自Object.prototype的方法。有疑问可以看看上一节将instanceof时候那个大图，看看Function.prototype.__proto__是否指向Object.prototype。

 

 原型、原型链，大家都明白了吗？

 ### 深入理解javascript原型和闭包（7）——原型的灵活性
 >首先，对象属性可以随时改动。

 对象或者函数，刚开始new出来之后，可能啥属性都没有。但是你可以这会儿加一个，过一会儿在加两个，非常灵活。

 在jQuery的源码中，对象被创建时什么属性都没有，都是代码一步一步执行时，一个一个加 上的。

![prototype](../../../image/closure/prototype15.png)

> 其次，如果继承的方法不合适，可以做出修改。
 同理，我也可以自定义一个函数，并自己去修改prototype.toString()方法。
![prototype](../../../image/closure/prototype16.png) 
>最后，如果感觉当前缺少你要用的方法，可以自己去创建
![prototype](../../../image/closure/prototype17.png) 

**如果你要添加内置方法的原型属性，最好做一步判断，如果该属性不存在，则添加。如果本来就存在，就没必要再添加了**

### 深入理解javascript原型和闭包（8）——简述【执行上下文】上
**什么是“执行上下文”**
![prototype](../../../image/closure/prototype18.png)
 第一句报错，a未定义，很正常。第二句、第三句输出都是undefined，说明浏览器在执行console.log(a)时，已经知道了a是undefined，但却不知道a是10（第三句中）。

 >在一段js代码拿过来真正一句一句运行之前，浏览器已经做了一些“准备工作”，其中就包括对变量的声明，而不是赋值。变量赋值是在赋值语句执行的时候进行的。可用下图模拟：

![prototype](../../../image/closure/prototype19.png)

>有js开发经验的朋友应该都知道，你无论在哪个位置获取this，都是有值的。至于this的取值情况，比较复杂，会专门拿出一篇文章来讲解。

 与第一种情况不同的是：第一种情况只是对变量进行声明（并没有赋值），而此种情况直接给this赋值。这也是“准备工作”情况要做的事情之一。
 下面还有。。。第三种情况。
>在第三种情况中，需要注意代码注释中的两个名词——“函数表达式”和“函数声明”。虽然两者都很常用，但是这两者在“准备工作”时，却是两种待遇。
![prototype](../../../image/closure/prototype20.png)
看以上代码。“函数声明”时我们看到了第二种情况的影子，而“函数表达式”时我们看到了第一种情况的影子。

没错。在“准备工作”中，对待函数表达式就像对待“ var a = 10 ”这样的变量一样，只是声明。而对待函数声明时，却把函数整个赋值了。

**总结**
>在“准备工作”中完成了哪些工作：
- 变量、函数表达式——变量声明，默认赋值为undefined；
- this——赋值；
- 函数声明——赋值；
 这三种数据的准备情况我们称之为“执行上下文”或者“执行上下文环境”。

>javascript在执行一个代码段之前，都会进行这些“准备工作”来生成执行上下文。这个“代码段”其实分三种情况——全局代码，函数体，eval代码

- 所谓“代码段”就是一段文本形式的代码。

  <script type="text/javascript">
   //代码段
  </script>

-   其次，eval代码接收的也是一段文本形式的代码。

 eval("alert(123)");

- 最后，函数体是代码段是因为函数在创建时，本质上是 new Function(…) 得来的，其中需要传入一个文本形式的参数作为函数体。
![prototype](../../../image/closure/prototype21.png) 

### 深入理解javascript原型和闭包（9）——简述【执行上下文】下
>上一篇我们讲到在全局环境下的代码段中，执行上下文环境中有如何数据：

- 变量、函数表达式——变量声明，默认赋值为undefined；
- this——赋值；
- 函数声明——赋值；

>如果在函数中，除了以上数据之外，还会有其他数据。先看以下代码：
![prototype](../../../image/closure/prototype22.png)
以上代码展示了在函数体的语句执行之前，arguments变量和函数的参数都已经被赋值。从这里可以看出，函数每被调用一次，都会产生一个新的执行上下文环境。因为不同的调用可能就会有不同的参数。

>另外一点不同在于，函数在定义的时候（不是调用的时候），就已经确定了函数体内部自由变量的作用域。至于“自由变量”和“作用域”是后面要专门拿出来讲述的重点，这里就先点到为止。用一个例子说明一下：
![prototype](../../../image/closure/prototype23.png)
**f() --> fn()**
**函数创建时候已经确定了要打印的fn()的值，函数声明**

>好了，总结完了函数的附加内容，我们就此要全面总结一下上下文环境的数据内容。
全局代码的上下文环境数据内容为：
![prototype](../../../image/closure/prototype24.png)

**给执行上下文环境下一个通俗的定义——在执行代码之前，把将要用到的所有的变量都事先拿出来，有的直接赋值了，有的先用undefined占个空。**

### 深入理解javascript原型和闭包（10）——this

**其实，this的取值，分四种情况。我们来挨个看一下。**
- 在函数中this到底取何值，是在函数真正被调用执行的时候确定的，函数定义的时候确定不了。因为this的取值是执行上下文环境的一部分，每次调用函数，都会产生一个新的执行上下文环境。

>情况1：构造函数
- 所谓构造函数就是用来new对象的函数。其实严格来说，所有的函数都可以new一个对象，但是有些函数的定义是为了new一个对象，而有些函数则不是。另外注意，构造函数的函数名第一个字母大写（规则约定）。例如：Object、Array、Function等。

![prototype](../../../image/closure/prototype25.png)

 以上代码中，如果函数作为构造函数用，那么其中的this就代表它即将new出来的对象。

 注意，以上仅限new Foo()的情况，即Foo函数作为构造函数的情况。如果直接调用Foo函数，而不是new Foo()，情况就大不一样了。
![prototype](../../../image/closure/prototype26.png)

>情况2：函数作为对象的一个属性
- 如果函数作为对象的一个属性时，并且作为对象的一个属性被调用时，函数中的this指向该对象
![prototype](../../../image/closure/prototype27.png)

 以上代码中，fn不仅作为一个对象的一个属性，而且的确是作为对象的一个属性被调用。结果this就是obj对象。
 
 注意，如果fn函数不作为obj的一个属性被调用，会是什么结果呢？
![prototype](../../../image/closure/prototype28.png) 
如上代码，如果fn函数被赋值到了另一个变量中，并没有作为obj的一个属性被调用，那么this的值就是window，this.x为undefined

>情况3：函数用call或者apply调用
- 当一个函数被call和apply调用时，this的值就取传入的对象的值。至于call和apply如何使用，不会的朋友可以去查查其他资料，本系列教程不做讲解。
![prototype](../../../image/closure/prototype29.png)

>情况4：全局 & 调用普通函数

 在全局环境下，this永远是window，这个应该没有非议。
 console.log(this ===window); //true

 普通函数在调用时，其中的this也都是window。

 var x = 10;
 var fn = function(){
     console.log(this); //window 
     console.log(this.x);//10
 }
 fn();
不过下面的情况你需要注意一下：
var obj = {
    x:10,
    fn:function(){
         function f(){
             console.log(this);//window
             console.log(this.x);//undefined
         }
         f();
    }
}
obj.fn();
函数f虽然是在obj.fn内部定义的，但是它仍然是一个普通的函数，this仍然指向window。

>最后jQuery源码
![prototype](../../../image/closure/prototype30.png)

 以上代码是从jQuery中摘除来的部分代码。jQuery.extend和jQuery.fn.extend都指向了同一个函数，但是当执行时，函数中的this是不一样的。

 执行jQuery.extend(…)时，this指向jQuery；执行jQuery.fn.extend(…)时，this指向jQuery.fn。

>this 补充
 原文中this的其中一种情况是构造函数的，要补充的内容是，在构造函数的prototype中，this代表着什么。
![prototype](../../../image/closure/prototype31.png) 
 如上代码，在Fn.prototype.getName函数中，this指向的是f1对象。因此可以通过this.name获取f1.name的值。

 其实，不仅仅是构造函数的prototype，即便是在整个原型链中，this代表的也都是当前对象的值。

### 深入理解javascript原型和闭包（11）——执行上下文栈
>执行全局代码时，会产生一个执行上下文环境，每次调用函数都又会产生执行上下文环境。当函数调用完成时，这个上下文环境以及其中的数据都会被消除，再重新回到全局上下文环境。处于活动状态的执行上下文环境只有一个。

 其实这是一个压栈出栈的过程——执行上下文栈。如下图：
![prototype](../../../image/closure/prototype32.png)  
>可根据以下代码来详细介绍上下文栈的压栈、出栈过程。
![prototype](../../../image/closure/prototype33.png)

如上代码。
![prototype](../../../image/closure/prototype35.png)
![prototype](../../../image/closure/prototype34.png)
>讲到这里，我不得不很遗憾的跟大家说：其实以上我们所演示的是一种比较理想的情况。有一种情况，而且是很常用的一种情况，无法做到这样干净利落的说销毁就销毁。这种情况就是伟大的——闭包。

要说闭包，咱们还得先从自由变量和作用域说起。

### 深入理解javascript原型和闭包（12）——简介【作用域】
>提到作用域，有一句话大家（有js开发经验者）可能比较熟悉：“javascript没有块级作用域”。所谓“块”，就是大括号“｛｝”中间的语句。例如if语句：
![prototype](../../../image/closure/prototype36.png)
>其实，你光知道“javascript没有块级作用域”是完全不够的，你需要知道的是——javascript除了全局作用域之外，只有函数可以创建的作用域。
**所以，我们在声明变量时，全局代码要在代码前端声明，函数中要在函数体一开始就声明好。除了这两个地方，其他地方都不要出现变量声明。而且建议用“单var”形式。**
- jQuery就是一个很好的示例
![prototype](../../../image/closure/prototype37.png)
>下面继续说作用域。作用域是一个很抽象的概念，类似于一个“地盘”
![prototype](../../../image/closure/prototype38.png)
- 如上图，全局代码和fn、bar两个函数都会形成一个作用域。而且，作用域有上下级的关系，上下级关系的确定就看函数是在哪个作用域下创建的。例如，fn作用域下创建了bar函数，那么“fn作用域”就是“bar作用域”的上级。
- 作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。例如以上代码中，三个作用域下都声明了“a”这个变量，但是他们不会有冲突。各自的作用域下，用各自的“a”。

说到这里，咱们又可以拿出jquery源码来讲讲了。

>jQuery源码的最外层是一个自动执行的匿名函数：
![prototype](../../../image/closure/prototype39.png)
- 原因就是在jQuery源码中，声明了大量的变量，这些变量将通过一个函数被限制在一个独立的作用域中，而不会与全局作用域或者其他函数作用域的同名变量产生冲突。全世界的开发者都在用jQuery，如果不这样做，很可能导致jQuery源码中的变量与外部javascript代码中的变量重名，从而产生冲突。

 作用域这块只是很不好解释，咱们就小步快跑，一步一步慢慢展示给大家。

  下一节将把作用域和执行上下文环境结合起来说一说。

 可见，要理解闭包，不是一两句话能说清楚的。。。

 ### 深入理解javascript原型和闭包（13）-【作用域】和【上下文环境】
 >本文把作用域和上下文环境结合起来说一下，会理解的更深一些
![prototype](../../../image/closure/prototype40.png)

**如上图，我们在上文中已经介绍了，除了全局作用域之外，每个函数都会创建自己的作用域，作用域在函数定义时就已经确定了。而不是在函数调用时确定。**

>第一步，在加载程序时，已经确定了全局上下文环境，并随着程序的执行而对变量就行赋值。
![prototype](../../../image/closure/prototype41.png)
>第二步，程序执行到第27行，调用fn(10)，此时生成此次调用fn函数时的上下文环境，压栈，并将此上下文环境设置为活动状态。
![prototype](../../../image/closure/prototype42.png)
>第三步，执行到第23行时，调用bar(100)，生成此次调用的上下文环境，压栈，并设置为活动状态。
![prototype](../../../image/closure/prototype43.png)
>第四步，执行完第23行，bar(100)调用完成。则bar(100)上下文环境被销毁。接着执行第24行，调用bar(200)，则又生成bar(200)的上下文环境，压栈，设置为活动状态
![prototype](../../../image/closure/prototype44.png)
>第五步，执行完第24行，则bar(200)调用结束，其上下文环境被销毁。此时会回到fn(10)上下文环境，变为活动状态。
![prototype](../../../image/closure/prototype45.png)
>第六步，执行完第27行代码，fn(10)执行完成之后，fn(10)上下文环境被销毁，全局上下文环境又回到活动状态。
![prototype](../../../image/closure/prototype46.png)
>最后我们可以把以上这几个图片连接起来看看。
![prototype](../../../image/closure/prototype47.png)
>连接起来看，还是挺有意思的。
- **作用域只是一个“地盘”，一个抽象的概念，其中没有变量。要通过作用域对应的执行上下文环境来获取变量的值。**
- **同一个作用域下，不同的调用会产生不同的执行上下文环境，继而产生不同的变量的值。所以，作用域中变量的值是在执行过程中产生的确定的，而作用域却是在函数创建时就确定了。**
- **所以，如果要查找一个作用域下某个变量的值，就需要找到这个作用域对应的执行上下文环境，再在其中寻找变量的值。**

### 深入理解javascript原型和闭包（14）——从【自由变量】到【作用域链】
>解释一下什么是“自由变量”。

- 在A作用域中使用的变量x，却没有在A作用域中声明（即在其他作用域中声明的），对于A作用域来说，x就是一个自由变量。如下图
![prototype](../../../image/closure/prototype48.png)
- 如上程序中，在调用fn()函数时，函数体中第6行。取b的值就直接可以在fn作用域中取，因为b就是在这里定义的。而取x的值时，就需要到另一个作用域中取。到哪个作用域中取呢？

>有人说过要到父作用域中取，其实有时候这种解释会产生歧义。例如：
![prototype](../../../image/closure/prototype49.png)
- 所以，不要在用以上说法了。相比而言，用这句话描述会更加贴切——**要到创建这个函数的那个作用域中取值——是“创建”**，而不是“调用”，切记切记——其实这就是所谓的“静态作用域”。

对于本文第一段代码，在fn函数中，取自由变量x的值时，要到哪个作用域中取？——要到创建fn函数的那个作用域中取——无论fn函数将在哪里调用。
>上面描述的只是跨一步作用域去寻找。
- 如果跨了一步，还没找到呢？——接着跨！——一直跨到全局作用域为止。要是在全局作用域中都没有找到，那就是真的没有了。
 这个一步一步“跨”的路线，我们称之为——作用域链。

- 我们拿文字总结一下取自由变量时的这个“作用域链”过程：（假设a是自由量）

1. 第一步，现在当前作用域查找a，如果有则获取并结束。如果没有则继续；

2. 第二步，如果当前作用域是全局作用域，则证明a未定义，结束；否则继续；

3. 第三步，（不是全局作用域，那就是函数作用域）将创建该函数的作用域作为当前作用域；

4. 第四步，跳转到第一步。
![prototype](../../../image/closure/prototype50.png)
**以上代码中：第13行，fn()返回的是bar函数，赋值给x。执行x()，即执行bar函数代码。取b的值时，直接在fn作用域取出。取a的值时，试图在fn作用域取，但是取不到，只能转向创建fn的那个作用域中去查找，结果找到了。**

### 深入理解javascript原型和闭包（15）——闭包
> 需要知道应用的两种情况即可——函数作为返回值，函数作为参数传递。
- 第一，函数作为返回值
![prototype](../../../image/closure/prototype51.png)
**如上代码，bar函数作为返回值，赋值给f1变量。执行f1(15)时，用到了fn作用域下的max变量的值。**
- 第二，函数作为参数被传递
![prototype](../../../image/closure/prototype52.png)
**如上代码中，fn函数作为一个参数被传递进入另一个函数，赋值给f参数。执行f(15)时，max变量的取值是10，而不是100。**
- 上一节讲到自由变量跨作用域取值时，曾经强调过：要去创建这个函数的作用域取值，而不是“父作用域”。理解了这一点，以上两端代码中，自由变量如何取值应该比较简单。

>另外，讲到闭包，除了结合着作用域之外，还需要结合着执行上下文栈来说一下。
- 在前面讲执行上下文栈时，我们提到当一个函数被调用完成之后，其执行上下文环境将被销毁，其中的变量也会被同时销毁。

但是在当时那篇文章中留了一个问号——有些情况下，函数调用完成之后，其执行上下文环境不会接着被销毁。这就是需要理解闭包的核心内容。

- 咱们可以拿本文的第一段代码（稍作修改）来分析一下。
![prototype](../../../image/closure/prototype53.png)

1. 第一步，代码执行前生成全局上下文环境，并在执行时对其中的变量进行赋值。此时全局上下文环境是活动状态。
![prototype](../../../image/closure/prototype54.png)

2. 第二步，执行第17行代码时，调用fn()，产生fn()执行上下文环境，压栈，并设置为活动状态。
![prototype](../../../image/closure/prototype55.png)

3. 第三步，执行完第17行，fn()调用完成。按理说应该销毁掉fn()的执行上下文环境，但是这里不能这么做。

**注意，重点来了：因为执行fn()时，返回的是一个函数。函数的特别之处在于可以创建一个独立的作用域。而正巧合的是，返回的这个函数体中，还有一个自由变量max要引用fn作用域下的fn()上下文环境中的max。因此，这个max不能被销毁，销毁了之后bar函数中的max就找不到值了。**

- 因此，这里的fn()上下文环境不能被销毁，还依然存在与执行上下文栈中。
即，执行到第18行时，全局上下文环境将变为活动状态，但是fn()上下文环境依然会在执行上下文栈中。另外，执行完第18行，全局上下文环境中的max被赋值为100。如下图：
![prototype](../../../image/closure/prototype56.png)

4. 第四步，执行到第20行，执行f1(15)，即执行bar(15)，创建bar(15)上下文环境，并将其设置为活动状态。
![prototype](../../../image/closure/prototype57.png)

-执行bar(15)时，max是自由变量，需要向创建bar函数的作用域中查找，找到了max的值为10。这个过程在作用域链一节已经讲过。

 这里的重点就在于，创建bar函数是在执行fn()时创建的。fn()早就执行结束了，但是fn()执行上下文环境还存在与栈中，因此bar(15)时，max可以查找到。如果fn()上下文环境销毁了，那么max就找不到了。

   使用闭包会增加内容开销，现在很明显了吧！
   
5. 第五步，执行完20行就是上下文环境的销毁过程，这里就不再赘述了。

- 闭包和作用域、上下文环境有着密不可分的关系，真的是“想说爱你不容易”！

- 另外，闭包在jQuery中的应用非常多，在这里就不一一举例子了。所以，无论你是想了解一个经典的框架/类库，还是想自己开发一个插件或者类库，像闭包、原型这些基本的理论，是一定要知道的。否则，到时候出了BUG你都不知道为什么，因为这些BUG可能完全在你的知识范围之外。

### 深入理解javascript原型和闭包（18）——补充：上下文环境和作用域的关系
- 作用域和上下文环境绝对不是一回事儿。再说明之前，咱们先用简单的语言来概括一下这两个的区别。
1. 上下文环境：
 可以理解为一个看不见摸不着的对象（有若干个属性），虽然看不见摸不着，但确实实实在在存在的，因为所有的变量都在里面存储着，要不然咱们定义的变量在哪里存？

 另外，对于函数来说，上下文环境是在调用时创建的，这个很好理解。拿参数做例子，你不调用函数，我哪儿知道你要给我传什么 参数？
2. 01 作用域:
>首先，它很抽象。第二，记住一句话：除了全局作用域，只有函数才能创建作用域。创建一个函数就创建了一个作用域，无论你调用不调用，函数只要创建了，它就有独立的作用域，就有自己的一个“地盘”。

3. 02 两者：
一个作用域下可能包含若干个上下文环境。有可能从来没有过上下文环境（函数从来就没有被调用过）；有可能有过，现在函数被调用完毕后，上下文环境被销毁了；有可能同时存在一个或多个（闭包）。

4. 且看下面的例子。
- 第一，除了全局作用域外，每个函数都要创建一个作用域。作用域之间的变量是相互独立的。因此，全局作用域中的x和fn作用域中的x，两者毫无关系，互不影响，和平相处。
![prototype](../../../image/closure/prototype58.png)

- 第二，程序执行之前，会生成全局上下文环境，并在程序执行时，对其中的变量赋值。
![prototype](../../../image/closure/prototype59.png)

- 第三，程序执行到第17行，调用fn(5)，会产生fn(5)的上下文环境，并压栈，并设置为活动状态。
![prototype](../../../image/closure/prototype60.png)

- 第四，执行完第17行，fn(5)的返回值赋值给了f1。此时执行上下文环境又重新回到全局，但是fn(5)的上下文环境不能就此销毁，因为其中有闭包的引用（可翻看前面文章，此处不再赘述）。
![prototype](../../../image/closure/prototype61.png)
- 第五，继续执行第18行，再次调用fn函数——fn(10)。产生fn(5)的上下文环境，并压栈，并设置为活动状态。但是此时fn(5)的上下文环境还在内存中——一个作用域下同时存在两个上下文环境。
![prototype](../../../image/closure/prototype62.png)


