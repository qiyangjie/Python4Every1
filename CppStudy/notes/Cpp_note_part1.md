# Cpp language part 1

## 01. Class Basic

A typical Class defination is shown below.

```cpp
class CRectangle
{
public:
    // Member variables
    // instance variables
    int w, h;
    // Member functions
    void Init(int w_, int h_)
    {
        w = w_;
        h = h_;
    }

    int Area()
    {
        return w * h;
    }

    int Perimeter()
    {
        return 2 * (w + h);
    }
};
```

We can instantiate a class by

```cpp
#include <iostream>

#include "CRectangle.h"

int main()
{
    std::cout << "Hello Class!\n";

    int w, h;
    // r is an instance
    CRectangle r;
    std::cin >> w >> h;
    r.Init(w, h);
    std::cout << r.Area() << std::endl << r.Perimeter();
    return 0;
}
```

The cache size of a instance can be calculated as
$ size\ of\ Object = \sum size\ of \ all\ member\  variables$

The value of _member_ or _instance_ variables of one instance changes will not have effct on other instances.

### Fix Me: member or instance above

The method visit member variables and functions of a class.

```cpp
//Method 1: instanceName.memberName
CRectangle r1,r2;
r1.w=5;
r2.Init(3,4)

//Method 2: point -> memberName
CRectangle *p1 = & r1;
CRectangle *p2 = & r2;
p1->w=5;
p2->Init(3,4);

//Mehtod 3: ReferenceName.member
CRectangle &rr = r2;
rr.2 = 5;
rr.Init(3,4) // is rr and r2 in same address?
```

Another way to print,

### Fix me: Is here &r used as reference?

```cpp
void PrintRectangle(CRectangle &r)
{
    cout<< r.Area() <<","<< r.Perimeter();
}
```

### Fix Me: is rr and r2 in same address

Another Method to write class.

### Fix Me: what is the difference

```cpp
class CRectangle
{
    public:
        int w,h;
        int Area();
        int Perimeter();
        void Init(int w_, int h_);
}

int CRectangle::Area()
{
    return w*h;
}

int CRectangle::Perimeter()
{
    return 2*(w+h);
}

void CRectangle::Init(int w_, int h_)
{
    w=w_;
    h=h_;
}
```

## 02. Accesse range of class member

- private: only access by member function, if not specified, consider as private member.
- public: aceessable for everywhere
- protected : Discuss later

```cpp
class Man
{
    // Default is private member
    int nAge;
    char szName[20];
public:
    void SetName(char *Name)
    {
        strcpy(szName,Name);
    }
}
```

- Member function of class can access:
  - All variables and functions of current instance
  - All variables and functions of other instance in same class
- Others can only access public member of class

### Fix Me : Make sure the above is right

```cpp
class CEmployee
{
    private:
        char szName[30];
    public:
        int salary;
        void setName(char *name);
        void getName(char *name);
        void averageSalary(CEmployee e1, CEmployee e2);
};

void CEmployee::setName(char *name)
{
    strcpy(szName,name);
}

void CEmployee::getName(char *name)
{
    strcpy(name,szName);
}

void CEmployee::averageSalary(CEmployee e1, CEmployee e2)
{
    salary = (e1.salary + e2.salary) /2;
}

int main()
{
    CEmployee e;
    strcpy(e.szName,"Tom1234567889");//cmplie wrong
    e.setName("Tom");
    e.salary =5000;
    return 0;
}
```

Why we need setup private, just like we use `szName[30]` in above code, if it is not private, when we need to change it to `szName[5]`, we need to find all the code like:

```cpp
strcpy(man1.szName,"Tom1234567889")
```

However, if we set it as private, and we only change the defination in the class, as from outside, the variable is accesses by member function like

```cpp
man1.setName("Tom1234567889")
```

## 3. Inline function, overloading function, and default parameters

### 3.1 Inline function

First, we should understand __inline function__. As we know, there is some cost for call a function, if a simple function called a lot of function, the cost is high. So cpp defined inline function to reduce the cost. It insert the entity of function in where call this function.

```cpp
inline int Max(int a, int b)
{
    if(a>b) return a;
    return b;
}
```

We can define a member function as inline function. There are two ways to define a inline member function.

- The first is using keyword __inline__.
- The second method is put the entity of the function inside the defination of class.

```cpp
class B
{
    // method 1
    inline void func1();
    // method 2
    void function2()
    {

    };
}

void B::func1(){};
```

### 3.2 Overloading function

If several function have same name, but different number of parameter or different parameter types. This is called __overloading__.

The following functions are overloading.

