# AStyle说明

一个免费，高速，小巧的自动格式化工具，支持C，C ++，C ++ / CLI，Objective-C，C＃和Java源代码。

下面是官方的说明文档：  
[Artistic Style 3.3官网](https://astyle.sourceforge.net/astyle.html)

下面的参数，是我推荐的一种方案：  
--style=allman --indent=force-tab --attach-inlines --attach-extern-c --attach-closing-while --pad-oper --pad-comma --pad-header --align-pointer=type --suffix=none 

## braces 括号风格

### --style=allman / --style=bsd / --style=break / -A1

Allman style uses broken braces.  

```c++
int Foo(bool isBar)
{
    if (isBar)
    {
        bar();
        return 1;
    }
    else
        return 0;
}
```

### --style=java / --style=attach / -A2

Java style uses attached braces.  

```c++
int Foo(bool isBar) {
    if (isBar) {
        bar();
        return 1;
    } else
        return 0;
}
```

### --style=kr / --style=k&r / --style=k/r / -A3

Kernighan & Ritchie style uses linux braces. Opening braces are broken from namespaces, classes, and function definitions. The braces are attached to everything else, including arrays, structs, enums, and statements within a function.  

```c++
int Foo(bool isBar)
{
    if (isBar) {
        bar();
        return 1;
    } else
        return 0;
}
```

## 其他选项

### --indent=force-tab / --indent=force-tab=# / -T / -T#

Indent using all tab characters, if possible. If a continuation line is not an even number of tabs, spaces will be added at the end. Treat each tab as # spaces (e.g. -T6 / --indent=force-tab=6). # must be between 2 and 20. If no # is set, treats tabs as 4 spaces.

with indent=force-tab:

```c++
void Foo() {
>   if (isBar1
>   >   >   && isBar2)    // indent of this line can be changed with min-conditional-indent
>   >   bar();
}
```

### --attach-inlines / -xl

Attach braces to class and struct inline function definitions. This option has precedence for all styles except Horstmann and Pico (run-in styles). It is effective for C++ files only.

all braces are attached to class and struct inline method definitions:

```c++
class FooClass
{
    void Foo() {
    ...
    }
};
```

### --attach-extern-c / -xk
Attach braces to a braced extern "C" statement. This is done regardless of the brace style being used. This option is effective for C++ files only.

An extern "C" statement that is part of a function definition is formatted according to the requested brace style. Braced extern "C" statements are unaffected by the brace style and this option is the only way to change them.

this option attaches braces to a braced extern "C" statement:

```c++
#ifdef __cplusplus
extern "C" {
#endif
but function definitions are formatted according to the requested brace style:

extern "C" EXPORT void STDCALL Foo()
{}
```

### --attach-closing-while / -xV

Attach the closing 'while' of a 'do-while' statement to the closing brace. This has precedence over both the brace style and the break closing braces option.

```c++
do
{
    bar();
    ++x;
}
while x == 1;

becomes:

do
{
    bar();
    ++x;
} while x == 1;
```

### --pad-oper / -p

Insert space padding around operators. This will also pad commas. Any end of line comments will remain in the original column, if possible. Note that there is no option to unpad. Once padded, they stay padded.

```c++
if (foo==2)
    a=bar((b-c)*a,d--);

becomes:

if (foo == 2)
    a = bar((b - c) * a, d--);
```

### --pad-comma / -xg

Insert space padding after commas. This is not needed if pad-oper is used. Any end of line comments will remain in the original column, if possible. Note that there is no option to unpad. Once padded, they stay padded.

```c++
if (isFoo(a,b))
    bar(a,b);

becomes:

if (isFoo(a, b))
    bar(a, b);
```

### --pad-header / -H

Insert space padding between a header (e.g. 'if', 'for', 'while'...) and the following paren. Any end of line comments will remain in the original column, if possible. This can be used with unpad-paren to remove unwanted spaces.

```c++
if(isFoo((a+2), b))
    bar(a, b);

becomes:

if (isFoo((a+2), b))
    bar(a, b);
```

### --align-pointer=type   / -k1 
### --align-pointer=middle / -k2
### --align-pointer=name   / -k3

Attach a pointer or reference operator (*, &, or ^) to either the variable type (left) or variable name (right), or place it between the type and name (middle). The spacing between the type and name will be preserved, if possible. This option is for C/C++, C++/CLI, and C# files. To format references separately, use the following align-reference option.

```c++
char* foo1;
char & foo2;
string ^s1;
becomes (with align-pointer=type):

char* foo1;
char& foo2;
string^ s1;
```

```c++
char* foo1;
char & foo2;
string ^s1;
becomes (with align-pointer=middle):

char * foo1;
char & foo2;
string ^ s1;
```
```c++
char* foo1;
char & foo2;
string ^s1;
becomes (with align-pointer=name):

char *foo1;
char &foo2;
string ^s1;
```

### --align-reference=none   / -W0
### --align-reference=type   / -W1
### --align-reference=middle / -W2
### --align-reference=name   / -W3

This option will align references separate from pointers. Pointers are not changed by this option. If pointers and references are to be aligned the same, use the previous align-pointer option. The option align-reference=none will not change the reference alignment. The other options are the same as for align-pointer. This option is for C/C++, C++/CLI, and C# files.

```c++
char &foo1;
becomes (with align-reference=type):

char& foo1;
```
```c++
char& foo2;
becomes (with align-reference=middle):

char & foo2;
```
```c++
char& foo3;
becomes (with align-reference=name):

char &foo3;
```
 
### --suffix=none / -n

Do not retain a backup of the original file. The original file is purged after it is formatted.

### --recursive / -r / -R

For each directory in the command line, process all subdirectories recursively. When using the recursive option the file name statement should contain a wildcard. Linux users should place the file path and name in double quotes so the shell will not resolve the wildcards (e.g. "$HOME/src/*.cpp"). Windows users should place the file path and name in double quotes if the path or name contains spaces.

