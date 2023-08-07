# GiraKoo速查手册

本页面用于记录编译器编译时，可能遇到的错误。已经对应的解决方案。  
> 如果有更好的解决方案，欢迎通过girakoo的163邮箱进行沟通。或者在https://github.com/GiraKoo/help_wiki进行反馈。  
> 当前本人最常使用的编译器是MSVS，Clang++，GCC。  
> 沟通可使用中文，English。  

## 基本内容

!!! note "xxxxxxx"

    - 原因：
    - 解决方案：

    **备注：**  

    -

## 编译警告和错误

本章节记录在编译器中遇到的warning提示。

!!! note "warning: 'xxxxx' macro redefined"

    - 原因：宏xxxxx在不同的地方，被定义为不同的内容。
    - 解决方案：根据提示信息，确认不同定义的位置。回避引用，或者舍弃一处宏定义。

    **备注：**  

    - 在编写代码时，由于不同的人开发不同的模块，或者是引用了外部的库，源码等情况。同一个名称被多次宏定义的情况，是很正常的情况。
    - 良好准确的进行宏名称定义，避免使用过于通用，简短的定义。
    - 如果由于跨平台适配的需要，某些宏只能在特定平台使用。可以通过追加ifdef宏判定的方式，只在没有定义该宏的情况下，进行使用。
    ```c++
    #ifndef XXXXX
    #define XXXXX
    #endif
    ```

!!! note "warning: unknown pragma ignored"

    - 原因：#pragma在不同的平台，支持的规则不同。
    - 解决方案：利用平台宏进行限制。例如使用VS专用的版本宏，确定当前是否使用的是VS编译器。

    **备注：**  

    - 可以使用的宏值进行屏蔽。[常用宏值整理](pages/macros.md)
    - 例如：
    ```c++
    #ifdef _MSC_VER
    #pragma warning(disable: 4996)
    #endif
    ```
    该宏用于禁用4996号警告。4996号警告是VS编译器的警告。在其他编译器中，可能没有该警告。因此，需要进行屏蔽。 

!!! note "warning: typedef requires a name"

    - 原因：typedef重定义struct或enum时，缺少提供名称。
    - 解决方案：
    ```c++
    typedef struct XXXX {} XXXX; // 在括号}和;之间追加名称XXXX
    ``` 

    **备注：**  

    - typedef的标准语法是typedef A B;将A重命名为B。在C语言中，由于语法限制。使用struct A， enum B时，不能省略类型。例如：  
    ```c++
    struct A {};

    struct A a; // 正确使用  
    A a1; // 会引起编译错误  
    ```    
    不仅使用不便，还会与C++/java等语言的语法有所区别。所以会利用typedef struct C{} C;的方式。在定义struct C的同时，也起一个别名，叫做C。此时再使用C c;就不会出现编译错误。
    ```c++
    typedef struct C {} C;

    C c; // 正确使用  
    ```    
!!! note "warning: 'xxxxx' overrides a member function but is not marked 'override'"

    - 原因：重载基类的virtual函数没有在子类中标明override
    - 解决方案：
    ```c++
    class Base {
    public:
        virtual void func() {}
    };
    
    class Derived : public Base {
    public:
        virtual void func() override {} // 在子类中标明override
    };
    ``` 
    **备注：**  
    
    - override关键字是在C++11之后引入的关键字。
    - 使用override关键字，可以在编译期间，检查是否正确重载了基类的virtual函数。对于提高代码的可读性，可维护性，有很大的帮助。