```cpp
int Max(double f1, double f2){};
int Max(int n1, int n2){};
int Max(int n1, int n2, int n3){};
```

### 3.3 Default parameters

In cpp, when you define a function, you can give default value for several continous parameters begin the rightest one.

```cpp
void func(int x1, int x2 = 2, int x3=3){};

func(10); // func(10,2,3)
func(10,8)// func(10,8,3)
func(10,,8)// not work
```

The default parameters are useful when you need to add new parameters to an old function, in this method, you don't need to modify everywhere you call this function.

## 4. Constructor -- Member function

- Function name same as the class name
- Can have parameters, but can not have return value
- Initialize the object (member variables)
- If not defined, automatic generate a constructor withour any parameters
- Auto called when generate an object, cannot executed after the object generated
- Multiple constructor is allowed for one class

```cpp
class Complex
{
    private:
        double real,imag;
    public:
        Complex(double r, double i =0);

};

Complex::Complex(double r, double i)
{
    real =r;
    imag =i;
}

Complex c1; // error
Complex *pc = new Complex;// error
Complex c1(2);//c1(2,0)
Complex c1(2,4),C2(3,5)
Complex *pc = new Complex(3,4);
```

- Multiple constructor situation

```cpp
class Complex
{
    private:
        double real,imag;
    public:
        void Set(double r, double i);
        Complex(double r, double i);
        Complex(double r);
        Complex(Complex c1, Complex c2);
};

Complex::Complex(double r, double i)
{
    real =r;
    imag =i;
}

Complex::Complex(double r)
{
    real =r;
    imag = 0;
}

Complex::Complex(Complex c1, Complex c2);
{
    real = c1.real+c2.real;
    imag = c1.imag+c2.imag;
}

Complex c1(3),c2(1,0), C3(c1,c2);
```

- Constuctor would __public__
- Private constructor cannot be used to intialize object directly
- Constructor in data array

```cpp
class CSample
{
        int x;
    public:
        CSample()
        {
            cout<<"Constructor 1 Called"<<endl;
        }
        CSample(int n)
        {
            x=n;
            cout<<"Constructor 2 Called"<<endl;
        }
};

int main()
{
    CSample array1[2];
    cout<<"step1"<<endl;
    CSample array2[2]={4,5};
    cout<<"step2"<<endl;
    CSample array3[2]={3};
    cout<<"step3"<<endl;
    CSample *array4= new CSample[2];
    delete []array4;
    return 0
}
```

The output is as following

- Constructor 1 Called
- Constructor 1 Called
- step1
- Constructor 2 Called
- Constructor 2 Called
- step2
- Constructor 2 Called
- Constructor 1 Called
- step3
- Constructor 1 Called
- Constructor 1 Called

Another example for using data array

```cpp
class Test
{
    public:
        Test(int n) {}//(1)
        Test(int n, int m){}//(2)
        Test(){}//(3)
}

Test array1[3]={1,Test(1,2)};//(1),(2),(3)
Test array2[3]={Test(2,3),Test(1,2),1}//(2),(2),(1)
Test *pArray[3]={new Test(4),new Test(1,2)};
//(1),(2), the third object will not generate as pArray[2] is a point
```

## 5. Copy Constructor

- Only one parameter: object for same class
- `X::X( X& )` or `X::X(const X &)`, the latter one can use const object as parameter

```cpp
class Complex
{
    public:
        double real,imag;
    Complex(){}
    Complex(const Complex & c)
    {
        real = c.real;
        image = c.imag;
        cout << "copy constructor called";
    }
};
Complex c1;
Complex c2(c1)
```

- If copy constructor function not defined, the complier can generate default copy constructor.

```cpp
class Complex
{
    private:
        double real,imag;
};
Complex c1;
Complex c2(c1); // create c2 and initialize as c1
```

- Not allowed constructor like `X::X(X)`

```cpp
class CSample
{
    CSample(CSample c){} // Error
}
```

The situation copy constructor take advantage

1. Use one object to initialize another object in same class

```cpp
Complex c2(c1);

Complex c2 = c1; // Notice here is initialize statement
```

2. If there is a function's parameter is object of class A, so when the function is callled, the copy constructor of class A will be called.

```cpp
class A
{
    public:
        A(){};
        A(A & a)
        {
            cout<< "Copy constructor called"<<endl;
        }// THis will be called
};

void Func(A a1){}

int main()
{
    A a2;
    Func(a2);
    return 0;
}
```

The output is "Copy constructor called".

3. If the return value of a function is object of class A, the copy constructor is called when function return.

