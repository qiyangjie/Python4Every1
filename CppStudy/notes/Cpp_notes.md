# Cpp Language Learning Notes

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