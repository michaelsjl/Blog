# c++ 学习随笔

## 1. sizeof和strlen的区别
参考 [sizeof和strlen的区别及使用详解](https://blog.csdn.net/magic_world_wow/article/details/80500473)

## 2. c++陌生的关键字：  

### 标准转换运算符：  
`const_cast`  
通俗的说，const_cast转换符可以用来将数据类型中的const属性去除。  
例子：  
```
const int constant = 21;
const int* const_p = &constant;
int* modifier = const_cast<int*>(const_p);
*modifier = 7;
```

参考[C++标准转换运算符const_cast](https://www.cnblogs.com/ider/archive/2011/07/22/cpp_cast_operator_part2.html)  

`dynamic_cast`  
dynamic_cast运算符的主要用途：将基类的指针或引用安全地转换成派生类的指针或引用，  
并用派生类的指针或引用调用非虚函数。如果是基类指针或引用调用的是虚函数无需转换就能在运行时调用派生类的虚函数。  
前提条件：当我们将dynamic_cast用于某种类型的指针或引用时，只有该类型至少含有虚函数时(最简单是基类析构函数为虚函数)，才能进行这种转换。否则，编译器会报错。  
参考[C++中深入理解dynamic_cast](https://www.cnblogs.com/chechen/p/11728743.html)  

`reinterpret_cast`  
reinterpret_cast<new_type> (expression)  
reinterpret_cast和static_cast是并列的一种类型转换操作符，它可以将一种类型的指针转换为另一种类型的指针。  
例如，讲`int*`类型的&i转换为`float*`类型:
```
int i = 2;
float *p = reinterpret_cast<float*>(&i);
```

参考[C++标准转换运算符reinterpret_cast](https://www.cnblogs.com/lsgxeva/p/11005293.html)  

`satic_cast`  
static_cast< new_type >(expression)  
static_cast相当于传统的C语言里的强制转换，该运算符把expression转换为new_type类型，用来强迫隐式转换如non-const对象转为const对象，编译时检查，用于非多态的转换，可以转换指针及其他，但没有运行时类型检查来保证转换的安全性。它主要有如下几种用法：  
1. 用于类层次结构中基类（父类）和派生类（子类）之间指针或引用的转换。  
进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；  
进行下行转换（把基类指针或引用转换成派生类表示）时，由于没有动态类型检查，所以是不安全的。  
2. 用于基本数据类型之间的转换，如把int转换成char，把int转换成enum。  
3. 把空指针转换成目标类型的空指针。  
4. 把任何类型的表达式转换成void类型。  

注意：static_cast不能转换掉expression的const、volatile、或者__unaligned属性。  
参考[static_cast和dynamic_cast详解](https://blog.csdn.net/u014624623/article/details/79837849)  

### 其他用的少的
`asm`  
__asm关键字启动内联汇编并且能写在任何c/c++合法语句之处.它不能单独出现.它必须接汇编指令、一组被大括号包含的指令或一对空括号.术语“__asm 块”在这里是任意一个指令或一组指令无论是否在括号内。  

`explicit`  
首先, C++中的explicit关键字只能用于修饰只有一个参数的类构造函数, 它的作用是表明该构造函数是显示的, 而非隐式的, 跟它相对应的另一个关键字是implicit, 意思是隐藏的,类构造函数默认情况下即声明为implicit(隐式).  
参考[C++ explicit关键字详解](https://www.cnblogs.com/rednodel/p/9299251.html)  

`mutable`  
mutalbe的中文意思是“可变的，易变的”，跟constant（既C++中的const）是反义词。  
在C++中，mutable也是为了突破const的限制而设置的。被mutable修饰的变量，将永远处于可变的状态，即使在一个const函数中。  
参考[C++中的mutable关键字](https://www.cnblogs.com/yongdaimi/p/9565996.html)   

`register`   
在早期c语言编译器不会对代码进行优化，因此使用register关键字修饰变量是很好的补充，大大提高的速度。  
register关键字请求让编译器将变量a直接放入寄存器里面，以提高读取速度，在C语言中register关键字修饰的变量不可以被取地址，但是c++中进行了优化。  
参考[浅谈c/c++中register关键字](https://blog.csdn.net/m0_37717595/article/details/79615775)  

`typeid`  
注意：typeid是操作符，不是函数。这点与sizeof类似）  
运行时获知变量类型名称，可以使用 typeid(变量).name()  
需要注意不是所有编译器都输出”int”、”float”等之类的名称，对于这类的编译器可以这样使用  
```
int ia = 3;
if(typeid(ia) == typeid(int))
{
    cout <<"int" <<endl;
}
```

参考[C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821)

`volatile`  
volatile提醒编译器它后面所定义的变量随时都有可能改变，因此编译后的程序每次需要存储或读取这个变量的时候，都会直接从变量地址中读取数据。如果没有volatile关键字，则编译器可能优化读取和存储，可能暂时使用寄存器中的值，如果这个变量由别的程序更新了的话，将出现不一致的现象。  
参考[详解C/C++中volatile关键字](https://blog.csdn.net/weixin_44363885/article/details/92838607)

`wchat_t`  
char 是单字符类型，长度为一个字节  
wchar_t 是宽字符类型，长度为两个字节，主要用在国际 Unicode 编码中  
参考[[C++] wchar_t关键字使用方法](https://www.cnblogs.com/lialong1st/p/12005520.html)  

## 3. new T 与new T()的区别
1. T *p =new T;  
2. T *p =new T();  
这两类用法不同点的总结:    
1. 若T为类类型，且用户定义了构造函数，则两种形式的效果完全相同，都会调用这个定义了的构造函数来初始化内部成员变量，但是如果此构造函数中并未对成员变量初始化，则这个时候内部的成员变量进行默认初始化——值是未定义的。  
2. 若T为类类型，但是用户并没有定义任何构造函数，则我们可以知道编译器会为该类合成一个默认的构造函数，这个时候上述两种形式的结果就不同了，①的类内部的成员变量这个时候执行默认初始化，其值是未定义的。但是在②中就不同了，加了括号后，p内部的成员变量会执行值初始化，即以0的形式进行初始化（整数就为0，bool就为false，string 就为空）  
3. 若T为内置类型，则①的形式中*p的值为未定义的，②中进行值初始化如上。  
