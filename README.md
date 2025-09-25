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
  {ok, Data} -> // todo
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
### Package Manager (包管理器)
The Kx language is capable of supporting very large projects. To manage different third-party packages, a package management mechanism is introduced, with a syntax that emphasizes consistency. It looks roughly like this:

kx语言能够支持超大型的项目，为了管理不同的第三方包，引入包管理机制，语法上注重一致性，大致如下：
```python
import {google.kx}/net/tcp as google_tcp
import {twitter.kx}/net/tcp as twitter_tcp
import {name.kx@1}/net/udp as udp
import {name.kx@2}/util/list as list
```
Why are there several different forms of imports? The main reason is to meet the needs of large projects. The name inside the curly braces after `import` represents a package name, which must be unique within the same repository. For example, all package names must be unique on **a.com**. However, since package repositories may be maintained by different companies or organizations, it is impractical to ensure global uniqueness of package names across all repositories. There may be multiple packages with the same name in different repositories. for instance, there might also be a `name.kx` on **b.com**. In such cases, the `@id` method is used to import packages with the same name from different repositories, thereby resolving naming conflicts!

为什么有这么几种不同的导入形式呢？主要的原因是基于考虑到要支持大项目的需要，`import`后的大括号里的名称表示一个包名，这个包名在同一个仓库下，必须唯一，比如在**a.com**中所有的包名唯一。但是包的仓库可能由不同的公司和组织来维护，要做到所有包名实现跨仓库的唯一是不切实际的，当在不同的仓库中存在多个名字一样的包，比如在**b.com**中也有一个`name.kx` 这时候，需要用到`@id`的方式，来导入不同仓库的相同的包名，这就解决了命名冲突！

In addition to package name conflicts, module names within different packages under the same repository may also be duplicated. This is illustrated in the first two lines of the import statements above: both the kx packages from Google and Twitter contain a module named `tcp`. The solution is simple—just rename it after `as`, which resolves the naming conflict for module names!

除了包名的冲突外，在同一个仓库下，不同的包的模块名，也可能是重名的，这时候，就是上面的包导入语句的前两行出现的情况，在google和twitter两家公司的kx包中，都有一个重名模块`tcp`，那么解决方案很简单，直接在`as`后，重命名就行了，也就解决了模块名的名字冲突！

// todo
