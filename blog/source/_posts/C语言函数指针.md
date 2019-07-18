一个数据变量的内存地址可以存储在相应的指针变量中，函数的首地址也以存储在某个函数指针变量中。这样，我就可以通过这个函数指针变量来调用所指向的函数了。
     在C系列语言中，任何一个变量，总是要先声明，之后才能使用的。函数指针变量也应该要先声明。
     函数指针变量的声明：
     void (*funP)(int) ;   //声明一个指向同样参数、返回值的函数指针变量。
    （整个函数指针变量的声明格式如同函数myFun的声明处一样，只不过——我们把myFun改成（*funP）而已，这样就有了一个能指向myFun函数的指针了。当然，这个funP指针变量也可以指向所有其它具有相同参数及返回值的函数。）
   
...
void (*funP)(int);   //函数指针的声明，也可写成void(*funP)(int x)，但习惯上一般不这样。
void (*funA)(int);
void myFun(int x);   //函数的声明，也可写成：void myFun( int );
int main()
{
    //一般的函数调用
    myFun(100);
    
    //myFun与funP的类型关系类似于int 与int *的关系。
    funP=&myFun;  //将myFun函数的地址赋给funP变量
    (*funP)(200);  //通过函数指针变量来调用函数
    
    //myFun与funA的类型关系类似于int 与int 的关系。
    funA=myFun;
    funA(300);
    
    //三个貌似错乱的调用
    funP(400);
    (*funA)(600);
    (*myFun)(1000);
    return 0;
}

void myFun(int x)
{
    printf("myFun: %d\n",x);
}
...

总结：
1、 其实，myFun的函数名与funP、funA函数指针都是一样的，即都是函数指针。myFun函数名是一个函数指针常量，而funP、funA是函数指针变量，这是它们的关系。
2、但函数名调用如果都得如(*myFun)(10)这样，那书写与读起来都是不方便和不习惯的。所以C语言的设计者们才会设计成又可允许myFun(10)这种形式地调用。
3、 为了统一调用方式，funP函数指针变量也可以funP(10)的形式来调用。
4、赋值时，可以写成funP=&myFun形式，也可以写成funP=myFun。
5、但是在声明时，void myFun(int )不能写成void (*myFun)(int )，void (*funP)(int )不能写成void funP(int )。
6、函数指针变量也可以存入一个数组内。数组的声明方法：int (*fArray[10]) ( int );

...
void (*funP)(int);
void (*funA)(int);
void myFun(int x);
int main()
{
    funP=&myFun;
    printf("myFun\t 0x%p=0x%p\n",&myFun,myFun);
    printf("funP\t 0x%p=0x%p\n",&funP,funP);
    printf("funA\t 0x%p=0x%p\n",&funA,funA);
    return 0;
}

void myFun(int x)
{
    printf("myFun: %d\n",x);
}
...

总结：
1、函数指针常量&myFun = myFun，函数指针变量的值funP等于所指向的函数指针常量。
2、函数指针变量和函数指针常量存储在内存的不同位置。
3、未赋值的函数指针变量（全局）的值为0。

typedef void(*FunType)(int);
//前加一个typedef关键字，这样就定义一个名为FunType函数指针类型，而不是一个FunType变量。