```cpp
class A
{
    public:
        int v;
        A(int n)
        {
            v = n;
        }
        A(const A & a)
        {
            v = a.v;
            cout << "Copy constructor called"<<endl;
        }
};

A Func()
{
    A b(4);
    return b;
}

int main()
{
    cout << Func().v<<endl;
    return 0;
}
```
The output:
_Copy constructor called_
_4_

## 6. Type conveter function

- Realize type convert
- Only one parameter
- Not copy constructor
- Complier call type convert constructor and build a temp object/variable

```cpp
class Complex
{
    public:
        double real, imag;
        Complex(int i)
        {
            cout<<"IntConstructor called"<<endl;
            real = i;
            imag = 0;
        }
        Complex(double r, double i)
        {
            real = r;
            imag = i;
        }
};

int main()
{
    Complex c1(7,8);
    Complex c2 = 12 ;
    c1 = 9; // 9 is automatic convert as a temp object of complex
    cout << c1.real << "," << c1.imag << endl;
    return 0;
}
```

The output:
_IntConstructor called_
_IntCOnstructor called_
_9,0_

## 7. Destructor -- member function

### 7.1 Basic format

- Function name same as class name
- Begin with ~
- No parameter and return value
- One destructor one class
- Called when delete object

```cpp
class String
{
    private:
        char *p;
    public:
        String()
        {
            p = new char[10];
        }

        ~ String();
};

String::~String()
{
    delete []p;
}
```

### 7.2 Destructor and delete

- `delete` lead to call destructor

```cpp
Ctest *p Test
pTest = new Ctest;  // call constructor
delete pTest;       // call destructor

pTest1 = new Ctest[3]; // call constructor 3 times
delete [] pTest;      // call destructor 3 times
```

### 7.3 Exampel for desturctor

```cpp
class Demo
{
    int id;
    public:
        Demo(int i)
        {
            id = i;
            cout << "id="<<id<<"Constructed"<<endl;
        }
        ~Demo()
        {
            cout<< "id ="<< id <<"Destructed"<<endl;
        }
};

Demo d1(1);

void Func()
{
    static Demo d2(2);
    Demo d3(3);
    cout << "Func" >> endl;
}

int main()
{
    Demo d4(4);
    d4 = 6;
    cout << "main" << endl;
    { Demo d5(5);}
    Func();
    cout << "main ends"<< endl;
    return 0;
}
```

The output:
_id = 1 Constructed_
_id = 4 Constructed_
_id = 6 Constructed_
_id = 6 Destructed_
_main_
_id = 5 Constructed_
_id = 5 Destructed_
_id = 2 Constructed_
_id = 3 Constructed_
_Func_
_id = 3 Destructed_
_main ends_
_id = 6 Destructed_
_id = 2 Destructed_
_id = 1 Destructed_

## 8. Static member variables and functions

- 普通成员变量每个对象有一份，静态成员变量一共只有一份，所有对象共享

```cpp
# sizeof operation do not compute static memmber
class CMycalss
{
    int n;
    static int s;
}
# sizeof(CMyclass) equal to 4
```

- 普通成员函数必须具体作用于某个对象，静态成员函数并不具体作用于某个对象
- 静态成员不需要通过对象就能访问

```cpp
class CRectangle
{
    private:
        int w,h;
        static int nTotalArea;
        static int nTotalNumber;
    public:
        CRectangle(int w_, int h_);
        ~CRectangle();
        static void PrintTotal();
};

# Method 1
CRectangle::PrintTotal();

# Method 2
CRectangle r;
r.PrintTotal();

# Method 3
CRectangle *p = &r;
p-> PrintTotal()

# Method 4
CRectangle & ref = r;
int n = ref.nTotalNumber;
```

- Example

```cpp
CRectangle::CRectangle(int w_,int h_)
{
    w = w_;
    h = h_;
    nTotal Area += w*h;
}

CRectangle::~CRectangle()
{
    nTotalNumber --;
    nTotalArea -= w*h;
}

void CRectangle::PrintTotal()
{
    cout<<nTotalNumber<<","<<nTotalArea<<endl;
}

# Must define static variable, otherwise cause link error
int CRectangle::nTotalNumber =0;
int CRectangle::nTotalArea = 0;

int main()
{
    CRectangle r1(3,3),r2(2,2);
    CRectangle::PrintTotal();
    r1.PrintTotal();
    return 0;
}
```

- 在静态成员函数中，不能访问非静态成员变量，也不能调用非静态成员函数。

- 上面的类没有考虑到调用复制构造函数的情况，需要特别注意。

```cpp
CRectangle::CRectangle(CRectangle &r)
{
    w=r.w;
    h=r.h;
    nTotalNumber++;
    nTotalArea +=w*h;
}
```

