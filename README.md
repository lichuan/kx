# The Kx Programming Language
Kx is a statically typed programming language for concurrency, focusing on **simplicity**, **orthogonality**, **clarity** and **consistency**. It borrows many features and advantages from Erlang and can be considered a statically typed version of Erlang.

Kx是一门面向并发的静态类型的编程语言，注重简洁性，正交性，明确清晰和一致性。它借鉴了很多来自erlang语言的一些特性和优点，可以说kx语言是静态类型版本的erlang语言。

## Design philosophy (设计哲学)
The process of software development is essentially a process of managing complexity. Simplicity and clarity are key principles of the Kx language. Here are two examples to illustrate what kind of design is unclear and ambiguous:

软件开发的过程本质上就是管理复杂性的过程。简洁性与明晰性是kx语言的一个重要的准则。这里举两个例子来说明什么样的设计不是明晰的，是有二义性的：

In C lanauge, when declaring two variables:

在c语言中声明两个变量时：
```c
int* a, b;
```
At first glance, it appears that both a and b are pointers to int, but the actual semantics are that only a is a pointer, while b is an integer.

第一眼看上去，好像a和b两个变量都是int型的指针类型，但是实际上的语义却是只有a是指针，而b只是一个整型int变量。

In Golang, the return value after calling a function is:

在golang语言，调用函数后的返回值形式：
```go
data, err := funcFoo(arg)
```
This syntax is also ambiguous because data and err can have multiple combinations. It's unclear whether data is valid when err is nil, and it's also unclear whether data is valid when err is not nil. From a language design perspective, such combinations are ambiguous. In contrast, the return value design in Erlang is much clearer:

这种形式的语法，也是不明确的，因为data和err可以有多种组合，当err为nil时，data是否一定有效是不明确的，同时当err不为nil时，data又是否一定有效也是不明确的，从语言设计上看，这样的组合都是不明确的。相反在erlang语言中的返回值的设计就清晰了很多：
```erlang
case funcFoo(arg) of
  {ok, Data} -> // todo;
  {error, Reason} -> // todo
end
```
Because in this code, error is not combined with Data, but instead with Reason, it clearly indicates the error and its cause. When Data is valid, it is also clearly combined with the ok atom.

因为这段代码中，error没有与Data组合在一起使用，而是选择和Reason组合，是非常明确的表示错误以及错误的原因。而当Data有效时也是非常明确的选择和ok元子来组合

## Features (特性)
- **Static typing** - Strong type checking during compile time makes the Kx language safer and more robust.
- **Concurrency model** - Lightweight processes, similar to the Erlang process model, make concurrency easier.
- **Immutability** - Variables are assigned once and cannot be changed, eliminating the need for complex locking mechanisms.
- **Functional** - Kx is a functional language, offering greater orthogonality and a clearer logical structure.
- **Pattern matching** - The pattern matching mechanism in the Kx language greatly improves development efficiency.
- **Binary processing** - Powerful binary processing, combined with pattern matching, makes parsing binary data very easy.
- **Fault tolerance** - Allows monitoring and restarting of lightweight processes, eliminating the tedious defensive programming required in languages ​​like Go.
- **Package manager** - A simple and clear package management mechanism allows Kx packages with duplicate names from multiple repositories to be used simultaneously in your codebase.

##
- **静态类型** - 编译时的强类型检查，让kx语言更安全和健壮
- **并发模型** - 轻量级进程，类似于erlang中的process模型，让并发更容易
- **不可变性** - 变量一次性赋值，不可更改，免去复杂的加锁机制
- **函数式** - Kx是函数式语言，更具有正交性的特点和更清晰的逻辑结构
- **模式匹配** - Kx语言中的模式匹配机制，大大提升了开发效率
- **二进制处理** - 强大的二进制处理，搭配模式匹配，让解析二进制数据变成非常容易
- **容错机制** - 允许对轻量级进程的监控与重启，免去了类似golang语言中的那种啰嗦的防御性编程
- **包管理器** - 简洁明晰的包管理机制，让多个仓库中的重复名的不同的kx包也能同时在你的代码库中为你所用

## Kx Language Specification (Kx语言规范)
// todo