## 9. Enclosing

- 成员对象：一个类的成员变量是另一个类的对象
- 包含成员对象的类叫封闭类

```cpp
class CTyre
{
    private:
        int radius;
        int width;
    public:
        CTyre(int r, int w):radius(r), width(w){}
};

class CEngine{}

class CCar
{
    private:
        int price;
        CTyre tyre;
        CEngine engine;
    public:
        CCar(int p, int tr, int tw);
};

CCar::CCar(int p, int tr, int w):price(p), tyre(tr,w){};

int main()
{
    CCar car(20000,17,225);
    return 0;
}

```

## 10. Friend

- 一个类的友元可以访问该类的私有成员

```cpp
class CCar;

class CDriver
{
    public:
        void ModifyCar(CCar *pCar);
};

class CCar
{
    private:
        int price;
    friend int MostExpensiveCar(CCar cars[], int total);
    friend void CDriver::ModifyCar(CCar *pCar);
}

void CDriver::ModifyCar(CCar *pCar)
{
    pCar->price += 1000;
}
int MostExpensiveCar(CCar cars[], int total)
{
    int tmpMax = -1;
    for(int i=0; i < total;++i)
        if(car[i].price > tmpMax)
            tmpMax = cars[i].price;
    return tmpMax;
}

int main()
{
    return 0;
}
```

- 将一个类的成员函数作为另一个类的友元

- A是B的友元类，那么A的成员函数可以访问B的私有成员

```cpp
class CCar
{
    private:
        int price;
    friend class CDriver;
}

class CDriver
{
    public:
        CCar myCar;
        void ModifyCar()
        {
            myCar.price += 1000;
        }
}

int main()
{
    return 0;
}
```

- Notice: 友元之间的关系不能传递，不能继承

## 11. This pointer

- C++ and C translate

```cpp
class CCar
{
    public:
        int price;
        void SetPrice(int p);
};

void CCar::SetPrice(int p)
{
    price = p;
}

int main()
{
    CCar car;
    car.SetPrice(20000);
    return 0;
}
```

```c
struct CCar
{
    int price;
};

void SetPrice(struct CCar *this, int p)
{
    this->price = p;
}

int main()
{
    struct CCar car;
    SetPrice(& car, 20000);
    return 0;
}
```

- this 指针的作用就是指向成员函数所作用的对象

- 非静态成员函数中可以直接使用this来代表指向该函数作用的对象的指针

```cpp
class Complex
{
    public:
        double real, imag;
        void Print()
        {
            cout<<real<<","<<imag;
        }
        Complex(double r, double i):real(r),imag(i){}
        Complex AddOne()
        {
            this->real++;// equal to real++
            this->Print();//equal to Print
            return *this;
        }
};

int main()
{
    Complex c1(1,1),c2(0,0);
    c2 = c1.AddOne();
    return 0;
}
```

- 静态成员函数中不能使用this指针

## 12. Constent object, Member variables, and refrerence

- 如果不希望某个对象的值被改变，则定义该对象的时候可以在前面加const

```cpp
class Demo
{
    private:
        int value;
    public:
        void SetValue(){}
};
const Demo Obj;
```

- 在类的成员函数说明后面可以加const，则该成员函数成为常量成员函数。
- 常量成员函数执行期间
-- 不应该修改其所作用的对象
-- 不应该修改成员变量的值（静态成员变量除外）
-- 不能调用同类的非常量成员函数（静态成员函数除外）

```cpp
class Sample
{
    public:
        int value;
        void GetValue() const;
        void func(){};
        Sample(){};
};

void Sample:GetValue() const
{
    value = 0; //wrong, 常量成员函数执行期间不应该修改成员变量的值
    func();//wrong, 常量成员函数执行期间不能调用同类的非常量成员函数
}

int main()
{
    const Sample o;
    o.value = 100 //wrong, 常量对象不可被修改
    o.func(); // wrong, 常量对象上面不能执行非常量成员函数
    o.GetValue();// OK
    return 0;
}
```

- 两个成员函数，名字和参数表都一样，但是一个const，一个不是，算重载。

- 引用前面可以加const关键字，成为常引用。不能通过常引用，修改其引用的变量。

```cpp
const int &r = n;
r = 5; // wrong
n = 4; //OK
```

- 对象作为函数的参数时，生成该参数需要调用复制构 造函数，效率比较低。用指针作参数，代码又不好看， 如何解决？
-- 用对象的常引用作为参数

```cpp
class Sample{};

void PrintfObj( const Sample & o){}
```

这样函数中就能确保不会出现无意中更改o的值了。
