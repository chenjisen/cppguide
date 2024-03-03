9# Google C++ 风格指南

# Google C++ Style Guide

# 背景

# Background

C++ 是 Google 许多开源项目使用的主要开发语言之一。每个 C++ 程序员都知道，该语言具有许多强大的功能，但这种功能也带来了复杂性，这反过来又会使代码更容易出现错误，并且更难以阅读和维护。
C++ is one of the main development languages used by many of Google's open-source projects. As every C++ programmer knows, the language has many powerful features, but this power brings with it complexity, which in turn can make code more bug-prone and harder to read and maintain.

本指南的目标是通过详细描述编写 C++ 代码的注意事项来管理这种复杂性。这些规则的存在是为了保持代码库的可管理性，同时仍然允许编码人员高效地使用 C++ 语言功能。
The goal of this guide is to manage this complexity by describing in detail the dos and don'ts of writing C++ code . These rules exist to keep the code base manageable while still allowing coders to use C++ language features productively.

_样式_，也称为可读性，是我们所说的管理 C++ 代码的约定。术语 "样式" 有点用词不当，因为这些约定涵盖的不仅仅是源文件格式。
_Style_, also known as readability, is what we call the conventions that govern our C++ code. The term Style is a bit of a misnomer, since these conventions cover far more than just source file formatting.

Google 开发的大多数开源项目都符合本指南中的要求。
Most open-source projects developed by Google conform to the requirements in this guide.

请注意，本指南不是 C++ 教程：我们假设读者熟悉该语言。
Note that this guide is not a C++ tutorial: we assume that the reader is familiar with the language.

## 风格指南的目标

## Goals of the Style Guide

为什么我们有这个文件？
Why do we have this document?

我们认为本指南应该服务于几个核心目标。这些是构成所有单独规则基础的根本*原因*。通过将这些想法带到前台，我们希望能够开展讨论，并使我们更广泛的社区更清楚地了解制定这些规则的原因以及做出特定决定的原因。如果您了解每条规则的服务目标，那么每个人都应该更清楚何时可以放弃某条规则（有些可以放弃），以及需要什么样的论点或替代方案来更改指南中的规则。
There are a few core goals that we believe this guide should serve. These are the fundamental **why**s that underlie all of the individual rules. By bringing these ideas to the fore, we hope to ground discussions and make it clearer to our broader community why the rules are in place and why particular decisions have been made. If you understand what goals each rule is serving, it should be clearer to everyone when a rule may be waived (some can be), and what sort of argument or alternative would be necessary to change a rule in the guide.

我们目前看到的风格指南的目标如下：
The goals of the style guide as we currently see them are as follows:

- 风格规则应该发挥作用
- Style rules should pull their weight

  样式规则的好处必须足够大，才能要求我们所有的工程师记住它。好处是相对于我们在没有规则的情况下获得的代码库来衡量的，因此，如果人们无论如何都不太可能这样做，那么针对非常有害的做法的规则可能仍然会带来很小的好处。这个原则主要解释了我们没有的规则，而不是我们有的规则：例如， `goto` 违反了以下许多原则，但已经很少见，因此样式指南没有讨论它。
  The benefit of a style rule must be large enough to justify asking all of our engineers to remember it. The benefit is measured relative to the codebase we would get without the rule, so a rule against a very harmful practice may still have a small benefit if people are unlikely to do it anyway. This principle mostly explains the rules we don’t have, rather than the rules we do: for example, `goto` contravenes many of the following principles, but is already vanishingly rare, so the Style Guide doesn’t discuss it.

- 针对读者而不是作者进行优化
- Optimize for the reader, not the writer

  我们的代码库（以及提交给它的大多数单独组件）预计将持续相当长的一段时间。因此，阅读大部分代码所花费的时间比编写代码的时间还要多。我们明确选择针对普通软件工程师在代码库中阅读、维护和调试代码的体验进行优化，而不是在编写所述代码时变得轻松。 "为读者留下痕迹" 是该原则的一个特别常见的子点：当一段代码中发生令人惊讶或不寻常的事情时（例如，指针所有权的转移），在该点为读者留下文本提示使用是有价值的（`std::unique_ptr`在调用站点明确地演示了所有权转移）。
  Our codebase (and most individual components submitted to it) is expected to continue for quite some time. As a result, more time will be spent reading most of our code than writing it. We explicitly choose to optimize for the experience of our average software engineer reading, maintaining, and debugging code in our codebase rather than ease when writing said code. "Leave a trace for the reader" is a particularly common sub-point of this principle: When something surprising or unusual is happening in a snippet of code (for example, transfer of pointer ownership), leaving textual hints for the reader at the point of use is valuable (`std::unique_ptr` demonstrates the ownership transfer unambiguously at the call site).

- 与现有代码保持一致
- Be consistent with existing code

  在我们的代码库中一致使用一种风格可以让我们专注于其他（更重要的）问题。一致性还可以实现自动化：只有当您的代码与工具的期望一致时，格式化代码或调整 `#include` 的工具才能正常工作。在许多情况下，归因于 "保持一致" 的规则归结为 "只选择一个，不再担心它" ；在这些问题上允许灵活性的潜在价值被人们争论的成本所抵消。然而，一致性是有限制的；当没有明确的技术论证或长期方向时，这是一个很好的平局打破者。它在局部应用更广泛（每个文件，或一组紧密相关的接口）。一般来说，一致性不应该被用作以旧风格做事的理由，而不考虑新风格的好处，或者代码库随着时间的推移向新风格收敛的趋势。（一致性通常不应该被用作在不考虑新风格的好处的情况下以旧风格做事的理由，或者不考虑代码库随着时间的推移向新风格靠拢的趋势。？）
  Using one style consistently through our codebase lets us focus on other (more important) issues. Consistency also allows for automation: tools that format your code or adjust your `#include`s only work properly when your code is consistent with the expectations of the tooling. In many cases, rules that are attributed to "Be Consistent" boil down to "Just pick one and stop worrying about it"; the potential value of allowing flexibility on these points is outweighed by the cost of having people argue over them. However, there are limits to consistency; it is a good tie breaker when there is no clear technical argument, nor a long-term direction. It applies more heavily locally (per file, or for a tightly-related set of interfaces). Consistency should not generally be used as a justification to do things in an old style without considering the benefits of the new style, or the tendency of the codebase to converge on newer styles over time.

- 适当时与更广泛的 C++ 社区保持一致
- Be consistent with the broader C++ community when appropriate

  与其他组织使用 C++ 的方式保持一致是有价值的，其原因与我们代码库中的一致性相同。如果 C++ 标准中的某个功能解决了某个问题，或者某个惯用语被广泛知晓并接受，那就是使用它的理由。然而，有时标准功能和习惯用法是有缺陷的，或者只是在设计时没有考虑到我们的代码库的需求。在这些情况下（如下所述），限制或禁止标准功能是适当的。在某些情况下，我们更喜欢自行开发的或第三方的库，而不是 C++ 标准中定义的库，要么是因为它们被认为更好，要么是因为没有足够的价值将代码库转换为标准接口。
  Consistency with the way other organizations use C++ has value for the same reasons as consistency within our code base. If a feature in the C++ standard solves a problem, or if some idiom is widely known and accepted, that's an argument for using it. However, sometimes standard features and idioms are flawed, or were just designed without our codebase's needs in mind. In those cases (as described below) it's appropriate to constrain or ban standard features. In some cases we prefer a homegrown or third-party library over a library defined in the C++ Standard, either out of perceived superiority or insufficient value to transition the codebase to the standard interface.

- 避免令人惊讶或危险的构造
- Avoid surprising or dangerous constructs

  C++ 的特性比人们乍一看更令人惊讶或更危险。一些样式指南限制是为了防止陷入这些陷阱。样式指南对此类限制的豁免有很高的门槛，因为放弃此类规则通常会直接面临损害程序正确性的风险。
  C++ has features that are more surprising or dangerous than one might think at a glance. Some style guide restrictions are in place to prevent falling into these pitfalls. There is a high bar for style guide waivers on such restrictions, because waiving such rules often directly risks compromising program correctness.

- 避免我们的普通 C++ 程序员会觉得棘手或难以维护的结构
- Avoid constructs that our average C++ programmer would find tricky or hard to maintain

  C++ 具有可能不适合普遍使用的功能，因为它们给代码带来了复杂性。在广泛使用的代码中，使用更复杂的语言结构可能更容易被接受，因为更复杂的实现的任何好处都会随着使用而大幅增加，并且在使用代码的新部分时不需要再次付出理解复杂性的成本。代码库。如有疑问，可以通过询问您的项目负责人来寻求此类规则的豁免。这对于我们的代码库尤其重要，因为代码所有权和团队成员资格会随着时间的推移而发生变化：即使使用某些代码的每个人目前都理解它，这种理解也不能保证在几年后仍然有效。
  C++ has features that may not be generally appropriate because of the complexity they introduce to the code. In widely used code, it may be more acceptable to use trickier language constructs, because any benefits of more complex implementation are multiplied widely by usage, and the cost in understanding the complexity does not need to be paid again when working with new portions of the codebase. When in doubt, waivers to rules of this type can be sought by asking your project leads. This is specifically important for our codebase because code ownership and team membership changes over time: even if everyone that works with some piece of code currently understands it, such understanding is not guaranteed to hold a few years from now.

- 注意我们的规模
- Be mindful of our scale

  由于代码库包含 1 亿多行代码和数千名工程师，一名工程师的一些错误和简化可能会让许多工程师付出高昂的代价。例如，避免污染全局命名空间尤为重要：如果每个人都将内容放入全局命名空间，则数亿行代码库中的名称冲突将很难处理且难以避免。
  With a codebase of 100+ million lines and thousands of engineers, some mistakes and simplifications for one engineer can become costly for many. For instance it's particularly important to avoid polluting the global namespace: name collisions across a codebase of hundreds of millions of lines are difficult to work with and hard to avoid if everyone puts things into the global namespace.

- 必要时允许优化
- Concede to optimization when necessary

  性能优化有时可能是必要且适当的，即使它们与本文档的其他原则相冲突。
  Performance optimizations can sometimes be necessary and appropriate, even when they conflict with the other principles of this document.

本文件的目的是提供最大程度的指导和合理的限制。一如既往，常识和良好品味应该占上风。我们特指整个 Google C++ 社区的既定惯例，而不仅仅是您的个人偏好或您团队的偏好。对巧妙或不寻常的结构持怀疑态度并不愿使用：没有禁令并不等于获得继续进行的许可。运用您的判断，如果您不确定，请随时询问您的项目负责人以获取更多信息。
The intent of this document is to provide maximal guidance with reasonable restriction. As always, common sense and good taste should prevail. By this we specifically refer to the established conventions of the entire Google C++ community, not just your personal preferences or those of your team. Be skeptical about and reluctant to use clever or unusual constructs: the absence of a prohibition is not the same as a license to proceed. Use your judgment, and if you are unsure, please don't hesitate to ask your project leads to get additional input.

# C++ 版本

# C++ Version

目前，代码应以 C++17 为目标，即不应使用 C++2x 功能，_指定的初始化程序_ 除外。本指南针对的 C++ 版本将随着时间的推移（积极地）进步。
Currently, code should target C++17, i.e., should not use C++2x features, with the exception of _designated initializers_. The C++ version targeted by this guide will advance (aggressively) over time.

不要使用 [非标准扩展](#Nonstandard_Extensions)。
Do not use [non-standard extensions](#Nonstandard_Extensions).

在项目中使用 C++14 和 C++17 的功能之前，请考虑向其他环境的可移植性。
Consider portability to other environments before using features from C++14 and C++17 in your project.

# 头文件

# Header Files

一般来说，每个 `.cc` 文件都应该有一个关联的 `.h` 文件。有一些常见的例外，例如单元测试和仅包含 `main()` 函数的小 `.cc` 文件。
In general, every `.cc` file should have an associated `.h` file. There are some common exceptions, such as unit tests and small `.cc` files containing just a `main()` function.

正确使用头文件可以对代码的可读性、大小和性能产生巨大的影响。
Correct use of header files can make a huge difference to the readability, size and performance of your code.

以下规则将指导您克服使用头文件的各种陷阱。
The following rules will guide you through the various pitfalls of using header files.

## 自包含标头

## Self-contained Headers

头文件应该是独立的（自行编译）并以 `.h` 结尾。旨在包含的非头文件应以 `.inc` 结尾，并谨慎使用。
Header files should be self-contained (compile on their own) and end in `.h`. Non-header files that are meant for inclusion should end in `.inc` and be used sparingly.

所有头文件都应该是独立的。用户和重构工具不必遵守特殊条件才能包含标头。具体来说，标头应该具有*标头防护*并包含它需要的所有其他标头。
All header files should be self-contained. Users and refactoring tools should not have to adhere to special conditions to include the header. Specifically, a header should have _header guards_ and include all other headers it needs.

当标头声明标头的客户端将实例化的内联函数或模板时，内联函数和模板也必须在标头中直接或在其包含的文件中具有定义。不要将这些定义移动到单独包含的头文件（`-inl.h`）中；这种做法过去很常见，但现在已不再允许。当模板的所有实例化都出现在一个 `.cc` 文件中时，要么因为它们是[显式](https://en.cppreference.com/w/cpp/language/class_template#Explicit_instantiation)，要么因为定义是可访问的仅 `.cc` 文件，模板定义可以保留在该文件中。
When a header declares inline functions or templates that clients of the header will instantiate, the inline functions and templates must also have definitions in the header, either directly or in files it includes. Do not move these definitions to separately included header (`-inl.h`) files; this practice was common in the past, but is no longer allowed. When all instantiations of a template occur in one `.cc` file, either because they're [explicit](https://en.cppreference.com/w/cpp/language/class_template#Explicit_instantiation) or because the definition is accessible to only the `.cc` file, the template definition can be kept in that file.

在极少数情况下，设计为包含的文件不是独立的。这些通常旨在包含在不寻常的位置，例如另一个文件的中间。他们可能不使用*标头防护*，并且可能不包含其先决条件。使用 `.inc` 扩展名命名此类文件。谨慎使用，并尽可能选择独立的标头。
There are rare cases where a file designed to be included is not self-contained. These are typically intended to be included at unusual locations, such as the middle of another file. They might not use _header guards_, and might not include their prerequisites. Name such files with the `.inc` extension. Use sparingly, and prefer self-contained headers when possible.

## #define 保护

## The #define Guard

所有头文件都应该有 `#define` 保护以防止多重包含。符号名称的格式应为 `_<PROJECT>___<PATH>___<FILE>__H_` 。
All header files should have `#define` guards to prevent multiple inclusion. The format of the symbol name should be `_<PROJECT>___<PATH>___<FILE>__H_`.

为了保证唯一性，它们应该基于项目源树中的完整路径。例如，项目 `foo` 中的文件 `foo/src/bar/baz.h` 应该具有以下保护：
To guarantee uniqueness, they should be based on the full path in a project's source tree. For example, the file `foo/src/bar/baz.h` in project `foo` should have the following guard:

```C++
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_

...

#endif  // FOO_BAR_BAZ_H_
```

## 包括您使用的内容

## Include What You Use

如果源文件或头文件引用在其他地方定义的符号，则该文件应直接包含一个头文件，该头文件正确地旨在提供该符号的声明或定义。出于任何其他原因，它不应包含头文件。
If a source or header file refers to a symbol defined elsewhere, the file should directly include a header file which properly intends to provide a declaration or definition of that symbol. It should not include header files for any other reason.

不要依赖传递包含。这允许人们从标头中删除不再需要的 `#include` 语句，而不会破坏客户端。这也适用于相关标头 - 如果 `foo.cc` 使用其中的符号，则 `foo.cc` 应该包含 `bar.h` ，即使 `foo.h` 包含 `bar.h` 。
Do not rely on transitive inclusions. This allows people to remove no-longer-needed `#include` statements from their headers without breaking clients. This also applies to related headers - `foo.cc` should include `bar.h` if it uses a symbol from it even if `foo.h` includes `bar.h`.

## 转发声明

## Forward Declarations

尽可能避免使用前向声明。相反，_包含您需要的标头_。
Avoid using forward declarations where possible. Instead, _include the headers you need_.

"前向声明" 是没有关联定义的实体的声明。
A "forward declaration" is a declaration of an entity without an associated definition.

```C++
// In a C++ source file:
class B;
void FuncInB();
extern int variable_in_b;
ABSL_DECLARE_FLAG(flag_in_b);
```

- 前向声明可以节省编译时间，因为 `#include` 强制编译器打开更多文件并处理更多输入。
- Forward declarations can save compile time, as `#include`s force the compiler to open more files and process more input.
- 前向声明可以节省不必要的重新编译。由于标头中不相关的更改， `#include` 可能会强制您的代码更频繁地重新编译。
- Forward declarations can save on unnecessary recompilation. `#include`s can force your code to be recompiled more often, due to unrelated changes in the header.

- 前向声明可以隐藏依赖项，允许用户代码在标头更改时跳过必要的重新编译。
- Forward declarations can hide a dependency, allowing user code to skip necessary recompilation when headers change.
- 与 `#include` 语句相反的前向声明使得自动工具很难发现定义符号的模块。
- A forward declaration as opposed to an `#include` statement makes it difficult for automatic tooling to discover the module defining the symbol.
- 前向声明可能会因库的后续更改而被破坏。函数和模板的前向声明可以防止标头所有者对其 API 进行其他兼容的更改，例如扩大参数类型、添加具有默认值的模板参数或迁移到新的命名空间。
- A forward declaration may be broken by subsequent changes to the library. Forward declarations of functions and templates can prevent the header owners from making otherwise-compatible changes to their APIs, such as widening a parameter type, adding a template parameter with a default value, or migrating to a new namespace.
- 从名称空间 `std::` 向前声明符号会产生未定义的行为。
- Forward declaring symbols from namespace `std::` yields undefined behavior.
- 可能很难确定是否需要前向声明或完整的 `#include` 。用前向声明替换`#include`可以默默地改变代码的含义：
- It can be difficult to determine whether a forward declaration or a full `#include` is needed. Replacing an `#include` with a forward declaration can silently change the meaning of code:

  ```C++
  // b.h:
  struct B {};
  struct D : B {};

  // good_user.cc:
  #include "b.h"
  void f(B*);
  void f(void*);
  void test(D* x) { f(x); }  // Calls f(B*)
  ```

如果将 `#include` 替换为 `B` 和 `D` 的前向声明，则 `test()` 将调用 `f(void*)` 。
If the `#include` was replaced with forward decls for `B` and `D`, `test()` would call `f(void*)`.

- 从标头中转发声明多个符号可能比简单地 `#include` 标头更详细。
- Forward declaring multiple symbols from a header can be more verbose than simply `#include`ing the header.
- 构建代码以启用前向声明（例如，使用指针成员而不是对象成员）可能会使代码更慢且更复杂。
- Structuring code to enable forward declarations (e.g., using pointer members instead of object members) can make the code slower and more complex.

尽量避免在另一个项目中定义的实体的前向声明。
Try to avoid forward declarations of entities defined in another project.

## 内联函数

## Inline Functions

仅当函数很小时（例如 10 行或更少）时才定义内联函数。
Define functions inline only when they are small, say, 10 lines or fewer.

您可以以允许编译器内联扩展函数的方式声明函数，而不是通过通常的函数调用机制调用它们。
You can declare functions in a way that allows the compiler to expand them inline rather than calling them through the usual function call mechanism.

只要内联函数很小，内联函数就可以生成更有效的目标代码。随意内联访问器和修改器以及其他简短的、性能关键的函数。
Inlining a function can generate more efficient object code, as long as the inlined function is small. Feel free to inline accessors and mutators, and other short, performance-critical functions.

过度使用内联实际上会使程序变慢。根据函数的大小，内联它可能会导致代码大小增加或减少。内联一个非常小的访问器函数通常会减少代码大小，而内联一个非常大的函数会显着增加代码大小。在现代处理器上，由于更好地利用指令缓存，较小的代码通常运行得更快。
Overuse of inlining can actually make programs slower. Depending on a function's size, inlining it can cause the code size to increase or decrease. Inlining a very small accessor function will usually decrease code size while inlining a very large function can dramatically increase code size. On modern processors smaller code usually runs faster due to better use of the instruction cache.

一个不错的经验法则是，如果函数长度超过 10 行，则不要内联函数。小心析构函数，由于隐式的成员析构函数和基析构函数调用，析构函数通常比它们看起来要长！
A decent rule of thumb is to not inline a function if it is more than 10 lines long. Beware of destructors, which are often longer than they appear because of implicit member- and base-destructor calls!

另一个有用的经验法则：使用循环或 switch 语句内联函数通常不具有成本效益（除非在常见情况下，循环或 switch 语句从不执行）。
Another useful rule of thumb: it's typically not cost effective to inline functions with loops or switch statements (unless, in the common case, the loop or switch statement is never executed).

重要的是要知道函数并不总是内联的，即使它们是这样声明的；例如，虚拟函数和递归函数通常不是内联的。通常递归函数不应该是内联的。使虚拟函数内联的主要原因是将其定义放在类中，要么是为了方便，要么是为了记录其行为，例如访问器和修改器。
It is important to know that functions are not always inlined even if they are declared as such; for example, virtual and recursive functions are not normally inlined. Usually recursive functions should not be inline. The main reason for making a virtual function inline is to place its definition in the class, either for convenience or to document its behavior, e.g., for accessors and mutators.

## 包含的名称和顺序

## Names and Order of Includes

按以下顺序包含头文件：相关头文件、C 系统头文件、C++ 标准库头文件、其他库的头文件、您项目的头文件。
Include headers in the following order: Related header, C system headers, C++ standard library headers, other libraries' headers, your project's headers.

项目的所有头文件都应列为项目源目录的后代，而不使用 UNIX 目录别名 `.` （当前目录）或 `..` （父目录）。例如， `google-awesome-project/src/base/logging.h` 应包含为：
All of a project's header files should be listed as descendants of the project's source directory without use of UNIX directory aliases `.` (the current directory) or `..` (the parent directory). For example, `google-awesome-project/src/base/logging.h` should be included as:

```C++
#include "base/logging.h"
```

在 `dir/foo.cc` 或 `dir/foo_test.cc` 中，其主要目的是实现或测试 `dir2/foo2.h` 中的内容，按如下方式排序您的包含内容：

1. `dir2/foo2.h`。
2. 空行
3. C 系统头文件（更准确地说：尖括号中带有 `.h` 扩展名的头文件），例如 `<unistd.h>` 、 `<stdlib.h>` 。
4. 空行
5. C++ 标准库头文件（不带文件扩展名），例如`<algorithm>`、`<cstddef>`。
6. 空行
7. 其他库的`.h`文件。
8. 空行
9. 您项目的 `.h` 文件。

In `dir/foo.cc` or `dir/foo_test.cc`, whose main purpose is to implement or test the stuff in `dir2/foo2.h`, order your includes as follows:

1.  `dir2/foo2.h`.
2.  A blank line
3.  C system headers (more precisely: headers in angle brackets with the `.h` extension), e.g., `<unistd.h>`, `<stdlib.h>`.
4.  A blank line
5.  C++ standard library headers (without file extension), e.g., `<algorithm>`, `<cstddef>`.
6.  A blank line
7.  Other libraries' `.h` files.
8.  A blank line
9.  Your project's `.h` files.

用一个空行分隔每个非空组。
Separate each non-empty group with one blank line.

使用首选顺序，如果相关标头 `dir2/foo2.h` 省略任何必要的包含， `dir/foo.cc` 或 `dir/foo_test.cc` 的构建将中断。因此，此规则确保构建中断首先出现在处理这些文件的人员身上，而不是其他软件包中的无辜人员身上。
With the preferred ordering, if the related header `dir2/foo2.h` omits any necessary includes, the build of `dir/foo.cc` or `dir/foo_test.cc` will break. Thus, this rule ensures that build breaks show up first for the people working on these files, not for innocent people in other packages.

`dir/foo.cc` 和 `dir2/foo2.h` 通常位于同一目录中（例如，`base/basictypes_test.cc` 和 `base/basictypes.h`），但有时也可能位于不同目录中。
`dir/foo.cc` and `dir2/foo2.h` are usually in the same directory (e.g., `base/basictypes_test.cc` and `base/basictypes.h`), but may sometimes be in different directories too.

请注意，C 头文件（例如 `stddef.h` ）本质上可以与 C++ 头文件（ `cstddef` ）互换。任何一种风格都可以接受，但更喜欢与现有代码保持一致。
Note that the C headers such as `stddef.h` are essentially interchangeable with their C++ counterparts (`cstddef`). Either style is acceptable, but prefer consistency with existing code.

每个部分中的包含内容应按字母顺序排列。请注意，较旧的代码可能不符合此规则，应在方便时进行修复。
Within each section the includes should be ordered alphabetically. Note that older code might not conform to this rule and should be fixed when convenient.

例如， `google-awesome-project/src/foo/internal/fooserver.cc` 中的包含内容可能如下所示：
For example, the includes in `google-awesome-project/src/foo/internal/fooserver.cc` might look like this:

```C++
#include "foo/server/fooserver.h"

#include <sys/types.h>
#include <unistd.h>

#include <string>
#include <vector>

#include "base/basictypes.h"
#include "foo/server/bar.h"
#include "third_party/absl/flags/flag.h"
```

**例外：**
**Exception:**

有时，系统特定的代码需要条件包含。此类代码可以将条件包含放在其他包含之后。当然，请保持系统特定的代码较小且本地化。例子：
Sometimes, system-specific code needs conditional includes. Such code can put conditional includes after other includes. Of course, keep your system-specific code small and localized. Example:

```C++
#include "foo/public/fooserver.h"

#include "base/port.h"  // For LANG_CXX11.

#ifdef LANG_CXX11
#include <initializer_list>
#endif  // LANG_CXX11
```

# 范围界定

# Scoping

## 命名空间

## Namespaces

除少数例外，将代码放置在命名空间中。命名空间应该具有基于项目名称及其路径的唯一名称。不要使用 _using-directives_（例如，`using namespace foo`）。不要使用内联命名空间。对于未命名的命名空间，请参阅*内部链接*。
With few exceptions, place code in a namespace. Namespaces should have unique names based on the project name, and possibly its path. Do not use _using-directives_ (e.g., `using namespace foo`). Do not use inline namespaces. For unnamed namespaces, see _Internal Linkage_.

命名空间将全局范围细分为不同的命名范围，因此对于防止全局范围中的名称冲突非常有用。
Namespaces subdivide the global scope into distinct, named scopes, and so are useful for preventing name collisions in the global scope.

命名空间提供了一种防止大型程序中名称冲突的方法，同时允许大多数代码使用相当短的名称。
Namespaces provide a method for preventing name conflicts in large programs while allowing most code to use reasonably short names.

例如，如果两个不同的项目在全局范围内有一个类 `Foo` ，这些符号可能会在编译时或运行时发生冲突。如果每个项目将其代码放在命名空间中，则 `project1::Foo` 和 `project2::Foo` 现在是不冲突的不同符号，并且每个项目命名空间内的代码可以继续引用 `Foo` 而不带前缀。
For example, if two different projects have a class `Foo` in the global scope, these symbols may collide at compile time or at runtime. If each project places their code in a namespace, `project1::Foo` and `project2::Foo` are now distinct symbols that do not collide, and code within each project's namespace can continue to refer to `Foo` without the prefix.

内联命名空间会自动将其名称放置在封闭范围内。例如，考虑以下代码片段：
Inline namespaces automatically place their names in the enclosing scope. Consider the following snippet, for example:

**neutralcode**

```C++
namespace outer {
inline namespace inner {
  void foo();
}  // namespace inner
}  // namespace outer
```

表达式 `outer::inner::foo()` 和 `outer::foo()` 是可以互换的。内联命名空间主要用于跨版本的 ABI 兼容性。
The expressions `outer::inner::foo()` and `outer::foo()` are interchangeable. Inline namespaces are primarily intended for ABI compatibility across versions.

命名空间可能会令人困惑，因为它们使确定名称所指定义的机制变得复杂。
Namespaces can be confusing, because they complicate the mechanics of figuring out what definition a name refers to.

特别是内联命名空间可能会令人困惑，因为名称实际上并不限于声明它们的命名空间。它们仅作为某些较大版本控制策略的一部分有用。
Inline namespaces, in particular, can be confusing because names aren't actually restricted to the namespace where they are declared. They are only useful as part of some larger versioning policy.

在某些情况下，有必要通过符号的完全限定名称重复引用它们。对于深度嵌套的命名空间，这可能会增加很多混乱。
In some contexts, it's necessary to repeatedly refer to symbols by their fully-qualified names. For deeply-nested namespaces, this can add a lot of clutter.

命名空间应按如下方式使用：
Namespaces should be used as follows:

- 遵循*命名空间名称*的规则。
- Follow the rules on _Namespace Names_.
- 使用注释终止多行名称空间，如给定示例所示。
- Terminate multi-line namespaces with comments as shown in the given examples.
- 命名空间在包含、[gflags](https://gflags.github.io/gflags/)定义/声明以及来自其他命名空间的类的前向声明之后包装整个源文件。
- Namespaces wrap the entire source file after includes, [gflags](https://gflags.github.io/gflags/) definitions/declarations and forward declarations of classes from other namespaces.

  ```C++
  // In the .h file
  namespace mynamespace {

  // All declarations are within the namespace scope.
  // Notice the lack of indentation.
  class MyClass {
   public:
    ...
    void Foo();
  };

  }  // namespace mynamespace
  ```

  ```C++
  // In the .cc file
  namespace mynamespace {

  // Definition of functions is within scope of the namespace.
  void MyClass::Foo() {
    ...
  }

  }  // namespace mynamespace
  ```

  更复杂的 `.cc` 文件可能包含其他详细信息，例如标志或使用声明。
  More complex `.cc` files might have additional details, like flags or using-declarations.

  ```C++
  #include "a.h"

  ABSL_FLAG(bool, someflag, false, "a flag");

  namespace mynamespace {

  using ::foo::Bar;

  ...code for mynamespace...    // Code goes against the left margin.

  }  // namespace mynamespace
  ```

- 要将生成的协议消息代码放置在命名空间中，请在 `.proto` 文件中使用 `package` 说明符。有关详细信息，请参阅[协议缓冲区包](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated#package)。
- To place generated protocol message code in a namespace, use the `package` specifier in the `.proto` file. See [Protocol Buffer Packages](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated#package) for details.
- 不要在命名空间 `std` 中声明任何内容，包括标准库类的前向声明。在名称空间 `std` 中声明实体是未定义的行为，即不可移植。要声明标准库中的实体，请包含适当的头文件。
- Do not declare anything in namespace `std`, including forward declarations of standard library classes. Declaring entities in namespace `std` is undefined behavior, i.e., not portable. To declare entities from the standard library, include the appropriate header file.
- 您不能使用 _using-directive_ 来使命名空间中的所有名称可用。
- You may not use a _using-directive_ to make all names from a namespace available.

  **badcode**

  ```C++
  // Forbidden -- This pollutes the namespace.
  using namespace foo;
  ```

- 不要在头文件中的命名空间范围内使用*命名空间别名*，除非在显式标记的仅限内部命名空间中，因为导入到头文件中的命名空间中的任何内容都会成为该文件导出的公共 API 的一部分。
- Do not use _Namespace aliases_ at namespace scope in header files except in explicitly marked internal-only namespaces, because anything imported into a namespace in a header file becomes part of the public API exported by that file.

  ```C++
  // Shorten access to some commonly used names in .cc files.
  namespace baz = ::foo::bar::baz;
  ```

  ```C++
  // Shorten access to some commonly used names (in a .h file).
  namespace librarian {
  namespace impl {  // Internal, not part of the API.
  namespace sidetable = ::pipeline_diagnostics::sidetable;
  }  // namespace impl

  inline void my_inline_function() {
    // namespace alias local to a function (or method).
    namespace baz = ::foo::bar::baz;
    ...
  }
  }  // namespace librarian
  ```

- 不要使用内联命名空间。
- Do not use inline namespaces.
- 使用名称中带有 "internal" 的命名空间来记录 API 用户不应提及的 API 部分。
- Use namespaces with "internal" in the name to document parts of an API that should not be mentioned by users of the API.

  **badcode**

  ```C++
  // We shouldn't use this internal name in non-absl code.
  using ::absl::container_internal::ImplementationDetail;
  ```

## 内部链接

## Internal Linkage

当 `.cc` 文件中的定义不需要在该文件外部引用时，可以通过将它们放置在未命名的命名空间中或将它们声明为 `static` 来提供内部链接。不要在 `.h` 文件中使用这些结构。
When definitions in a `.cc` file do not need to be referenced outside that file, give them internal linkage by placing them in an unnamed namespace or declaring them `static`. Do not use either of these constructs in `.h` files.

所有声明都可以通过将它们放置在未命名的命名空间中来给予内部链接。函数和变量也可以通过声明为 `static` 来给予内部链接。这意味着您声明的任何内容都无法从另一个文件访问。如果不同的文件声明具有相同名称的内容，则这两个实体是完全独立的。
All declarations can be given internal linkage by placing them in unnamed namespaces. Functions and variables can also be given internal linkage by declaring them `static`. This means that anything you're declaring can't be accessed from another file. If a different file declares something with the same name, then the two entities are completely independent.

对于不需要在其他地方引用的所有代码，鼓励使用 `.cc` 文件中的内部链接。不要在 `.h` 文件中使用内部链接。
Use of internal linkage in `.cc` files is encouraged for all code that does not need to be referenced elsewhere. Do not use internal linkage in `.h` files.

像命名命名空间一样设置未命名命名空间的格式。在终止注释中，将命名空间名称留空：
Format unnamed namespaces like named namespaces. In the terminating comment, leave the namespace name empty:

```C++
namespace {
...
}  // namespace
```

## 非成员、静态成员和全局函数

## Nonmember, Static Member, and Global Functions

更喜欢将非成员函数放在命名空间中；很少使用完全全局的函数。不要仅仅使用类来对静态成员进行分组。类的静态方法通常应该与类的实例或类的静态数据密切相关。
Prefer placing nonmember functions in a namespace; use completely global functions rarely. Do not use a class simply to group static members. Static methods of a class should generally be closely related to instances of the class or the class's static data.

非成员函数和静态成员函数在某些情况下可能很有用。将非成员函数放入命名空间可以避免污染全局命名空间。
Nonmember and static member functions can be useful in some situations. Putting nonmember functions in a namespace avoids polluting the global namespace.

非成员和静态成员函数作为新类的成员可能更有意义，特别是当它们访问外部资源或具有显着依赖性时。
Nonmember and static member functions may make more sense as members of a new class, especially if they access external resources or have significant dependencies.

有时定义一个不绑定到类实例的函数很有用。这样的函数可以是静态成员函数或非成员函数。非成员函数不应依赖于外部变量，并且几乎应始终存在于命名空间中。不要创建类只是为了对静态成员进行分组；这与仅仅给名称提供一个共同的前缀没有什么不同，而且这样的分组通常是不必要的。
Sometimes it is useful to define a function not bound to a class instance. Such a function can be either a static member or a nonmember function. Nonmember functions should not depend on external variables, and should nearly always exist in a namespace. Do not create classes only to group static members; this is no different than just giving the names a common prefix, and such grouping is usually unnecessary anyway.

如果您定义了一个非成员函数并且仅在其 `.cc` 文件中需要它，请使用*内部链接*来限制其范围。
If you define a nonmember function and it is only needed in its `.cc` file, use _internal linkage_ to limit its scope.

## 局部变量

## Local Variables

将函数的变量放置在尽可能窄的范围内，并在声明中初始化变量。
Place a function's variables in the narrowest scope possible, and initialize variables in the declaration.

C++ 允许您在函数中的任何位置声明变量。我们鼓励您在尽可能本地的范围内声明它们，并尽可能接近第一次使用。这使得读者更容易找到声明并查看变量的类型以及它被初始化为什么。特别是，应该使用初始化而不是声明和赋值，例如：
C++ allows you to declare variables anywhere in a function. We encourage you to declare them in a scope as local as possible, and as close to the first use as possible. This makes it easier for the reader to find the declaration and see what type the variable is and what it was initialized to. In particular, initialization should be used instead of declaration and assignment, e.g.,:

**badcode**

```C++
int i;
i = f();      // Bad -- initialization separate from declaration.
```

```C++
int i = f();  // Good -- declaration has initialization.
```

**badcode**

```C++
int jobs = NumJobs();
// More code...
f(jobs);      // Bad -- declaration separate from use.
```

```C++
int jobs = NumJobs();
f(jobs);      // Good -- declaration immediately (or closely) followed by use.
```

**badcode**

```C++
std::vector<int> v;
v.push_back(1);  // Prefer initializing using brace initialization.
v.push_back(2);
```

```C++
std::vector<int> v = {1, 2};  // Good -- v starts initialized.
```

`if` 、 `while` 和 `for` 语句所需的变量通常应在这些语句内声明，以便这些变量被限制在这些范围内。例如：
Variables needed for `if`, `while` and `for` statements should normally be declared within those statements, so that such variables are confined to those scopes. E.g.:

```C++
while (const char* p = strchr(str, '/')) str = p + 1;
```

有一个警告：如果变量是一个对象，则每次进入作用域并创建时都会调用其构造函数，每次超出作用域时都会调用其析构函数。
There is one caveat: if the variable is an object, its constructor is invoked every time it enters scope and is created, and its destructor is invoked every time it goes out of scope.

**badcode**

```C++
// Inefficient implementation:
for (int i = 0; i < 1000000; ++i) {
  Foo f;  // My ctor and dtor get called 1000000 times each.
  f.DoSomething(i);
}
```

在循环外部声明一个在循环中使用的变量可能会更有效：
It may be more efficient to declare such a variable used in a loop outside that loop:

```C++
Foo f;  // My ctor and dtor get called once each.
for (int i = 0; i < 1000000; ++i) {
  f.DoSomething(i);
}
```

## 静态和全局变量

## Static and Global Variables

禁止具有[静态存储持续时间](http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration)的对象，除非它们[可轻易破坏](http://en.cppreference.com/w/cpp/types/is_destructible)。非正式地，这意味着析构函数不执行任何操作，即使考虑到成员和基析构函数。更正式地说，它意味着该类型没有用户定义的或虚拟的析构函数，并且所有基类和非静态成员都是可轻易破坏的。静态函数局部变量可以使用动态初始化。不鼓励对静态类成员变量或命名空间范围内的变量使用动态初始化，但在有限的情况下允许；详情请参见下文。
Objects with [static storage duration](http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration) are forbidden unless they are [trivially destructible](http://en.cppreference.com/w/cpp/types/is_destructible). Informally this means that the destructor does not do anything, even taking member and base destructors into account. More formally it means that the type has no user-defined or virtual destructor and that all bases and non-static members are trivially destructible. Static function-local variables may use dynamic initialization. Use of dynamic initialization for static class member variables or variables at namespace scope is discouraged, but allowed in limited circumstances; see below for details.

根据经验，如果全局变量的声明（单独考虑）可以是 `constexpr` ，则它满足这些要求。
As a rule of thumb: a global variable satisfies these requirements if its declaration, considered in isolation, could be `constexpr`.

每个对象都有一个存储持续时间，这与其生命周期相关。具有静态存储持续时间的对象从其初始化点一直存活到程序结束。此类对象显示为名称空间范围内的变量（ "全局变量" ）、类的静态数据成员或使用 `static` 说明符声明的函数局部变量。函数局部静态变量在控制权首次通过其声明时被初始化；所有其他具有静态存储持续时间的对象都作为程序启动的一部分进行初始化。所有具有静态存储持续时间的对象都会在程序退出时被销毁（这发生在未加入的线程终止之前）。
Every object has a storage duration, which correlates with its lifetime. Objects with static storage duration live from the point of their initialization until the end of the program. Such objects appear as variables at namespace scope ("global variables"), as static data members of classes, or as function-local variables that are declared with the `static` specifier. Function-local static variables are initialized when control first passes through their declaration; all other objects with static storage duration are initialized as part of program start-up. All objects with static storage duration are destroyed at program exit (which happens before unjoined threads are terminated).

初始化可能是动态的，这意味着初始化期间会发生一些重要的事情。 （例如，考虑分配内存的构造函数，或使用当前进程 ID 初始化的变量。）另一种初始化是静态初始化。不过，这两者并不完全相反：静态初始化*always*发生在具有静态存储持续时间的对象上（将对象初始化为给定常量或由所有字节设置为零的表示形式），而动态初始化发生在这之后，如果必需的。
Initialization may be dynamic, which means that something non-trivial happens during initialization. (For example, consider a constructor that allocates memory, or a variable that is initialized with the current process ID.) The other kind of initialization is static initialization. The two aren't quite opposites, though: static initialization _always_ happens to objects with static storage duration (initializing the object either to a given constant or to a representation consisting of all bytes set to zero), whereas dynamic initialization happens after that, if required.

全局变量和静态变量对于大量应用程序非常有用：命名常量、某些翻译单元内部的辅助数据结构、命令行标志、日志记录、注册机制、后台基础设施等。
Global and static variables are very useful for a large number of applications: named constants, auxiliary data structures internal to some translation unit, command-line flags, logging, registration mechanisms, background infrastructure, etc.

使用动态初始化或具有重要析构函数的全局变量和静态变量会产生复杂性，很容易导致难以发现的错误。动态初始化不是跨翻译单元排序的，销毁也不是（除了销毁以与初始化相反的顺序发生）。当一个初始化引用另一个具有静态存储持续时间的变量时，这可能会导致对象在其生命周期开始之前（或在其生命周期结束之后）被访问。此外，当程序启动在退出时未加入的线程时，如果这些线程的析构函数已经运行，则这些线程可能会在其生命周期结束后尝试访问对象。
Global and static variables that use dynamic initialization or have non-trivial destructors create complexity that can easily lead to hard-to-find bugs. Dynamic initialization is not ordered across translation units, and neither is destruction (except that destruction happens in reverse order of initialization). When one initialization refers to another variable with static storage duration, it is possible that this causes an object to be accessed before its lifetime has begun (or after its lifetime has ended). Moreover, when a program starts threads that are not joined at exit, those threads may attempt to access objects after their lifetime has ended if their destructor has already run.

### 销毁决定

### Decision on destruction

当析构函数很简单时，它们的执行根本不受顺序限制（它们实际上不是 "运行" ）；否则，我们将面临在对象生命周期结束后访问对象的风险。因此，我们只允许具有静态存储持续时间的对象（如果它们是可轻易破坏的）。基本类型（如指针和 `int` ）是普通可破坏的，普通可破坏类型的数组也是如此。请注意，标有 `constexpr` 的变量是可破坏的。
When destructors are trivial, their execution is not subject to ordering at all (they are effectively not "run"); otherwise we are exposed to the risk of accessing objects after the end of their lifetime. Therefore, we only allow objects with static storage duration if they are trivially destructible. Fundamental types (like pointers and `int`) are trivially destructible, as are arrays of trivially destructible types. Note that variables marked with `constexpr` are trivially destructible.

```C++
const int kNum = 10;  // Allowed

struct X { int n; };
const X kX[] = {{1}, {2}, {3}};  // Allowed

void foo() {
  static const char* const kMessages[] = {"hello", "world"};  // Allowed
}

// Allowed: constexpr guarantees trivial destructor.
constexpr std::array<int, 3> kArray = {1, 2, 3};
```

**badcode**

```C++
// bad: non-trivial destructor
const std::string kFoo = "foo";

// Bad for the same reason, even though kBar is a reference (the
// rule also applies to lifetime-extended temporary objects).
const std::string& kBar = StrCat("a", "b", "c");

void bar() {
  // Bad: non-trivial destructor.
  static std::map<int, int> kData = {{1, 0}, {2, 0}, {3, 0}};
}
```

请注意，引用不是对象，因此它们不受可破坏性的约束。不过，动态初始化的限制仍然适用。特别是，允许使用 `static T& t = *new T;` 形式的函数局部静态引用。
Note that references are not objects, and thus they are not subject to the constraints on destructibility. The constraint on dynamic initialization still applies, though. In particular, a function-local static reference of the form `static T& t = *new T;` is allowed.

### 初始化决定

### Decision on initialization

初始化是一个更复杂的主题。这是因为我们不仅要考虑类构造函数是否执行，还必须考虑初始化器的评估：
Initialization is a more complex topic. This is because we must not only consider whether class constructors execute, but we must also consider the evaluation of the initializer:

**neutralcode**

```C++
int n = 5;    // Fine
int m = f();  // ? (Depends on f)
Foo x;        // ? (Depends on Foo::Foo)
Bar y = g();  // ? (Depends on g and on Bar::Bar)
```

除了第一条语句之外的所有语句都让我们面临不确定的初始化顺序。
All but the first statement expose us to indeterminate initialization ordering.

我们正在寻找的概念在 C++ 标准的形式语言中称为 _常量初始化_ 。这意味着初始化表达式是一个常量表达式，如果对象是通过构造函数调用初始化的，那么构造函数也必须指定为 `constexpr` ：
The concept we are looking for is called _constant initialization_ in the formal language of the C++ standard. It means that the initializing expression is a constant expression, and if the object is initialized by a constructor call, then the constructor must be specified as `constexpr`, too:

```C++
struct Foo { constexpr Foo(int) {} };

int n = 5;  // Fine, 5 is a constant expression.
Foo x(2);   // Fine, 2 is a constant expression and the chosen constructor is constexpr.
Foo a[] = { Foo(1), Foo(2), Foo(3) };  // Fine
```

始终允许常量初始化。静态存储持续时间变量的常量初始化应使用 `constexpr` 进行标记，或者在可能的情况下使用 [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/03c1513538584f4a04d666be5eb469e3979febba/absl/base/attributes.h#L540) 属性。任何没有如此标记的非本地静态存储持续时间变量都应该被假定为具有动态初始化，并非常仔细地检查。
Constant initialization is always allowed. Constant initialization of static storage duration variables should be marked with `constexpr` or where possible the [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/03c1513538584f4a04d666be5eb469e3979febba/absl/base/attributes.h#L540) attribute. Any non-local static storage duration variable that is not so marked should be presumed to have dynamic initialization, and reviewed very carefully.

相比之下，以下初始化是有问题的：
By contrast, the following initializations are problematic:

**badcode**

```C++
// Some declarations used below.
time_t time(time_t*);      // Not constexpr!
int f();                   // Not constexpr!
struct Bar { Bar() {} };

// Problematic initializations.
time_t m = time(nullptr);  // Initializing expression not a constant expression.
Foo y(f());                // Ditto
Bar b;                     // Chosen constructor Bar::Bar() not constexpr.
```

不鼓励动态初始化非局部变量，并且一般来说是禁止的。但是，如果程序的任何方面都不依赖于此初始化相对于所有其他初始化的顺序，我们确实允许这样做。在这些限制下，初始化的顺序不会产生明显的差异。例如：
Dynamic initialization of nonlocal variables is discouraged, and in general it is forbidden. However, we do permit it if no aspect of the program depends on the sequencing of this initialization with respect to all other initializations. Under those restrictions, the ordering of the initialization does not make an observable difference. For example:

```C++
int p = getpid();  // Allowed, as long as no other static variable
                   // uses p in its own initialization.
```

静态局部变量的动态初始化是允许的（并且很常见）。
Dynamic initialization of static local variables is allowed (and common).

### 常见模式

### Common patterns

- 全局字符串：如果需要命名全局或静态字符串常量，请考虑使用指向字符串字面量的 `string_view` 、字符数组或字符指针的 `constexpr` 变量。字符串字面量已经具有静态存储持续时间，并且通常就足够了。请参阅 [TotW #140.](https://abseil.io/tips/140)
- Global strings: if you require a named global or static string constant, consider using a `constexpr` variable of `string_view`, character array, or character pointer, pointing to a string literal. String literals have static storage duration already and are usually sufficient. See [TotW #140.](https://abseil.io/tips/140)
- 映射、集合和其他动态容器：如果您需要静态、固定的集合（例如要搜索的集合或查找表），则不能将标准库中的动态容器用作静态变量，因为它们不具有静态变量。 - 琐碎的析构函数。相反，考虑一个由简单类型组成的简单数组，例如，整数数组的数组（用于 `从 int 到 int 的映射` ），或对数组（例如， `int` 和 `const char*` 对） 。对于小型集合，线性搜索完全足够（并且由于内存局部性而有效）；考虑使用 [absl/algorithm/container.h](https://github.com/abseil/abseil-cpp/blob/master/absl/algorithm/container.h) 中的设施进行标准操作。如有必要，请保持集合按排序顺序并使用二分搜索算法。如果您确实更喜欢标准库中的动态容器，请考虑使用函数局部静态指针，如下所述。
- Maps, sets, and other dynamic containers: if you require a static, fixed collection, such as a set to search against or a lookup table, you cannot use the dynamic containers from the standard library as a static variable, since they have non-trivial destructors. Instead, consider a simple array of trivial types, e.g., an array of arrays of ints (for a "map from int to int"), or an array of pairs (e.g., pairs of `int` and `const char*`). For small collections, linear search is entirely sufficient (and efficient, due to memory locality); consider using the facilities from [absl/algorithm/container.h](https://github.com/abseil/abseil-cpp/blob/master/absl/algorithm/container.h) for the standard operations. If necessary, keep the collection in sorted order and use a binary search algorithm. If you do really prefer a dynamic container from the standard library, consider using a function-local static pointer, as described below .
- 智能指针（`std::unique_ptr`、`std::shared_ptr`）：智能指针在销毁期间执行清理，因此被禁止。考虑您的用例是否适合本节中描述的其他模式之一。一个简单的解决方案是使用指向动态分配对象的普通指针，并且永远不删除它（请参阅最后一项）。
- Smart pointers (`std::unique_ptr`, `std::shared_ptr`): smart pointers execute cleanup during destruction and are therefore forbidden. Consider whether your use case fits into one of the other patterns described in this section. One simple solution is to use a plain pointer to a dynamically allocated object and never delete it (see last item).
- 自定义类型的静态变量：如果您需要需要自己定义的类型的静态常量数据，请为该类型提供一个简单的析构函数和一个 `constexpr` 构造函数。
- Static variables of custom types: if you require static, constant data of a type that you need to define yourself, give the type a trivial destructor and a `constexpr` constructor.
- 如果所有其他方法都失败，您可以动态创建一个对象，并且永远不会使用函数本地静态指针或引用删除它（例如，`static const auto& impl = *new T(args...);`）。
- If all else fails, you can create an object dynamically and never delete it by using a function-local static pointer or reference (e.g., `static const auto& impl = *new T(args...);`).

## thread_local 变量

## thread_local Variables

未在函数内声明的 thread_local 变量必须使用真正的编译时常量进行初始化，并且必须使用 [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h) 属性。与其他定义线程本地数据的方法相比，更喜欢使用 `thread_local` 。
`thread_local` variables that aren't declared inside a function must be initialized with a true compile-time constant, and this must be enforced by using the [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h) attribute. Prefer `thread_local` over other ways of defining thread-local data.

可以使用 `thread_local` 说明符声明变量：
Variables can be declared with the `thread_local` specifier:

```C++
thread_local Foo foo = ...;
```

这样的变量实际上是一个对象的集合，这样不同的线程访问它时，实际上访问的是不同的对象。 `thread_local` 变量在很多方面都很像*静态存储持续时间变量*。例如，它们可以在命名空间范围、函数内部或作为静态类成员声明，但不能作为普通类成员声明。
Such a variable is actually a collection of objects, so that when different threads access it, they are actually accessing different objects. `thread_local` variables are much like _static storage duration variables_ in many respects. For instance, they can be declared at namespace scope, inside functions, or as static class members, but not as ordinary class members.

`thread_local` 变量实例的初始化与静态变量非常相似，只不过它们必须为每个线程单独初始化，而不是在程序启动时初始化一次。这意味着函数中声明的 `thread_local` 变量是安全的，但其他 `thread_local` 变量会受到与静态变量相同的初始化顺序问题（以及更多问题）的影响。
`thread_local` variable instances are initialized much like static variables, except that they must be initialized separately for each thread, rather than once at program startup. This means that `thread_local` variables declared within a function are safe, but other `thread_local` variables are subject to the same initialization-order issues as static variables (and more besides).

`thread_local` 变量有一个微妙的销毁顺序问题：在线程关闭期间，`thread_local` 变量将以与初始化相反的顺序销毁（C++ 中通常如此）。如果由任何 `thread_local` 变量的析构函数触发的代码引用该线程上任何已销毁的 `thread_local` ，我们将特别难以诊断释放后使用。
`thread_local` variables have a subtle destruction-order issue: during thread shutdown, `thread_local` variables will be destroyed in the opposite order of their initialization (as is generally true in C++). If code triggered by the destructor of any `thread_local` variable refers to any already-destroyed `thread_local` on that thread, we will get a particularly hard to diagnose use-after-free.

- 线程本地数据本质上是安全的，不会出现竞争（因为通常只有一个线程可以访问它），这使得 `thread_local` 对于并发编程非常有用。
- Thread-local data is inherently safe from races (because only one thread can ordinarily access it), which makes `thread_local` useful for concurrent programming.
- `thread_local` 是创建线程本地数据的唯一标准支持的方式。
- `thread_local` is the only standard-supported way of creating thread-local data.

- 访问 `thread_local` 变量可能会在线程启动或首次在给定线程上使用期间触发不可预测且无法控制数量的其他代码的执行。
- Accessing a `thread_local` variable may trigger execution of an unpredictable and uncontrollable amount of other code during thread-start or first use on a given thread.
- `thread_local` 变量实际上是全局变量，除了缺乏线程安全性之外，具有全局变量的所有缺点。
- `thread_local` variables are effectively global variables, and have all the drawbacks of global variables other than lack of thread-safety.
- `thread_local` 变量消耗的内存随着正在运行的线程数量（在最坏的情况下）而变化，这在程序中可能非常大。
- The memory consumed by a `thread_local` variable scales with the number of running threads (in the worst case), which can be quite large in a program.
- 数据成员不能是 `thread_local` ，除非它们也是 `static` 。
- Data members cannot be `thread_local` unless they are also `static`.
- 如果 `thread_local` 变量具有复杂的析构函数，我们可能会遇到释放后使用错误。特别是，任何此类变量的析构函数不得（传递地）调用任何引用任何可能被破坏的 `thread_local` 的代码。该属性很难执行。
- We may suffer from use-after-free bugs if `thread_local` variables have complex destructors. In particular, the destructor of any such variable must not call any code (transitively) that refers to any potentially-destroyed `thread_local`. This property is hard to enforce.
- 避免全局/静态上下文中释放后使用的方法不适用于 `thread_local` 。具体来说，跳过全局变量和静态变量的析构函数是允许的，因为它们的生命周期在程序关闭时结束。因此，任何 "泄漏" 都会立即由操作系统清理内存和其他资源来管理。相比之下，跳过 `thread_local` 变量的析构函数会导致资源泄漏，该泄漏与程序生命周期内终止的线程总数成正比。
- Approaches for avoiding use-after-free in global/static contexts do not work for `thread_local`s. Specifically, skipping destructors for globals and static variables is allowable because their lifetimes end at program shutdown. Thus, any "leak" is managed immediately by the OS cleaning up our memory and other resources. By contrast, skipping destructors for `thread_local` variables leads to resource leaks proportional to the total number of threads that terminate during the lifetime of the program.

类或命名空间范围内的 `thread_local` 变量必须使用真正的编译时常量进行初始化（即，它们必须没有动态初始化）。为了强制执行此操作，类或命名空间范围内的 `thread_local` 变量必须使用 [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h) 进行注释（或 `constexpr`，但这应该很少见）：
`thread_local` variables at class or namespace scope must be initialized with a true compile-time constant (i.e., they must have no dynamic initialization). To enforce this, `thread_local` variables at class or namespace scope must be annotated with [`ABSL_CONST_INIT`](https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h) (or `constexpr`, but that should be rare):

```C++
  ABSL_CONST_INIT thread_local Foo foo = ...;
```

函数内的 `thread_local` 变量没有初始化问题，但在线程退出期间仍然存在释放后使用的风险。请注意，您可以通过定义公开它的函数或静态方法来使用函数范围 `thread_local` 来模拟类或命名空间范围 `thread_local` ：
`thread_local` variables inside a function have no initialization concerns, but still risk use-after-free during thread exit. Note that you can use a function-scope `thread_local` to simulate a class- or namespace-scope `thread_local` by defining a function or static method that exposes it:

```C++
Foo& MyThreadLocalFoo() {
  thread_local Foo result = ComplicatedInitialization();
  return result;
}
```

请注意，每当线程退出时， `thread_local` 变量都会被销毁。如果任何此类变量的析构函数引用任何其他（可能被破坏的） `thread_local` ，我们将遇到难以诊断释放后使用错误的问题。更喜欢简单类型，或者在销毁时可证明不运行用户提供的代码的类型，以最大程度地减少访问任何其他 `thread_local` 的可能性。
Note that `thread_local` variables will be destroyed whenever a thread exits. If the destructor of any such variable refers to any other (potentially-destroyed) `thread_local` we will suffer from hard to diagnose use-after-free bugs. Prefer trivial types, or types that provably run no user-provided code at destruction to minimize the potential of accessing any other `thread_local`.

`thread_local` 应该优先于定义线程本地数据的其他机制。
`thread_local` should be preferred over other mechanisms for defining thread-local data.

# 类

# Classes

类是 C++ 中代码的基本单元。当然，我们广泛使用它们。本节列出了编写类时应遵循的主要注意事项。
Classes are the fundamental unit of code in C++. Naturally, we use them extensively. This section lists the main dos and don'ts you should follow when writing a class.

## 在构造函数中工作

## Doing Work in Constructors

避免在构造函数中调用虚拟方法，并避免在无法发出错误信号时可能失败的初始化。
Avoid virtual method calls in constructors, and avoid initialization that can fail if you can't signal an error.

可以在构造函数主体中执行任意初始化。
It is possible to perform arbitrary initialization in the body of the constructor.

- 无需担心类是否已初始化。
- No need to worry about whether the class has been initialized or not.
- 通过构造函数调用完全初始化的对象可以是 `const` ，并且也可能更容易与标准容器或算法一起使用。
- Objects that are fully initialized by constructor call can be `const` and may also be easier to use with standard containers or algorithms.

- 如果工作调用虚函数，这些调用将不会被分派到子类实现。即使您的类当前没有子类化，将来对您的类的修改也会悄悄地引入此问题，从而造成很多混乱。
- If the work calls virtual functions, these calls will not get dispatched to the subclass implementations. Future modification to your class can quietly introduce this problem even if your class is not currently subclassed, causing much confusion.
- 构造函数没有简单的方法来发出错误信号，除非使程序崩溃（并不总是合适）或使用异常（_禁止_）。
- There is no easy way for constructors to signal errors, short of crashing the program (not always appropriate) or using exceptions (which are _forbidden_).
- 如果工作失败，我们现在有一个初始化代码失败的对象，因此它可能是一个不寻常的状态，需要 `bool IsValid()` 状态检查机制（或类似的），很容易忘记调用。
- If the work fails, we now have an object whose initialization code failed, so it may be an unusual state requiring a `bool IsValid()` state checking mechanism (or similar) which is easy to forget to call.
- 您无法获取构造函数的地址，因此构造函数中完成的任何工作都不能轻易地传递给其他线程等。
- You cannot take the address of a constructor, so whatever work is done in the constructor cannot easily be handed off to, for example, another thread.

构造函数永远不应该调用虚函数。如果适合您的代码，终止程序可能是适当的错误处理响应。否则，请考虑使用工厂函数或 `Init()` 方法，如 [TotW #42](https://abseil.io/tips/42) 中所述。避免在没有其他状态影响公共方法调用的对象上使用 `Init()` 方法（这种形式的半构造对象特别难以正确使用）。
Constructors should never call virtual functions. If appropriate for your code , terminating the program may be an appropriate error handling response. Otherwise, consider a factory function or `Init()` method as described in [TotW #42](https://abseil.io/tips/42) . Avoid `Init()` methods on objects with no other states that affect which public methods may be called (semi-constructed objects of this form are particularly hard to work with correctly).

## 隐式转换

## Implicit Conversions

不要定义隐式转换。对转换运算符和单参数构造函数使用 `explicit` 关键字。
Do not define implicit conversions. Use the `explicit` keyword for conversion operators and single-argument constructors.

隐式转换允许在需要不同类型（称为目标类型）的地方使用一种类型（称为源类型）的对象，例如将 `int` 参数传递给采用 `double` 参数的函数时。
Implicit conversions allow an object of one type (called the source type) to be used where a different type (called the destination type) is expected, such as when passing an `int` argument to a function that takes a `double` parameter.

除了语言定义的隐式转换之外，用户还可以通过将适当的成员添加到源或目标类型的类定义中来定义自己的隐式转换。源类型中的隐式转换由以目标类型命名的类型转换运算符定义（例如 `operator bool()` ）。目标类型中的隐式转换由构造函数定义，该构造函数可以将源类型作为其唯一参数（或唯一没有默认值的参数）。
In addition to the implicit conversions defined by the language, users can define their own, by adding appropriate members to the class definition of the source or destination type. An implicit conversion in the source type is defined by a type conversion operator named after the destination type (e.g., `operator bool()`). An implicit conversion in the destination type is defined by a constructor that can take the source type as its only argument (or only argument with no default value).

`explicit` 关键字可以应用于构造函数或转换运算符，以确保它只能在使用时目标类型是显式的（例如，通过强制转换）时使用。这不仅适用于隐式转换，还适用于列表初始化语法：
The `explicit` keyword can be applied to a constructor or a conversion operator, to ensure that it can only be used when the destination type is explicit at the point of use, e.g., with a cast. This applies not only to implicit conversions, but to list initialization syntax:

```C++
class Foo {
  explicit Foo(int x, double y);
  ...
};

void Func(Foo f);
```

**badcode**

```C++
Func({42, 3.14});  // Error
```

从技术上讲，这种代码并不是隐式转换，但就 `explicit` 而言，该语言将其视为一种隐式转换。
This kind of code isn't technically an implicit conversion, but the language treats it as one as far as `explicit` is concerned.

- 隐式转换可以消除在明显时显式命名类型的需要，从而使类型更可用且更具表现力。
- Implicit conversions can make a type more usable and expressive by eliminating the need to explicitly name a type when it's obvious.
- 隐式转换可以是重载的更简单替代方案，例如当带有 `string_view` 参数的单个函数取代 `std::string` 和 `const char*` 的单独重载时。
- Implicit conversions can be a simpler alternative to overloading, such as when a single function with a `string_view` parameter takes the place of separate overloads for `std::string` and `const char*`.
- 列表初始化语法是一种简洁且富有表现力的初始化对象的方式。
- List initialization syntax is a concise and expressive way of initializing objects.

- 隐式转换可以隐藏类型不匹配的错误，其中目标类型与用户的期望不匹配，或者用户不知道将发生任何转换。
- Implicit conversions can hide type-mismatch bugs, where the destination type does not match the user's expectation, or the user is unaware that any conversion will take place.
- 隐式转换会使代码更难阅读，特别是在存在重载的情况下，因为它使实际调用的代码变得不那么明显。
- Implicit conversions can make code harder to read, particularly in the presence of overloading, by making it less obvious what code is actually getting called.
- 采用单个参数的构造函数可能会意外地用作隐式类型转换，即使它们无意这样做。
- Constructors that take a single argument may accidentally be usable as implicit type conversions, even if they are not intended to do so.
- 当单参数构造函数未标记为 `explicit` 时，没有可靠的方法来判断它是否打算定义隐式转换，或者作者只是忘记标记它。
- When a single-argument constructor is not marked `explicit`, there's no reliable way to tell whether it's intended to define an implicit conversion, or the author simply forgot to mark it.
- 隐式转换可能会导致调用点歧义，尤其是当存在双向隐式转换时。这可能是由于两种类型都提供隐式转换造成的，也可能是由于单一类型同时具有隐式构造函数和隐式类型转换运算符造成的。
- Implicit conversions can lead to call-site ambiguities, especially when there are bidirectional implicit conversions. This can be caused either by having two types that both provide an implicit conversion, or by a single type that has both an implicit constructor and an implicit type conversion operator.
- 如果目标类型是隐式的，列表初始化可能会遇到同样的问题，特别是如果列表只有一个元素。
- List initialization can suffer from the same problems if the destination type is implicit, particularly if the list has only a single element.

类型转换运算符和可使用单个参数调用的构造函数必须在类定义中标记为 `explicit` 。作为例外，复制和移动构造函数不应该是 `explicit` ，因为它们不执行类型转换。
Type conversion operators, and constructors that are callable with a single argument, must be marked `explicit` in the class definition. As an exception, copy and move constructors should not be `explicit`, since they do not perform type conversion.

对于设计为可互换的类型，隐式转换有时是必要且适当的，例如，当两种类型的对象只是同一基础值的不同表示时。在这种情况下，请联系您的项目负责人请求豁免此规则。
Implicit conversions can sometimes be necessary and appropriate for types that are designed to be interchangeable, for example when objects of two types are just different representations of the same underlying value. In that case, contact your project leads to request a waiver of this rule.

不能使用单个参数调用的构造函数可能会省略 `explicit` 。采用单个 `std::initializer_list` 参数的构造函数也应该省略 `explicit`，以支持复制初始化（例如，`MyType m = {1, 2};`）。
Constructors that cannot be called with a single argument may omit `explicit`. Constructors that take a single `std::initializer_list` parameter should also omit `explicit`, in order to support copy-initialization (e.g., `MyType m = {1, 2};`).

## 可复制和可移动类型

## Copyable and Movable Types

类的公共 API 必须明确该类是可复制、只能移动还是既不可复制又不可移动。如果复制和/或移动这些操作对于您的类型来说清晰且有意义，则支持这些操作。
A class's public API must make clear whether the class is copyable, move-only, or neither copyable nor movable. Support copying and/or moving if these operations are clear and meaningful for your type.

可移动类型是一种可以从临时类型初始化和分配的类型。
A movable type is one that can be initialized and assigned from temporaries.

可复制类型是一种可以从相同类型的任何其他对象初始化或分配的类型（因此根据定义也可以移动），前提是源的值不更改。 `std::unique_ptr<int>` 是可移动但不可复制类型的示例（因为源 `std::unique_ptr<int>` 的值必须在分配给目标期间修改）。 `int` 和 `std::string` 是可复制的可移动类型的示例。 （对于 `int` ，移动和复制操作是相同的；对于 `std::string` ，存在比复制更便宜的移动操作。）
A copyable type is one that can be initialized or assigned from any other object of the same type (so is also movable by definition), with the stipulation that the value of the source does not change. `std::unique_ptr<int>` is an example of a movable but not copyable type (since the value of the source `std::unique_ptr<int>` must be modified during assignment to the destination). `int` and `std::string` are examples of movable types that are also copyable. (For `int`, the move and copy operations are the same; for `std::string`, there exists a move operation that is less expensive than a copy.)

对于用户定义类型，复制行为由复制构造函数和复制赋值运算符定义。移动行为由移动构造函数和移动赋值运算符（如果存在）定义，否则由复制构造函数和复制赋值运算符定义。
For user-defined types, the copy behavior is defined by the copy constructor and the copy-assignment operator. Move behavior is defined by the move constructor and the move-assignment operator, if they exist, or by the copy constructor and the copy-assignment operator otherwise.

在某些情况下，例如，按值传递对象时，编译器可以隐式调用复制/移动构造函数。
The copy/move constructors can be implicitly invoked by the compiler in some situations, e.g., when passing objects by value.

可复制和可移动类型的对象可以按值传递和返回，这使得 API 更简单、更安全、更通用。与通过指针或引用传递对象不同，不存在所有权、生命周期、可变性和类似问题的混淆风险，并且无需在合约中指定它们。它还可以防止客户端和实现之间的非本地交互，这使得编译器更容易理解、维护和优化它们。此外，此类对象可以与需要按值传递的通用 API（例如大多数容器）一起使用，并且它们允许在类型组合等方面提供额外的灵活性。
Objects of copyable and movable types can be passed and returned by value, which makes APIs simpler, safer, and more general. Unlike when passing objects by pointer or reference, there's no risk of confusion over ownership, lifetime, mutability, and similar issues, and no need to specify them in the contract. It also prevents non-local interactions between the client and the implementation, which makes them easier to understand, maintain, and optimize by the compiler. Further, such objects can be used with generic APIs that require pass-by-value, such as most containers, and they allow for additional flexibility in e.g., type composition.

复制/移动构造函数和赋值运算符通常比 `Clone()` 、 `CopyFrom()` 或 `Swap()` 等替代方法更容易正确定义，因为它们可以由编译器隐式或使用 `= default` 生成。它们很简洁，并确保复制所有数据成员。复制和移动构造函数通常也更高效，因为它们不需要堆分配或单独的初始化和赋值步骤，并且它们有资格进行优化，例如[复制省略](http://en.cppreference.com/w/cpp/language/copy_elision) 。
Copy/move constructors and assignment operators are usually easier to define correctly than alternatives like `Clone()`, `CopyFrom()` or `Swap()`, because they can be generated by the compiler, either implicitly or with `= default`. They are concise, and ensure that all data members are copied. Copy and move constructors are also generally more efficient, because they don't require heap allocation or separate initialization and assignment steps, and they're eligible for optimizations such as [copy elision](http://en.cppreference.com/w/cpp/language/copy_elision).

移动操作允许从右值对象隐式且高效地转移资源。在某些情况下，这允许更简单的编码风格。
Move operations allow the implicit and efficient transfer of resources out of rvalue objects. This allows a plainer coding style in some cases.

某些类型不需要是可复制的，并且为此类类型提供复制操作可能会令人困惑、无意义或完全不正确。表示单例对象（ `Registerer` ）、与特定范围相关的对象（ `Cleanup` ）或与对象标识紧密耦合（ `Mutex` ）的类型无法进行有意义的复制。要以多态方式使用的基类类型的复制操作是危险的，因为使用它们可能会导致[对象切片](https://en.wikipedia.org/wiki/Object_slicing)。默认或粗心实施的复制操作可能不正确，并且产生的错误可能令人困惑且难以诊断。
Some types do not need to be copyable, and providing copy operations for such types can be confusing, nonsensical, or outright incorrect. Types representing singleton objects (`Registerer`), objects tied to a specific scope (`Cleanup`), or closely coupled to object identity (`Mutex`) cannot be copied meaningfully. Copy operations for base class types that are to be used polymorphically are hazardous, because use of them can lead to [object slicing](https://en.wikipedia.org/wiki/Object_slicing). Defaulted or carelessly-implemented copy operations can be incorrect, and the resulting bugs can be confusing and difficult to diagnose.

复制构造函数是隐式调用的，这使得调用很容易被错过。对于习惯使用传统或强制传递引用的语言的程序员来说，这可能会造成混乱。它还可能鼓励过度复制，从而导致性能问题。
Copy constructors are invoked implicitly, which makes the invocation easy to miss. This may cause confusion for programmers used to languages where pass-by-reference is conventional or mandatory. It may also encourage excessive copying, which can cause performance problems.

每个类的公共接口必须明确该类支持哪些复制和移动操作。这通常应采取在声明的 `public` 部分中显式声明和/或删除适当操作的形式。
Every class's public interface must make clear which copy and move operations the class supports. This should usually take the form of explicitly declaring and/or deleting the appropriate operations in the `public` section of the declaration.

具体来说，可复制类应显式声明复制操作，仅移动类应显式声明移动操作，不可复制/可移动类应显式删除复制操作。可复制类还可以声明移动操作以支持高效移动。允许显式声明或删除所有四个复制/移动操作，但不是必需的。如果您提供复制或移动赋值运算符，则还必须提供相应的构造函数。
Specifically, a copyable class should explicitly declare the copy operations, a move-only class should explicitly declare the move operations, and a non-copyable/movable class should explicitly delete the copy operations. A copyable class may also declare move operations in order to support efficient moves. Explicitly declaring or deleting all four copy/move operations is permitted, but not required. If you provide a copy or move assignment operator, you must also provide the corresponding constructor.

```C++
class Copyable {
 public:
  Copyable(const Copyable& other) = default;
  Copyable& operator=(const Copyable& other) = default;

  // The implicit move operations are suppressed by the declarations above.
  // You may explicitly declare move operations to support efficient moves.
};

class MoveOnly {
 public:
  MoveOnly(MoveOnly&& other) = default;
  MoveOnly& operator=(MoveOnly&& other) = default;

  // The copy operations are implicitly deleted, but you can
  // spell that out explicitly if you want:
  MoveOnly(const MoveOnly&) = delete;
  MoveOnly& operator=(const MoveOnly&) = delete;
};

class NotCopyableOrMovable {
 public:
  // Not copyable or movable
  NotCopyableOrMovable(const NotCopyableOrMovable&) = delete;
  NotCopyableOrMovable& operator=(const NotCopyableOrMovable&)
      = delete;

  // The move operations are implicitly disabled, but you can
  // spell that out explicitly if you want:
  NotCopyableOrMovable(NotCopyableOrMovable&&) = delete;
  NotCopyableOrMovable& operator=(NotCopyableOrMovable&&)
      = delete;
};
```

这些声明/删除只有在显而易见时才可以省略：
These declarations/deletions can be omitted only if they are obvious:

- 如果类没有 `private` 部分，例如 [struct](#Structs_vs._Classes) 或仅接口基类，则可复制性/可移动性可以由任何公共数据成员的可复制性/可移动性来确定。
- If the class has no `private` section, like a [struct](#Structs_vs._Classes) or an interface-only base class, then the copyability/movability can be determined by the copyability/movability of any public data members.
- 如果基类显然不可复制或可移动，则派生类自然也不会。将这些操作隐式保留的仅接口基类不足以使具体子类变得清晰。
- If a base class clearly isn't copyable or movable, derived classes naturally won't be either. An interface-only base class that leaves these operations implicit is not sufficient to make concrete subclasses clear.
- 请注意，如果显式声明或删除复制的构造函数或赋值操作，则另一个复制操作并不明显，必须声明或删除。对于移动操作也是如此。
- Note that if you explicitly declare or delete either the constructor or assignment operation for copy, the other copy operation is not obvious and must be declared or deleted. Likewise for move operations.

如果临时用户不清楚复制/移动的含义，或者会产生意外成本，则类型不应是可复制/移动的。可复制类型的移动操作严格来说是一种性能优化，并且是错误和复杂性的潜在来源，因此请避免定义它们，除非它们比相应的复制操作更有效。如果您的类型提供复制操作，建议您设计您的类，以便这些操作的默认实现是正确的。请记住像检查任何其他代码一样检查任何默认操作的正确性。
A type should not be copyable/movable if the meaning of copying/moving is unclear to a casual user, or if it incurs unexpected costs. Move operations for copyable types are strictly a performance optimization and are a potential source of bugs and complexity, so avoid defining them unless they are significantly more efficient than the corresponding copy operations. If your type provides copy operations, it is recommended that you design your class so that the default implementation of those operations is correct. Remember to review the correctness of any defaulted operations as you would any other code.

为了消除切片的风险，最好将基类抽象化，方法是保护它们的构造函数、声明它们的析构函数受保护，或者为它们提供一个或多个纯虚拟成员函数。最好避免从具体类派生。
To eliminate the risk of slicing, prefer to make base classes abstract, by making their constructors protected, by declaring their destructors protected, or by giving them one or more pure virtual member functions. Prefer to avoid deriving from concrete classes.

## 结构与类

## Structs vs. Classes

仅对携带数据的被动对象使用 `struct` ；其他一切都是一个 `class` 。
Use a `struct` only for passive objects that carry data; everything else is a `class`.

`struct` 和 `class` 关键字在 C++ 中的行为几乎相同。我们为每个关键字添加我们自己的语义，因此您应该为您定义的数据类型使用适当的关键字。
The `struct` and `class` keywords behave almost identically in C++. We add our own semantic meanings to each keyword, so you should use the appropriate keyword for the data-type you're defining.

`struct` 应用于携带数据的被动对象，并且可能具有关联的常量。所有字段都必须是公共的。该结构不得具有暗示不同字段之间关系的不变量，因为用户直接访问这些字段可能会破坏这些不变量。可能存在构造函数、析构函数和辅助方法；但是，这些方法不得要求或强制执行任何不变量。
`structs` should be used for passive objects that carry data, and may have associated constants. All fields must be public. The struct must not have invariants that imply relationships between different fields, since direct user access to those fields may break those invariants. Constructors, destructors, and helper methods may be present; however, these methods must not require or enforce any invariants.

如果需要更多函数或不变量， `class` 更合适。如果有疑问，请将其设为 `class` 。
If more functionality or invariants are required, a `class` is more appropriate. If in doubt, make it a `class`.

为了与 STL 保持一致，您可以对无状态类型使用 `struct` 而不是 `class` ，例如特征、*模板元函数*和一些函子。
For consistency with STL, you can use `struct` instead of `class` for stateless types, such as traits, _template metafunctions_, and some functors.

请注意，结构和类中的成员变量具有*不同的命名规则*。
Note that member variables in structs and classes have _different naming rules_.

## 结构体与对和元组

## Structs vs. Pairs and Tuples

只要元素可以具有有意义的名称，就更愿意使用 `struct` 而不是一对或元组。
Prefer to use a `struct` instead of a pair or a tuple whenever the elements can have meaningful names.

虽然使用对和元组可以避免定义自定义类型的需要，可能会在编写代码时节省工作量，但在读取代码时，有意义的字段名称几乎总是比 `.first` 、 `.second` 或 `std::get<X>` 更清晰。虽然 C++14 引入 `std::get<Type>` 通过类型而不是索引（当类型唯一时）访问元组元素有时可以部分缓解这种情况，但字段名称通常比类型更清晰、信息更丰富。
While using pairs and tuples can avoid the need to define a custom type, potentially saving work when _writing_ code, a meaningful field name will almost always be much clearer when _reading_ code than `.first`, `.second`, or `std::get<X>`. While C++14's introduction of `std::get<Type>` to access a tuple element by type rather than index (when the type is unique) can sometimes partially mitigate this, a field name is usually substantially clearer and more informative than a type.

对和元组可能适用于通用代码，其中对或元组的元素没有特定含义。为了与现有代码或 API 进行互操作，也可能需要使用它们。
Pairs and tuples may be appropriate in generic code where there are not specific meanings for the elements of the pair or tuple. Their use may also be required in order to interoperate with existing code or APIs.

## 继承

## Inheritance

组合通常比继承更合适。使用继承时，请将其设置为 `public` 。
Composition is often more appropriate than inheritance. When using inheritance, make it `public`.

当子类继承基类时，它包含基类定义的所有数据和操作的定义。 "接口继承" 是从纯抽象基类（没有状态或定义的方法）继承；所有其他继承都是 "实现继承" 。
When a sub-class inherits from a base class, it includes the definitions of all the data and operations that the base class defines. "Interface inheritance" is inheritance from a pure abstract base class (one with no state or defined methods); all other inheritance is "implementation inheritance".

实现继承通过重用基类代码来减少代码大小，因为它专用于现有类型。因为继承是编译时声明，所以您和编译器可以理解操作并检测错误。接口继承可用于以编程方式强制类公开特定的 API。同样，在本例中，当类未定义 API 的必要方法时，编译器可以检测到错误。
Implementation inheritance reduces code size by re-using the base class code as it specializes an existing type. Because inheritance is a compile-time declaration, you and the compiler can understand the operation and detect errors. Interface inheritance can be used to programmatically enforce that a class expose a particular API. Again, the compiler can detect errors, in this case, when a class does not define a necessary method of the API.

对于实现继承，由于实现子类的代码分布在基类和子类之间，因此理解实现可能会更加困难。子类不能重写非虚拟函数，因此子类不能更改实现。
For implementation inheritance, because the code implementing a sub-class is spread between the base and the sub-class, it can be more difficult to understand an implementation. The sub-class cannot override functions that are not virtual, so the sub-class cannot change implementation.

多重继承尤其成问题，因为它常常会带来更高的性能开销（事实上，从单继承到多重继承的性能下降往往会大于从普通分派到虚拟分派的性能下降），而且因为它有导致 "菱形" 的风险。 `继承模式很容易出现歧义、混乱和彻底的错误。
Multiple inheritance is especially problematic, because it often imposes a higher performance overhead (in fact, the performance drop from single inheritance to multiple inheritance can often be greater than the performance drop from ordinary to virtual dispatch), and because it risks leading to "diamond" inheritance patterns, which are prone to ambiguity, confusion, and outright bugs.

所有继承都应该是 `public` 。如果您想进行私有继承，则应该将基类的实例作为成员包含在内。当您不打算支持将类用作基类时，您可以在类上使用 `final` 。
All inheritance should be `public`. If you want to do private inheritance, you should be including an instance of the base class as a member instead. You may use `final` on classes when you don't intend to support using them as base classes.

不要过度使用实现继承。构图往往更合适。尝试将继承的使用限制在 `is-a` 的情况下：如果可以合理地说 `Bar` "是一种" `Foo` ，则 `Bar` 是 `Foo` 的子类。
Do not overuse implementation inheritance. Composition is often more appropriate. Try to restrict use of inheritance to the "is-a" case: `Bar` subclasses `Foo` if it can reasonably be said that `Bar` "is a kind of" `Foo`.

将 `protected` 的使用限制为那些可能需要从子类访问的成员函数。请注意，[数据成员应该是 `private`](#Access_Control)。
Limit the use of `protected` to those member functions that might need to be accessed from subclasses. Note that [data members should be `private`](#Access_Control).

使用 `override` 或（不太频繁） `final` 说明符之一明确注释虚拟函数或虚拟析构函数的覆盖。声明覆盖时不要使用 `virtual` 。基本原理：标记为 `override` 或 `final` 的函数或析构函数不是基类虚函数的重写，将无法编译，这有助于捕获常见错误。说明符充当文档；如果不存在说明符，则读者必须检查相关类的所有祖先以确定该函数或析构函数是否是虚拟的。
Explicitly annotate overrides of virtual functions or virtual destructors with exactly one of an `override` or (less frequently) `final` specifier. Do not use `virtual` when declaring an override. Rationale: A function or destructor marked `override` or `final` that is not an override of a base class virtual function will not compile, and this helps catch common errors. The specifiers serve as documentation; if no specifier is present, the reader has to check all ancestors of the class in question to determine if the function or destructor is virtual or not.

允许多重继承，但强烈建议不要多重*实现*继承。
Multiple inheritance is permitted, but multiple _implementation_ inheritance is strongly discouraged.

## 运算符重载

## Operator Overloading

明智地重载运算符。不要使用用户定义的字面量。
Overload operators judiciously. Do not use user-defined literals.

C++ 允许用户代码使用 `operator` 关键字[声明内置运算符的重载版本](http://en.cppreference.com/w/cpp/language/operators)，只要参数之一是用户定义的类型。 `operator` 关键字还允许用户代码使用 `operator""` 定义新类型的字面量，并定义类型转换函数，例如 `operator bool()`。
C++ permits user code to [declare overloaded versions of the built-in operators](http://en.cppreference.com/w/cpp/language/operators) using the `operator` keyword, so long as one of the parameters is a user-defined type. The `operator` keyword also permits user code to define new kinds of literals using `operator""`, and to define type-conversion functions such as `operator bool()`.

运算符重载可以使用户定义类型的行为与内置类型相同，从而使代码更加简洁和直观。重载运算符是某些操作的惯用名称（例如 `==` 、 `<` 、 `=` 和 `<<` ），遵守这些约定可以使用户定义的类型更具可读性并使它们能够互操作与期望这些名称的库。
Operator overloading can make code more concise and intuitive by enabling user-defined types to behave the same as built-in types. Overloaded operators are the idiomatic names for certain operations (e.g., `==`, `<`, `=`, and `<<`), and adhering to those conventions can make user-defined types more readable and enable them to interoperate with libraries that expect those names.

用户定义的字面量是用于创建用户定义类型的对象的非常简洁的表示法。
User-defined literals are a very concise notation for creating objects of user-defined types.

- 提供一组正确、一致且不出意外的操作符重载需要小心谨慎，否则可能会导致混乱和错误。
- Providing a correct, consistent, and unsurprising set of operator overloads requires some care, and failure to do so can lead to confusion and bugs.
- 过度使用运算符可能会导致代码混乱，特别是在重载运算符的语义不遵循约定的情况下。
- Overuse of operators can lead to obfuscated code, particularly if the overloaded operator's semantics don't follow convention.
- 函数重载的危险与运算符重载同样存在，甚至更严重。
- The hazards of function overloading apply just as much to operator overloading, if not more so.
- 运算符重载可能会欺骗我们的直觉，让我们认为昂贵的操作是廉价的内置操作。
- Operator overloads can fool our intuition into thinking that expensive operations are cheap, built-in operations.
- 查找重载运算符的调用站点可能需要一个了解 C++ 语法的搜索工具，而不是 grep 等。
- Finding the call sites for overloaded operators may require a search tool that's aware of C++ syntax, rather than e.g., grep.
- 如果重载运算符的参数类型错误，您可能会得到不同的重载而不是编译器错误。例如， `foo < bar` 可能做一件事，而 `&foo < &bar` 则做完全不同的事情。
- If you get the argument type of an overloaded operator wrong, you may get a different overload rather than a compiler error. For example, `foo < bar` may do one thing, while `&foo < &bar` does something totally different.
- 某些运算符过载本质上是危险的。重载一元 `&` 可能会导致相同的代码具有不同的含义，具体取决于重载声明是否可见。 `&&`、`||` 和 `,`（逗号）的重载无法匹配内置运算符的求值顺序语义。
- Certain operator overloads are inherently hazardous. Overloading unary `&` can cause the same code to have different meanings depending on whether the overload declaration is visible. Overloads of `&&`, `||`, and `,` (comma) cannot match the evaluation-order semantics of the built-in operators.
- 运算符通常在类之外定义，因此存在不同文件引入同一运算符的不同定义的风险。如果两个定义都链接到同一个二进制文件中，则会导致未定义的行为，这可能表现为微妙的运行时错误。
- Operators are often defined outside the class, so there's a risk of different files introducing different definitions of the same operator. If both definitions are linked into the same binary, this results in undefined behavior, which can manifest as subtle run-time bugs.
- 用户定义的字面量 (UDL) 允许创建即使是经验丰富的 C++ 程序员也不熟悉的新语法形式，例如作为 `std::string_view("Hello World")` 的简写的 `"Hello World"sv`。现有的符号更清晰，但不太简洁。
- User-defined literals (UDLs) allow the creation of new syntactic forms that are unfamiliar even to experienced C++ programmers, such as `"Hello World"sv` as a shorthand for `std::string_view("Hello World")`. Existing notations are clearer, though less terse.
- 因为它们不能是命名空间限定的，所以 UDL 的使用还需要使用 using 指令（_我们禁止_）或 using-声明（_我们在头文件中禁止_，除非导入的名称是由有问题的头文件）。鉴于头文件必须避免 UDL 后缀，我们更愿意避免头文件和源文件之间的字面量约定不同。
- Because they can't be namespace-qualified, uses of UDLs also require use of either using-directives (which _we ban_) or using-declarations (which _we ban in header files_ except when the imported names are part of the interface exposed by the header file in question). Given that header files would have to avoid UDL suffixes, we prefer to avoid having conventions for literals differ between header files and source files.

仅当重载运算符的含义显而易见、不令人意外且与相应的内置运算符一致时，才定义它们。例如，使用 `|` 作为按位或逻辑或，而不是作为外壳式管道。
Define overloaded operators only if their meaning is obvious, unsurprising, and consistent with the corresponding built-in operators. For example, use `|` as a bitwise- or logical-or, not as a shell-style pipe.

仅在您自己的类型上定义运算符。更准确地说，将它们定义在与它们所操作的类型相同的标头、 `.cc` 文件和命名空间中。这样，无论类型在哪里，运算符都可用，从而最大限度地降低了多重定义的风险。如果可能，请避免将运算符定义为模板，因为它们必须对于任何可能的模板参数满足此规则。如果您定义了运算符，请同时定义任何有意义的相关运算符，并确保它们的定义一致。例如，如果您重载 `<` ，请重载所有比较运算符，并确保 `<` 和 `>` 对于相同的参数永远不会返回 true。
Define operators only on your own types. More precisely, define them in the same headers, `.cc` files, and namespaces as the types they operate on. That way, the operators are available wherever the type is, minimizing the risk of multiple definitions. If possible, avoid defining operators as templates, because they must satisfy this rule for any possible template arguments. If you define an operator, also define any related operators that make sense, and make sure they are defined consistently. For example, if you overload `<`, overload all the comparison operators, and make sure `<` and `>` never return true for the same arguments.

更喜欢将非修改二元运算符定义为非成员函数。如果将二元运算符定义为类成员，则隐式转换将应用于右侧参数，但不适用于左侧参数。如果 `a < b` 编译但 `b < a` 不编译，这会让你的用户感到困惑。
Prefer to define non-modifying binary operators as non-member functions. If a binary operator is defined as a class member, implicit conversions will apply to the right-hand argument, but not the left-hand one. It will confuse your users if `a < b` compiles but `b < a` doesn't.

不要竭尽全力避免定义运算符重载。例如，更喜欢定义 `==` 、 `=` 和 `<<` ，而不是 `Equals()` 、 `CopyFrom()` 和 `PrintTo()` 。相反，不要仅仅因为其他库需要它们就定义运算符重载。例如，如果您的类型没有自然顺序，但您想将其存储在 `std::set` 中，请使用自定义比较器而不是重载 `<` 。
Don't go out of your way to avoid defining operator overloads. For example, prefer to define `==`, `=`, and `<<`, rather than `Equals()`, `CopyFrom()`, and `PrintTo()`. Conversely, don't define operator overloads just because other libraries expect them. For example, if your type doesn't have a natural ordering, but you want to store it in a `std::set`, use a custom comparator rather than overloading `<`.

不要重载 `&&`、`||`、`,`（逗号）或一元 `&`。不要重载`operator""`，即不要引入用户定义的字面量。不要使用其他人（包括标准库）提供的任何此类字面量。
Do not overload `&&`, `||`, `,` (comma), or unary `&`. Do not overload `operator""`, i.e., do not introduce user-defined literals. Do not use any such literals provided by others (including the standard library).

*隐式转换*部分介绍了类型转换运算符。 `=` 运算符在 `复制构造函数` 部分中进行了介绍。 _streams_ 部分介绍了重载 `<<` 以与流一起使用。另请参阅*函数重载*的规则，这些规则也适用于运算符重载。
Type conversion operators are covered in the section on _implicit conversions_. The `=` operator is covered in the section on _copy constructors_. Overloading `<<` for use with streams is covered in the section on _streams_. See also the rules on _function overloading_, which apply to operator overloading as well.

## 访问控制

## Access Control

将类的数据成员设置为 `private` ，除非它们是*常量*。这简化了关于不变量的推理，但如果需要的话，代价是一些以访问器（通常是 `const` ）形式的简单样板。
Make classes' data members `private`, unless they are _constants_. This simplifies reasoning about invariants, at the cost of some easy boilerplate in the form of accessors (usually `const`) if necessary.

出于技术原因，我们允许在使用 [Google Test](https://github.com/google/googletest) 时 `cc` 文件中定义的测试夹具类的数据成员为 `protected` 。如果测试装置类是在使用它的 `.cc` 文件之外定义的，例如在 `.h` 文件中，请将数据成员设置为 `private` 。
For technical reasons, we allow data members of a test fixture class defined in a `.cc` file to be `protected` when using [Google Test](https://github.com/google/googletest). If a test fixture class is defined outside of the `.cc` file it is used in, for example in a `.h` file, make data members `private`.

## 声明顺序

## Declaration Order

将类似的声明组合在一起，将 `public` 部分放在前面。
Group similar declarations together, placing `public` parts earlier.

类定义通常应以 `public:` 部分开头，然后是 `protected:` ，然后是 `private:` 。省略空的部分。
A class definition should usually start with a `public:` section, followed by `protected:`, then `private:`. Omit sections that would be empty.

在每个部分中，更喜欢将类似类型的声明分组在一起，并且更喜欢以下顺序：

1. 类型和类型别名（`typedef`、`using`、`enum`、嵌套结构和类以及 `friend` 类型）
2. （可选，仅适用于结构）非 `static` 数据成员
3. 静态常量
4. 工厂函数
5. 构造函数和赋值运算符
6. 析构函数
7. 所有其他函数（`static`和非`static`成员函数，以及`friend`函数）
8. 所有其他数据成员（静态和非静态）

Within each section, prefer grouping similar kinds of declarations together, and prefer the following order:

1.  Types and type aliases (`typedef`, `using`, `enum`, nested structs and classes, and `friend` types)
2.  (Optionally, for structs only) non-`static` data members
3.  Static constants
4.  Factory functions
5.  Constructors and assignment operators
6.  Destructor
7.  All other functions (`static` and non-`static` member functions, and `friend` functions)
8.  All other data members (static and non-static)

不要将大型方法定义内联到类定义中。通常，只有琐碎或性能关键且非常短的方法可以内联定义。有关更多详细信息，请参阅*内联函数*。
Do not put large method definitions inline in the class definition. Usually, only trivial or performance-critical, and very short, methods may be defined inline. See _Inline Functions_ for more details.

# 函数

# Functions

## 输入和输出

## Inputs and Outputs

C++ 函数的输出自然是通过返回值提供的，有时也通过输出参数（或输入/输出参数）提供。
The output of a C++ function is naturally provided via a return value and sometimes via output parameters (or in/out parameters).

优先使用返回值而不是输出参数：它们提高了可读性，并且通常提供相同或更好的性能。
Prefer using return values over output parameters: they improve readability, and often provide the same or better performance.

更喜欢按值返回，或者如果失败，则按引用返回。避免返回指针，除非它可以为空。
Prefer to return by value or, failing that, return by reference. Avoid returning a pointer unless it can be null.

参数可以是函数的输入、函数的输出或两者。非可选输入参数通常应该是值或 `const` 引用，而非可选输出和输入/输出参数通常应该是引用（不能为空）。通常，使用 `std::optional` 来表示可选的按值输入，并在非可选形式使用引用时使用 `const` 指针。使用非 const 指针来表示可选输出和可选输入/输出参数。
Parameters are either inputs to the function, outputs from the function, or both. Non-optional input parameters should usually be values or `const` references, while non-optional output and input/output parameters should usually be references (which cannot be null). Generally, use `std::optional` to represent optional by-value inputs, and use a `const` pointer when the non-optional form would have used a reference. Use non-`const` pointers to represent optional outputs and optional input/output parameters.

避免定义需要 `const` 引用参数才能比调用更有效的函数，因为 `const` 引用参数绑定到临时变量。相反，找到一种方法来消除生命周期要求（例如，通过复制参数），或者通过 `const` 指针传递它并记录生命周期和非空要求。
Avoid defining functions that require a `const` reference parameter to outlive the call, because `const` reference parameters bind to temporaries. Instead, find a way to eliminate the lifetime requirement (for example, by copying the parameter), or pass it by `const` pointer and document the lifetime and non-null requirements.

对函数参数进行排序时，请将所有仅输入参数放在任何输出参数之前。特别是，不要仅仅因为新参数是新的就在函数末尾添加新参数；将新的仅输入参数放在输出参数之前。这不是硬性规定。输入和输出参数都会使水变得浑浊，并且与往常一样，与相关函数的一致性可能需要您改变规则。可变参数函数也可能需要不寻常的参数排序。
When ordering function parameters, put all input-only parameters before any output parameters. In particular, do not add new parameters to the end of the function just because they are new; place new input-only parameters before the output parameters. This is not a hard-and-fast rule. Parameters that are both input and output muddy the waters, and, as always, consistency with related functions may require you to bend the rule. Variadic functions may also require unusual parameter ordering.

## 编写短函数

## Write Short Functions

喜欢小型且集中的函数。
Prefer small and focused functions.

我们认识到长函数有时是合适的，因此对函数长度没有硬性限制。如果一个函数超过大约 40 行，请考虑是否可以在不损害程序结构的情况下将其分解。
We recognize that long functions are sometimes appropriate, so no hard limit is placed on functions length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

即使你的长函数现在工作得很好，几个月后有人修改它可能会添加新的行为。这可能会导致难以发现的错误。保持函数简短，可以让其他人更轻松地阅读和修改您的代码。小函数也更容易测试。
Even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code. Small functions are also easier to test.

在使用某些代码时，您可能会发现又长又复杂的函数。不要被修改现有代码吓倒：如果使用这样的函数被证明很困难，您发现错误很难调试，或者您想在多个不同的上下文中使用其中的一部分，请考虑将该函数分解为更小的函数以及更易于管理的部分。
You could find long and complicated functions when working with some code. Do not be intimidated by modifying existing code: if working with such a function proves to be difficult, you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.

## 函数重载

## Function Overloading

仅当查看调用站点的读者可以很好地了解正在发生的情况而不必首先弄清楚正在调用哪个重载时，才使用重载函数（包括构造函数）。
Use overloaded functions (including constructors) only if a reader looking at a call site can get a good idea of what is happening without having to first figure out exactly which overload is being called.

您可以编写一个采用 `const std::string&` 的函数，并用另一个采用 `const char*` 的函数重载它。但是，在这种情况下，请考虑使用 `std::string_view` 。
You may write a function that takes a `const std::string&` and overload it with another that takes `const char*`. However, in this case consider `std::string_view` instead.

```C++
class MyClass {
 public:
  void Analyze(const std::string &text);
  void Analyze(const char *text, size_t textlen);
};
```

重载允许同名函数采用不同的参数，从而使代码更加直观。对于模板化代码来说可能是必需的，并且对于访问者来说可能很方便。
Overloading can make code more intuitive by allowing an identically-named function to take different arguments. It may be necessary for templatized code, and it can be convenient for Visitors.

基于 `const` 或 ref 限定的重载可能会使实用程序代码更可用、更高效，或两者兼而有之。 （有关更多信息，请参阅 [TotW 148](http://abseil.io/tips/148)。）
Overloading based on `const` or ref qualification may make utility code more usable, more efficient, or both. (See [TotW 148](http://abseil.io/tips/148) for more.)

如果函数仅由参数类型重载，读者可能必须了解 C++ 复杂的匹配规则才能知道发生了什么。如果派生类仅覆盖函数的某些变体，许多人也会对继承的语义感到困惑。
If a function is overloaded by the argument types alone, a reader may have to understand C++'s complex matching rules in order to tell what's going on. Also many people are confused by the semantics of inheritance if a derived class overrides only some of the variants of a function.

当变体之间不存在语义差异时，您可以重载函数。这些重载的类型、限定符或参数计数可能有所不同。但是，此类调用的读者不必知道选择了重载集中的哪个成员，只需知道正在调用该集中的**某些内容**即可。如果您可以使用标题中的单个注释来记录重载集中的所有条目，那么这是一个很好的迹象，表明它是一个设计良好的重载集。
You may overload a function when there are no semantic differences between variants. These overloads may vary in types, qualifiers, or argument count. However, a reader of such a call must not need to know which member of the overload set is chosen, only that **something** from the set is being called. If you can document all entries in the overload set with a single comment in the header, that is a good sign that it is a well-designed overload set.

## 默认参数

## Default Arguments

当保证默认值始终具有相同的值时，非虚函数上允许使用默认参数。遵循与*函数重载*相同的限制，并且如果使用默认参数获得的可读性不超过下面的缺点，则更喜欢重载函数。
Default arguments are allowed on non-virtual functions when the default is guaranteed to always have the same value. Follow the same restrictions as for _function overloading_, and prefer overloaded functions if the readability gained with default arguments doesn't outweigh the downsides below.

通常您有一个使用默认值的函数，但有时您想覆盖默认值。默认参数提供了一种简单的方法来做到这一点，而无需为罕见的异常定义许多函数。与重载函数相比，默认参数具有更清晰的语法、更少的样板文件以及 `必需` 和 `可选` 参数之间更清晰的区别。
Often you have a function that uses default values, but occasionally you want to override the defaults. Default parameters allow an easy way to do this without having to define many functions for the rare exceptions. Compared to overloading the function, default arguments have a cleaner syntax, with less boilerplate and a clearer distinction between 'required' and 'optional' arguments.

默认参数是实现重载函数语义的另一种方法，因此所有不重载函数的原因都适用。
Defaulted arguments are another way to achieve the semantics of overloaded functions, so all the _reasons not to overload functions_ apply.

虚拟函数调用中参数的默认值由目标对象的静态类型确定，并且不能保证给定函数的所有重写都声明相同的默认值。
The defaults for arguments in a virtual function call are determined by the static type of the target object, and there's no guarantee that all overrides of a given function declare the same defaults.

默认参数会在每个调用站点重新计算，这会使生成的代码变得臃肿。读者可能还期望默认值在声明时固定，而不是在每次调用时变化。
Default parameters are re-evaluated at each call site, which can bloat the generated code. Readers may also expect the default's value to be fixed at the declaration instead of varying at each call.

在存在默认参数的情况下，函数指针会令人困惑，因为函数签名通常与调用签名不匹配。添加函数重载可以避免这些问题。
Function pointers are confusing in the presence of default arguments, since the function signature often doesn't match the call signature. Adding function overloads avoids these problems.

默认参数在虚拟函数中被禁止，因为它们无法正常工作，并且在指定的默认值可能不会根据计算时间而计算出相同值的情况下。 （例如，不要写 `void f(int n = counter++);` 。）
Default arguments are banned on virtual functions, where they don't work properly, and in cases where the specified default might not evaluate to the same value depending on when it was evaluated. (For example, don't write `void f(int n = counter++);`.)

在其他一些情况下，默认参数可以提高其函数声明的可读性，足以克服上述缺点，因此它们是允许的。如有疑问，请使用重载。
In some other cases, default arguments can improve the readability of their function declarations enough to overcome the downsides above, so they are allowed. When in doubt, use overloads.

## 尾随返回类型语法

## Trailing Return Type Syntax

仅当使用普通语法（前导返回类型）不切实际或可读性较差时才使用尾随返回类型。
Use trailing return types only where using the ordinary syntax (leading return types) is impractical or much less readable.

C++ 允许两种不同形式的函数声明。在旧的形式中，返回类型出现在函数名称之前。例如：
C++ allows two different forms of function declarations. In the older form, the return type appears before the function name. For example:

```C++
int foo(int x);
```

较新的形式在函数名称之前使用 `auto` 关键字，在参数列表之后使用尾随返回类型。例如，上面的声明可以等效地写成：
The newer form uses the `auto` keyword before the function name and a trailing return type after the argument list. For example, the declaration above could equivalently be written:

```C++
auto foo(int x) -> int;
```

尾随返回类型在函数的范围内。这对于像 `int` 这样的简单情况没有什么影响，但对于更复杂的情况很重要，例如在类作用域中声明的类型或根据函数参数编写的类型。
The trailing return type is in the function's scope. This doesn't make a difference for a simple case like `int` but it matters for more complicated cases, like types declared in class scope or types written in terms of the function parameters.

尾随返回类型是显式指定 _lambda 表达式_ 返回类型的唯一方法。在某些情况下，编译器能够推断出 lambda 的返回类型，但并非在所有情况下都如此。即使编译器可以自动推导它，有时显式指定它对读者来说会更清楚。
Trailing return types are the only way to explicitly specify the return type of a _lambda expression_. In some cases the compiler is able to deduce a lambda's return type, but not in all cases. Even when the compiler can deduce it automatically, sometimes specifying it explicitly would be clearer for readers.

有时，在函数的参数列表已经出现之后指定返回类型会更容易且更具可读性。当返回类型取决于模板参数时尤其如此。例如：
Sometimes it's easier and more readable to specify a return type after the function's parameter list has already appeared. This is particularly true when the return type depends on template parameters. For example:

```C++
    template <typename T, typename U>
    auto add(T t, U u) -> decltype(t + u);
```

相对
versus

```C++
    template <typename T, typename U>
    decltype(declval<T&>() + declval<U&>()) add(T t, U u);
```

尾随返回类型语法相对较新，在 C 和 Java 等类 C++ 语言中没有类似的语法，因此有些读者可能会觉得陌生。
Trailing return type syntax is relatively new and it has no analogue in C++-like languages such as C and Java, so some readers may find it unfamiliar.

现有的代码库有大量的函数声明，不会更改以使用新语法，因此实际的选择是仅使用旧语法或混合使用两者。使用单一版本更有利于风格的统一。
Existing code bases have an enormous number of function declarations that aren't going to get changed to use the new syntax, so the realistic choices are using the old syntax only or using a mixture of the two. Using a single version is better for uniformity of style.

在大多数情况下，继续使用旧的函数声明样式，其中返回类型位于函数名称之前。仅在需要时（例如 lambda）或通过将类型放在函数参数列表之后，允许您以更易读的方式编写类型，才使用新的尾随返回类型形式。后一种情况应该很少见；这主要是相当复杂的模板代码中的一个问题，在大多数情况下是不鼓励的。
In most cases, continue to use the older style of function declaration where the return type goes before the function name. Use the new trailing-return-type form only in cases where it's required (such as lambdas) or where, by putting the type after the function's parameter list, it allows you to write the type in a much more readable way. The latter case should be rare; it's mostly an issue in fairly complicated template code, which is _discouraged in most cases_.

# Google 特有的魔法

# Google-Specific Magic

我们使用各种技巧和实用程序来使 C++ 代码更加健壮，并且我们使用 C++ 的各种方式可能与您在其他地方看到的不同。
There are various tricks and utilities that we use to make C++ code more robust, and various ways we use C++ that may differ from what you see elsewhere.

## 所有权和智能指针

## Ownership and Smart Pointers

对于动态分配的对象，更喜欢拥有单一、固定的所有者。更喜欢使用智能指针转移所有权。
Prefer to have single, fixed owners for dynamically allocated objects. Prefer to transfer ownership with smart pointers.

"所有权" 是一种用于管理动态分配的内存（和其他资源）的簿记技术。动态分配对象的所有者是一个对象或函数，负责确保不再需要时将其删除。所有权有时可以共享，在这种情况下，最后一个所有者通常负责删除它。即使所有权不共享，也可以将其从一段代码转移到另一段代码。
"Ownership" is a bookkeeping technique for managing dynamically allocated memory (and other resources). The owner of a dynamically allocated object is an object or function that is responsible for ensuring that it is deleted when no longer needed. Ownership can sometimes be shared, in which case the last owner is typically responsible for deleting it. Even when ownership is not shared, it can be transferred from one piece of code to another.

"智能" 指针是像指针一样工作的类，例如，通过重载 `*` 和 `->` 运算符。某些智能指针类型可用于自动进行所有权簿记，以确保满足这些职责。 [`std::unique_ptr`](http://en.cppreference.com/w/cpp/memory/unique_ptr) 是一种智能指针类型，表示动态分配对象的独占所有权；当 `std::unique_ptr` 超出范围时，该对象将被删除。它无法复制，但可以*移动*以表示所有权转移。 [`std::shared_ptr`](http://en.cppreference.com/w/cpp/memory/shared_ptr) 是一种智能指针类型，表示动态分配对象的共享所有权。 `std::shared_ptr` 可以被复制；该对象的所有权在所有副本之间共享，并且当最后一个 `std::shared_ptr` 被销毁时该对象被删除。
"Smart" pointers are classes that act like pointers, e.g., by overloading the `*` and `->` operators. Some smart pointer types can be used to automate ownership bookkeeping, to ensure these responsibilities are met. [`std::unique_ptr`](http://en.cppreference.com/w/cpp/memory/unique_ptr) is a smart pointer type which expresses exclusive ownership of a dynamically allocated object; the object is deleted when the `std::unique_ptr` goes out of scope. It cannot be copied, but can be _moved_ to represent ownership transfer. [`std::shared_ptr`](http://en.cppreference.com/w/cpp/memory/shared_ptr) is a smart pointer type that expresses shared ownership of a dynamically allocated object. `std::shared_ptr`s can be copied; ownership of the object is shared among all copies, and the object is deleted when the last `std::shared_ptr` is destroyed.

- 如果没有某种所有权逻辑，实际上不可能管理动态分配的内存。
- It's virtually impossible to manage dynamically allocated memory without some sort of ownership logic.
- 转移对象的所有权可能比复制它更便宜（如果甚至可以复制）。
- Transferring ownership of an object can be cheaper than copying it (if copying it is even possible).
- 转移所有权比 '借用' 指针或引用更简单，因为它减少了协调两个用户之间对象生命周期的需要。
- Transferring ownership can be simpler than 'borrowing' a pointer or reference, because it reduces the need to coordinate the lifetime of the object between the two users.
- 智能指针可以通过使所有权逻辑明确、自记录且明确来提高可读性。
- Smart pointers can improve readability by making ownership logic explicit, self-documenting, and unambiguous.
- 智能指针可以消除手动所有权簿记，简化代码并排除大量错误。
- Smart pointers can eliminate manual ownership bookkeeping, simplifying the code and ruling out large classes of errors.
- 对于 `const` 对象，共享所有权可以是深度复制的简单而有效的替代方案。
- For `const` objects, shared ownership can be a simple and efficient alternative to deep copying.

- 所有权必须通过指针（无论是智能指针还是普通指针）来表示和转移。指针语义比值语义更复杂，尤其是在 API 中：您不仅要担心所有权，还要担心别名、生命周期和可变性等问题。
- Ownership must be represented and transferred via pointers (whether smart or plain). Pointer semantics are more complicated than value semantics, especially in APIs: you have to worry not just about ownership, but also aliasing, lifetime, and mutability, among other issues.
- 值语义的性能成本通常被高估，因此所有权转移的性能优势可能无法证明可读性和复杂性成本的合理性。
- The performance costs of value semantics are often overestimated, so the performance benefits of ownership transfer might not justify the readability and complexity costs.
- 转移所有权的 API 迫使其客户端采用单一内存管理模型。
- APIs that transfer ownership force their clients into a single memory management model.
- 使用智能指针的代码对于资源释放发生的位置不太明确。
- Code using smart pointers is less explicit about where the resource releases take place.
- `std::unique_ptr` 使用移动语义表达所有权转移，这是相对较新的，可能会让一些程序员感到困惑。
- `std::unique_ptr` expresses ownership transfer using move semantics, which are relatively new and may confuse some programmers.
- 共享所有权可能是仔细所有权设计的诱人替代方案，从而混淆了系统的设计。
- Shared ownership can be a tempting alternative to careful ownership design, obfuscating the design of a system.
- 共享所有权需要在运行时进行明确的簿记，这可能成本高昂。
- Shared ownership requires explicit bookkeeping at run-time, which can be costly.
- 在某些情况下（例如，循环引用），具有共享所有权的对象可能永远不会被删除。
- In some cases (e.g., cyclic references), objects with shared ownership may never be deleted.
- 智能指针并不能完美替代普通指针。
- Smart pointers are not perfect substitutes for plain pointers.

如果需要动态分配，最好保留分配它的代码的所有权。如果其他代码需要访问该对象，请考虑向其传递副本，或传递指针或引用而不转移所有权。更喜欢使用 `std::unique_ptr` 来明确所有权转移。例如：
If dynamic allocation is necessary, prefer to keep ownership with the code that allocated it. If other code needs access to the object, consider passing it a copy, or passing a pointer or reference without transferring ownership. Prefer to use `std::unique_ptr` to make ownership transfer explicit. For example:

```C++
std::unique_ptr<Foo> FooFactory();
void FooConsumer(std::unique_ptr<Foo> ptr);
```

如果没有充分的理由，请勿将代码设计为使用共享所有权。其中一个原因是避免昂贵的复制操作，但只有在性能优势显着并且底层对象是不可变的（即 `std::shared_ptr<const Foo>` ）时才应该这样做。如果您确实使用共享所有权，请优先使用 `std::shared_ptr` 。
Do not design your code to use shared ownership without a very good reason. One such reason is to avoid expensive copy operations, but you should only do this if the performance benefits are significant, and the underlying object is immutable (i.e., `std::shared_ptr<const Foo>`). If you do use shared ownership, prefer to use `std::shared_ptr`.

切勿使用 `std::auto_ptr` 。相反，请使用 `std::unique_ptr` 。
Never use `std::auto_ptr`. Instead, use `std::unique_ptr`.

## cpplint

## cpplint

使用 `cpplint.py` 来检测样式错误。
Use `cpplint.py` to detect style errors.

`cpplint.py` 是一个读取源文件并识别许多样式错误的工具。它并不完美，并且存在误报和误报，但它仍然是一个有价值的工具。
`cpplint.py` is a tool that reads a source file and identifies many style errors. It is not perfect, and has both false positives and false negatives, but it is still a valuable tool.

有些项目有关于如何从项目工具运行 `cpplint.py` 的说明。如果您参与的项目没有，您可以单独下载 [`cpplint.py`](https://raw.githubusercontent.com/google/styleguide/gh-pages/cpplint/cpplint.py)。
Some projects have instructions on how to run `cpplint.py` from their project tools. If the project you are contributing to does not, you can download [`cpplint.py`](https://raw.githubusercontent.com/google/styleguide/gh-pages/cpplint/cpplint.py) separately.

# 其他 C++ 特性

# Other C++ Features

## 右值引用

## Rvalue References

仅在下面列出的某些特殊情况下才使用右值引用。
Use rvalue references only in certain special cases listed below.

右值引用是一种只能绑定到临时对象的引用类型。语法类似于传统的引用语法。例如， `void f(std::string&& s);` 声明一个函数，其参数是对 `std::string` 的右值引用。
Rvalue references are a type of reference that can only bind to temporary objects. The syntax is similar to traditional reference syntax. For example, `void f(std::string&& s);` declares a function whose argument is an rvalue reference to a `std::string`.

当标记 `&&` 应用于函数参数中的非限定模板参数时，将应用特殊的模板参数推导规则。这样的引用称为转发引用。
When the token '&&' is applied to an unqualified template argument in a function parameter, special template argument deduction rules apply. Such a reference is called a forwarding reference.

- 定义移动构造函数（采用对类类型的右值引用的构造函数）可以移动值而不是复制它。例如，如果 `v1` 是 `std::vector<std::string>`，那么 `auto v2(std::move(v1))` 可能只会导致一些简单的指针操作，而不是复制一个大的指针数据量。在许多情况下，这可以带来重大的性能改进。
- Defining a move constructor (a constructor taking an rvalue reference to the class type) makes it possible to move a value instead of copying it. If `v1` is a `std::vector<std::string>`, for example, then `auto v2(std::move(v1))` will probably just result in some simple pointer manipulation instead of copying a large amount of data. In many cases this can result in a major performance improvement.
- 右值引用使得实现可移动但不可复制的类型成为可能，这对于没有合理的复制定义但您可能仍希望将它们作为函数参数传递、将它们放入容器等的类型非常有用。
- Rvalue references make it possible to implement types that are movable but not copyable, which can be useful for types that have no sensible definition of copying but where you might still want to pass them as function arguments, put them in containers, etc.
- `std::move` 对于有效使用某些标准库类型是必要的，例如 `std::unique_ptr`。
- `std::move` is necessary to make effective use of some standard-library types, such as `std::unique_ptr`.
- 使用右值引用标记的*转发引用*，可以编写一个通用函数包装器，将其参数转发到另一个函数，并且无论其参数是否是临时对象和/或常量，都可以工作。这称为 '完美转发' 。
- _Forwarding references_ which use the rvalue reference token, make it possible to write a generic function wrapper that forwards its arguments to another function, and works whether or not its arguments are temporary objects and/or const. This is called 'perfect forwarding'.

- 右值引用尚未被广泛理解。像引用折叠和转发引用的特殊扣除规则这样的规则有些模糊。
- Rvalue references are not yet widely understood. Rules like reference collapsing and the special deduction rule for forwarding references are somewhat obscure.
- 右值引用经常被滥用。在签名中使用右值引用是违反直觉的，其中参数在函数调用后应具有有效的指定状态，或者不执行移动操作。
- Rvalue references are often misused. Using rvalue references is counter-intuitive in signatures where the argument is expected to have a valid specified state after the function call, or where no move operation is performed.

不要使用右值引用（或将 `&&` 限定符应用于方法），但以下情况除外：
Do not use rvalue references (or apply the `&&` qualifier to methods), except as follows:

- 您可以使用它们来定义移动构造函数和移动赋值运算符（如*可复制和可移动类型*中所述）。
- You may use them to define move constructors and move assignment operators (as described in _Copyable and Movable Types_).
- 您可以使用它们来定义逻辑上 "消耗" `*this` 的 `&&` 限定的方法，使其处于不可用或空状态。请注意，这仅适用于方法限定符（位于函数签名的右括号之后）；如果您想 `使用` 普通函数参数，最好按值传递它。
- You may use them to define `&&`\-qualified methods that logically "consume" `*this`, leaving it in an unusable or empty state. Note that this applies only to method qualifiers (which come after the closing parenthesis of the function signature); if you want to "consume" an ordinary function parameter, prefer to pass it by value.
- 您可以将转发引用与 [`std::forward`](http://en.cppreference.com/w/cpp/utility/forward) 结合使用，以支持完美转发。
- You may use forwarding references in conjunction with [`std::forward`](http://en.cppreference.com/w/cpp/utility/forward), to support perfect forwarding.
- 您可以使用它们来定义一对重载，例如一个重载采用 `Foo&&` ，另一个采用 `const Foo&` 。通常首选的解决方案只是按值传递，但重载的函数对有时会产生更好的性能，有时在需要支持多种类型的通用代码中是必需的。一如既往：如果您为了性能而编写更复杂的代码，请确保您有证据表明它确实有帮助。
- You may use them to define pairs of overloads, such as one taking `Foo&&` and the other taking `const Foo&`. Usually the preferred solution is just to pass by value, but an overloaded pair of functions sometimes yields better performance and is sometimes necessary in generic code that needs to support a wide variety of types. As always: if you're writing more complicated code for the sake of performance, make sure you have evidence that it actually helps.

## 友元

## Friends

我们允许在合理范围内使用 `friend` 类和函数。
We allow use of `friend` classes and functions, within reason.

友元通常应该定义在同一个文件中，这样读者就不必在另一个文件中查找类的私有成员的用途。 `friend` 的常见用途是让 `FooBuilder` 类成为 `Foo` 的友元，以便它可以正确构造 `Foo` 的内部状态，而不将该状态暴露给世界。在某些情况下，使单元测试类成为它所测试的类的友元可能会很有用。
Friends should usually be defined in the same file so that the reader does not have to look in another file to find uses of the private members of a class. A common use of `friend` is to have a `FooBuilder` class be a friend of `Foo` so that it can construct the inner state of `Foo` correctly, without exposing this state to the world. In some cases it may be useful to make a unittest class a friend of the class it tests.

友元扩展但不破坏类的封装边界。在某些情况下，当您只想授予另一个类访问该成员的权限时，这比将成员设置为 `public` 要好。然而，大多数类应该仅通过其公共成员与其他类交互。
Friends extend, but do not break, the encapsulation boundary of a class. In some cases this is better than making a member `public` when you want to give only one other class access to it. However, most classes should interact with other classes solely through their public members.

## 例外情况

## Exceptions

我们不使用 C++ 异常。
We do not use C++ exceptions.

- 异常允许应用程序的更高级别决定如何处理深度嵌套函数中 "不可能发生" 的故障，而无需对错误代码进行模糊且容易出错的记录。
- Exceptions allow higher levels of an application to decide how to handle "can't happen" failures in deeply nested functions, without the obscuring and error-prone bookkeeping of error codes.

- 大多数其他现代语言都使用异常。在 C++ 中使用它们将使其与其他人熟悉的 Python、Java 和 C++ 更加一致。
- Exceptions are used by most other modern languages. Using them in C++ would make it more consistent with Python, Java, and the C++ that others are familiar with.

- 一些第三方 C++ 库使用异常，在内部关闭它们会使与这些库集成变得更加困难。
- Some third-party C++ libraries use exceptions, and turning them off internally makes it harder to integrate with those libraries.
- 异常是构造函数失败的唯一原因。我们可以使用工厂函数或 `Init()` 方法来模拟这一点，但它们分别需要堆分配或新的 "无效" 状态。
- Exceptions are the only way for a constructor to fail. We can simulate this with a factory function or an `Init()` method, but these require heap allocation or a new "invalid" state, respectively.
- 异常在测试框架中非常方便。
- Exceptions are really handy in testing frameworks.

- 当您向现有函数添加 `throw` 语句时，您必须检查其所有传递调用者。他们要么必须至少做出基本的异常安全保证，要么永远不能捕获异常并对程序因此终止感到满意。例如，如果 `f()` 调用 `g()` 调用 `h()` ，并且 `h` 引发 `f` 捕获的异常，则 `g` 必须小心，否则可能无法正确清理。
- When you add a `throw` statement to an existing function, you must examine all of its transitive callers. Either they must make at least the basic exception safety guarantee, or they must never catch the exception and be happy with the program terminating as a result. For instance, if `f()` calls `g()` calls `h()`, and `h` throws an exception that `f` catches, `g` has to be careful or it may not clean up properly.
- 更一般地说，异常使得程序的控制流难以通过查看代码来评估：函数可能会在您不期望的地方返回。这会导致可维护性和调试困难。您可以通过一些有关如何以及在何处使用异常的规则来最小化此成本，但代价是开发人员需要了解和理解更多内容。
- More generally, exceptions make the control flow of programs difficult to evaluate by looking at code: functions may return in places you don't expect. This causes maintainability and debugging difficulties. You can minimize this cost via some rules on how and where exceptions can be used, but at the cost of more that a developer needs to know and understand.
- 异常安全需要 RAII 和不同的编码实践。需要大量的支持机制才能轻松编写正确的异常安全代码。此外，为了避免要求读者理解整个调用图，异常安全代码必须将写入持久状态的逻辑隔离到 "提交" 阶段。这既有好处也有成本（也许您被迫混淆代码以隔离提交）。允许例外将迫使我们始终支付这些成本，即使它们不值得。
- Exception safety requires both RAII and different coding practices. Lots of supporting machinery is needed to make writing correct exception-safe code easy. Further, to avoid requiring readers to understand the entire call graph, exception-safe code must isolate logic that writes to persistent state into a "commit" phase. This will have both benefits and costs (perhaps where you're forced to obfuscate code to isolate the commit). Allowing exceptions would force us to always pay those costs even when they're not worth it.
- 打开异常会将数据添加到生成的每个二进制文件中，从而增加编译时间（可能略有增加）并可能增加地址空间压力。
- Turning on exceptions adds data to each binary produced, increasing compile time (probably slightly) and possibly increasing address space pressure.
- 异常的可用性可能会鼓励开发人员在异常不合适时抛出异常，或者在不安全时从中恢复。例如，无效的用户输入不应导致引发异常。我们需要使样式指南更长来记录这些限制！
- The availability of exceptions may encourage developers to throw them when they are not appropriate or recover from them when it's not safe to do so. For example, invalid user input should not cause exceptions to be thrown. We would need to make the style guide even longer to document these restrictions!

从表面上看，使用例外的好处大于成本，尤其是在新项目中。但是，对于现有代码，异常的引入会对所有相关代码产生影响。如果异常可以传播到新项目之外，那么将新项目集成到现有的无异常代码中也会出现问题。由于 Google 现有的大多数 C++ 代码都没有准备好处理异常，因此采用生成异常的新代码相对困难。
On their face, the benefits of using exceptions outweigh the costs, especially in new projects. However, for existing code, the introduction of exceptions has implications on all dependent code. If exceptions can be propagated beyond a new project, it also becomes problematic to integrate the new project into existing exception-free code. Because most existing C++ code at Google is not prepared to deal with exceptions, it is comparatively difficult to adopt new code that generates exceptions.

鉴于 Google 现有的代码不支持异常，因此使用异常的成本比新项目的成本要高一些。转换过程会很慢并且容易出错。我们认为异常的可用替代方案（例如错误代码和断言）不会带来重大负担。
Given that Google's existing code is not exception-tolerant, the costs of using exceptions are somewhat greater than the costs in a new project. The conversion process would be slow and error-prone. We don't believe that the available alternatives to exceptions, such as error codes and assertions, introduce a significant burden.

我们反对使用例外的建议不是基于哲学或道德理由，而是基于实践。因为我们希望使用 Google 的开源项目，但如果这些项目使用异常，则很难做到这一点，因此我们也需要建议不要在 Google 开源项目中使用异常。如果我们必须从头开始，事情可能会有所不同。
Our advice against using exceptions is not predicated on philosophical or moral grounds, but practical ones. Because we'd like to use our open-source projects at Google and it's difficult to do so if those projects use exceptions, we need to advise against exceptions in Google open-source projects as well. Things would probably be different if we had to do it all over again from scratch.

此禁令也适用于与异常处理相关的函数，例如 `std::exception_ptr` 和 `std::nested_exception` 。
This prohibition also applies to exception handling related features such as `std::exception_ptr` and `std::nested_exception`.

对于 Windows 代码，此规则有一个例外。
There is an _exception_ to this rule (no pun intended) for Windows code.

## `noexcept`

## `noexcept`

当 `noexcept` 有用且正确时指定它。
Specify `noexcept` when it is useful and correct.

`noexcept` 说明符用于指定函数是否抛出异常。如果异常从标记为 `noexcept` 的函数中逃逸，程序将通过 `std::terminate` 崩溃。
The `noexcept` specifier is used to specify whether a function will throw exceptions or not. If an exception escapes from a function marked `noexcept`, the program crashes via `std::terminate`.

`noexcept` 运算符执行编译时检查，如果声明表达式不抛出任何异常，则返回 true。
The `noexcept` operator performs a compile-time check that returns true if an expression is declared to not throw any exceptions.

- 在某些情况下，将移动构造函数指定为 `noexcept` 可提高性能，例如，如果 T 的移动构造函数为 `noexcept` ，则 `std::vector<T>::resize()` 会移动而不是复制对象。
- Specifying move constructors as `noexcept` improves performance in some cases, e.g., `std::vector<T>::resize()` moves rather than copies the objects if T's move constructor is `noexcept`.
- 在函数上指定 `noexcept` 可以在启用异常的环境中触发编译器优化，例如，如果编译器知道不会由于 `noexcept` 说明符而引发任何异常，则它不必为堆栈展开生成额外的代码。
- Specifying `noexcept` on a function can trigger compiler optimizations in environments where exceptions are enabled, e.g., compiler does not have to generate extra code for stack-unwinding, if it knows that no exceptions can be thrown due to a `noexcept` specifier.

- 在遵循本指南的禁用异常的项目中，很难确保 `noexcept` 说明符是正确的，甚至很难定义正确性的含义。
- In projects following this guide that have exceptions disabled it is hard to ensure that `noexcept` specifiers are correct, and hard to define what correctness even means.
- 撤消 `noexcept` 即使不是不可能，也很困难，因为它以难以检测的方式消除了调用者可能依赖的保证。
- It's hard, if not impossible, to undo `noexcept` because it eliminates a guarantee that callers may be relying on, in ways that are hard to detect.

如果它准确地反映了函数的预期语义，即如果从函数体内以某种方式抛出异常，那么它代表一个致命错误，那么当它对性能有用时，您可以使用 `noexcept` 。您可以假设移动构造函数上的 `noexcept` 具有有意义的性能优势。如果您认为在某些其他函数上指定 `noexcept` 可以显着提高性能，请与您的项目负责人讨论。
You may use `noexcept` when it is useful for performance if it accurately reflects the intended semantics of your function, i.e., that if an exception is somehow thrown from within the function body then it represents a fatal error. You can assume that `noexcept` on move constructors has a meaningful performance benefit. If you think there is significant performance benefit from specifying `noexcept` on some other function, please discuss it with your project leads.

如果完全禁用异常（即大多数 Google C++ 环境），则首选无条件 `noexcept` 。否则，请使用带有简单条件的条件 `noexcept` 说明符，仅在函数可能抛出异常的少数情况下评估 false。测试可能包括类型特征检查，检查所涉及的操作是否可能抛出（例如，对于移动构造对象，`std::is_nothrow_move_constructible`），或者分配是否可以抛出（例如，对于标准默认分配，`absl::default_allocator_is_nothrow`） 。请注意，在许多情况下，异常的唯一可能原因是分配失败（我们认为移动构造函数不应抛出异常，除非分配失败），并且在许多应用程序中，适合将内存耗尽视为致命错误而不是异常情况您的程序应该尝试从中恢复。即使对于其他潜在的故障，您也应该优先考虑接口的简单性，而不是支持所有可能的异常抛出场景：例如，不要编写依赖于哈希函数是否可以抛出的复杂的 `noexcept` 子句，而是简单地记录您的组件不支持哈希函数抛出并使其无条件 `noexcept` 。
Prefer unconditional `noexcept` if exceptions are completely disabled (i.e., most Google C++ environments). Otherwise, use conditional `noexcept` specifiers with simple conditions, in ways that evaluate false only in the few cases where the function could potentially throw. The tests might include type traits check on whether the involved operation might throw (e.g., `std::is_nothrow_move_constructible` for move-constructing objects), or on whether allocation can throw (e.g., `absl::default_allocator_is_nothrow` for standard default allocation). Note in many cases the only possible cause for an exception is allocation failure (we believe move constructors should not throw except due to allocation failure), and there are many applications where it’s appropriate to treat memory exhaustion as a fatal error rather than an exceptional condition that your program should attempt to recover from. Even for other potential failures you should prioritize interface simplicity over supporting all possible exception throwing scenarios: instead of writing a complicated `noexcept` clause that depends on whether a hash function can throw, for example, simply document that your component doesn’t support hash functions throwing and make it unconditionally `noexcept`.

## 运行时类型信息 (RTTI)

## Run-Time Type Information (RTTI)

避免使用运行时类型信息 (RTTI)。
Avoid using run-time type information (RTTI).

RTTI 允许程序员在运行时查询对象的 C++ 类。这是通过使用 `typeid` 或 `dynamic_cast` 来完成的。
RTTI allows a programmer to query the C++ class of an object at run-time. This is done by use of `typeid` or `dynamic_cast`.

RTTI 的标准替代方案（如下所述）需要修改或重新设计相关的类层次结构。有时，此类修改是不可行或不可取的，特别是在广泛使用或成熟的代码中。
The standard alternatives to RTTI (described below) require modification or redesign of the class hierarchy in question. Sometimes such modifications are infeasible or undesirable, particularly in widely-used or mature code.

RTTI 在某些单元测试中很有用。例如，它在工厂类的测试中很有用，其中测试必须验证新创建的对象是否具有预期的动态类型。它对于管理对象及其模拟之间的关系也很有用。
RTTI can be useful in some unit tests. For example, it is useful in tests of factory classes where the test has to verify that a newly created object has the expected dynamic type. It is also useful in managing the relationship between objects and their mocks.

RTTI 在考虑多个抽象对象时非常有用。考虑
RTTI is useful when considering multiple abstract objects. Consider

```C++
bool Base::Equal(Base* other) = 0;
bool Derived::Equal(Base* other) {
  Derived* that = dynamic_cast<Derived*>(other);
  if (that == nullptr)
    return false;
  ...
}
```

在运行时查询对象的类型通常意味着设计问题。在运行时需要知道对象的类型通常表明类层次结构的设计存在缺陷。
Querying the type of an object at run-time frequently means a design problem. Needing to know the type of an object at runtime is often an indication that the design of your class hierarchy is flawed.

不规范地使用 RTTI 会使代码难以维护。它可能导致基于类型的决策树或 switch 语句分散在整个代码中，在进行进一步更改时必须检查所有这些。
Undisciplined use of RTTI makes code hard to maintain. It can lead to type-based decision trees or switch statements scattered throughout the code, all of which must be examined when making further changes.

RTTI 具有合法用途，但容易被滥用，因此使用时必须小心。您可以在单元测试中自由使用它，但在其他代码中尽可能避免使用它。特别是，在新代码中使用 RTTI 之前请三思。如果您发现自己需要编写根据对象的类而表现不同的代码，请考虑以下查询类型的替代方法之一：
RTTI has legitimate uses but is prone to abuse, so you must be careful when using it. You may use it freely in unittests, but avoid it when possible in other code. In particular, think twice before using RTTI in new code. If you find yourself needing to write code that behaves differently based on the class of an object, consider one of the following alternatives to querying the type:

- 虚拟方法是根据特定子类类型执行不同代码路径的首选方式。这将工作置于对象本身内。
- Virtual methods are the preferred way of executing different code paths depending on a specific subclass type. This puts the work within the object itself.
- 如果工作属于对象之外，而是在某些处理代码中，请考虑双重分派解决方案，例如访问者设计模式。这允许对象本身外部的工具使用内置类型系统来确定类的类型。
- If the work belongs outside the object and instead in some processing code, consider a double-dispatch solution, such as the Visitor design pattern. This allows a facility outside the object itself to determine the type of class using the built-in type system.

当程序的逻辑保证基类的给定实例实际上是特定派生类的实例时，则可以在该对象上自由使用 `dynamic_cast` 。在这种情况下，通常可以使用 `static_cast` 作为替代方案。
When the logic of a program guarantees that a given instance of a base class is in fact an instance of a particular derived class, then a `dynamic_cast` may be used freely on the object. Usually one can use a `static_cast` as an alternative in such situations.

基于类型的决策树强烈表明您的代码走错了路。
Decision trees based on type are a strong indication that your code is on the wrong track.

**badcode**

```C++
if (typeid(*data) == typeid(D1)) {
  ...
} else if (typeid(*data) == typeid(D2)) {
  ...
} else if (typeid(*data) == typeid(D3)) {
...
```

当向类层次结构中添加其他子类时，此类代码通常会中断。而且，当子类的属性发生变化时，很难找到并修改所有受影响的代码段。
Code such as this usually breaks when additional subclasses are added to the class hierarchy. Moreover, when properties of a subclass change, it is difficult to find and modify all the affected code segments.

不要手动实现类似 RTTI 的解决方法。反对 RTTI 的论点同样适用于像带有类型标签的类层次结构这样的解决方法。此外，变通办法掩盖了你的真实意图。
Do not hand-implement an RTTI-like workaround. The arguments against RTTI apply just as much to workarounds like class hierarchies with type tags. Moreover, workarounds disguise your true intent.

## 强制转换

## Casting

使用 C++ 风格的强制转换，如 `static_cast<float>(double_value)` ，或使用大括号初始化来转换算术类型，如 `int64_t y = int64_t{1} << 42` 。不要使用像 `(int)x` 这样的转换格式，除非转换为 `void` 。仅当 `T` 是类类型时，才可以使用 `T(x)` 等强制转换格式。
Use C++-style casts like `static_cast<float>(double_value)`, or brace initialization for conversion of arithmetic types like `int64_t y = int64_t{1} << 42`. Do not use cast formats like `(int)x` unless the cast is to `void`. You may use cast formats like `T(x)` only when `T` is a class type.

C++ 引入了与 C 不同的强制转换系统，用于区分强制转换操作的类型。
C++ introduced a different cast system from C that distinguishes the types of cast operations.

C 类型转换的问题是操作的模糊性；有时你正在做一个*conversion*（例如，`(int)3.5`），有时你正在做一个*cast*（例如，`(int)"hello"`）。大括号初始化和 C++ 强制转换通常可以帮助避免这种歧义。此外，C++ 强制转换在搜索时更加明显。
The problem with C casts is the ambiguity of the operation; sometimes you are doing a _conversion_ (e.g., `(int)3.5`) and sometimes you are doing a _cast_ (e.g., `(int)"hello"`). Brace initialization and C++ casts can often help avoid this ambiguity. Additionally, C++ casts are more visible when searching for them.

C++ 风格的转换语法冗长且麻烦。
The C++-style cast syntax is verbose and cumbersome.

一般来说，不要使用 C 风格的强制转换。相反，当需要显式类型转换时，请使用这些 C++ 样式的强制转换。
In general, do not use C-style casts. Instead, use these C++-style casts when explicit type conversion is necessary.

- 使用大括号初始化来转换算术类型（例如，`int64_t{x}`）。这是最安全的方法，因为如果转换可能导致信息丢失，代码将无法编译。语法也很简洁。
- Use brace initialization to convert arithmetic types (e.g., `int64_t{x}`). This is the safest approach because code will not compile if conversion can result in information loss. The syntax is also concise.
- 使用 `absl::implicit_cast` 安全地转换类型层次结构，例如，将 `Foo*` 转换为 `SuperclassOfFoo*` 或将 `Foo*` 转换为 `const Foo*` 。 C++ 通常会自动执行此操作，但某些情况需要显式向上转换，例如使用 `?:` 运算符。
- Use `absl::implicit_cast` to safely cast up a type hierarchy, e.g., casting a `Foo*` to a `SuperclassOfFoo*` or casting a `Foo*` to a `const Foo*`. C++ usually does this automatically but some situations need an explicit up-cast, such as use of the `?:` operator.
- 当您需要将指针从类显式向上转换为其超类时，或者当您需要将指针从超类显式转换为子类。在最后一种情况下，您必须确保您的对象实际上是子类的实例。
- Use `static_cast` as the equivalent of a C-style cast that does value conversion, when you need to explicitly up-cast a pointer from a class to its superclass, or when you need to explicitly cast a pointer from a superclass to a subclass. In this last case, you must be sure your object is actually an instance of the subclass.
- 使用 `const_cast` 删除 `const` 限定符（参见 _const_）。
- Use `const_cast` to remove the `const` qualifier (see _const_).
- 使用 `reinterpret_cast` 对指针类型与整数和其他指针类型（包括 `void*` ）进行不安全的转换。仅当您知道自己在做什么并且了解别名问题时才使用此选项。另外，请考虑取消引用指针（不进行强制转换）并使用 `absl::bit_cast` 来强制转换结果值。
- Use `reinterpret_cast` to do unsafe conversions of pointer types to and from integer and other pointer types, including `void*`. Use this only if you know what you are doing and you understand the aliasing issues. Also, consider dereferencing the pointer (without a cast) and using `absl::bit_cast` to cast the resulting value.
- 使用 `absl::bit_cast` 使用相同大小的不同类型（类型双关语）解释值的原始位，例如将 `double` 的位解释为 `int64_t` 。
- Use `absl::bit_cast` to interpret the raw bits of a value using a different type of the same size (a type pun), such as interpreting the bits of a `double` as `int64_t`.

有关使用 `dynamic_cast` 的指导，请参阅 [RTTI 部分](#Run-Time_Type_Information__RTTI_)。
See the [RTTI section](#Run-Time_Type_Information__RTTI_) for guidance on the use of `dynamic_cast`.

## 流

## Streams

在适当的地方使用流，并坚持 "简单" 的用法。仅对表示值的类型进行流式重载 `<<` ，并且仅写入用户可见的值，而不写入任何实现细节。
Use streams where appropriate, and stick to "simple" usages. Overload `<<` for streaming only for types representing values, and write only the user-visible value, not any implementation details.

流是 C++ 中的标准 I/O 抽象，如标准头 `<iostream>` 所示。它们广泛用于 Google 代码中，主要用于调试日志记录和测试诊断。
Streams are the standard I/O abstraction in C++, as exemplified by the standard header `<iostream>`. They are widely used in Google code, mostly for debug logging and test diagnostics.

可移植、可重用和可扩展的 API 。相比之下， `printf` 甚至不支持 `std::string` ，更不用说用户定义的类型了，而且很难移植。 `printf` 还要求您在该函数的众多略有不同的版本中进行选择，并浏览数十个转换说明符。
The `<<` and `>>` stream operators provide an API for formatted I/O that is easily learned, portable, reusable, and extensible. `printf`, by contrast, doesn't even support `std::string`, to say nothing of user-defined types, and is very difficult to use portably. `printf` also obliges you to choose among the numerous slightly different versions of that function, and navigate the dozens of conversion specifiers.

流通过 `std::cin` 、 `std::cout` 、 `std::cerr` 和 `std::clog` 为控制台 I/O 提供一流的支持。 C API 也可以这样做，但由于需要手动缓冲输入而受到限制。
Streams provide first-class support for console I/O via `std::cin`, `std::cout`, `std::cerr`, and `std::clog`. The C APIs do as well, but are hampered by the need to manually buffer the input.

- 可以通过改变流的状态来配置流格式。此类突变是持久的，因此代码的行为可能会受到流的整个先前历史记录的影响，除非您每次其他代码可能触及它时都竭尽全力将其恢复到已知状态。用户代码不仅可以修改内置状态，还可以通过注册系统添加新的状态变量和行为。
- Stream formatting can be configured by mutating the state of the stream. Such mutations are persistent, so the behavior of your code can be affected by the entire previous history of the stream, unless you go out of your way to restore it to a known state every time other code might have touched it. User code can not only modify the built-in state, it can add new state variables and behaviors through a registration system.
- 由于上述问题、流代码中代码和数据混合的方式以及运算符重载的使用（可能会选择与您期望不同的重载），因此很难精确控制流输出。
- It is difficult to precisely control stream output, due to the above issues, the way code and data are mixed in streaming code, and the use of operator overloading (which may select a different overload than you expect).
- 通过 `<<` 运算符链构建输出的做法会干扰国际化，因为它将词序融入代码中，并且流对本地化的支持是[有缺陷的](http://www.boost.org/doc/libs/1_48_0/libs/locale/doc/html/rationale.html#rationale_why)。
- The practice of building up output through chains of `<<` operators interferes with internationalization, because it bakes word order into the code, and streams' support for localization is [flawed](http://www.boost.org/doc/libs/1_48_0/libs/locale/doc/html/rationale.html#rationale_why).
- 流 API 微妙而复杂，因此程序员必须积累使用它的经验才能有效地使用它。
- The streams API is subtle and complex, so programmers must develop experience with it in order to use it effectively.
- 解决`<<`的许多重载对于编译器来说代价极高。当在大型代码库中普遍使用时，它可能会消耗多达 20% 的解析和语义分析时间。
- Resolving the many overloads of `<<` is extremely costly for the compiler. When used pervasively in a large code base, it can consume as much as 20% of the parsing and semantic analysis time.

仅当流是完成工作的最佳工具时才使用它们。当 I/O 是临时的、本地的、人类可读的并且针对其他开发人员而不是最终用户时，通常会出现这种情况。与您周围的代码以及整个代码库保持一致；如果有一个既定的工具可以解决您的问题，请改用该工具。特别是，对于诊断输出，日志记录库通常比 `std::cerr` 或 `std::clog` 更好，而 `absl/strings` 或类似库中的库通常比 `std::stringstream` 更好。
Use streams only when they are the best tool for the job. This is typically the case when the I/O is ad-hoc, local, human-readable, and targeted at other developers rather than end-users. Be consistent with the code around you, and with the codebase as a whole; if there's an established tool for your problem, use that tool instead. In particular, logging libraries are usually a better choice than `std::cerr` or `std::clog` for diagnostic output, and the libraries in `absl/strings` or the equivalent are usually a better choice than `std::stringstream`.

避免使用面向外部用户或处理不受信任数据的 I/O 流。相反，找到并使用适当的模板库来处理国际化、本地化和安全强化等问题。
Avoid using streams for I/O that faces external users or handles untrusted data. Instead, find and use the appropriate templating libraries to handle issues like internationalization, localization, and security hardening.

如果您确实使用流，请避免使用流 API 的有状态部分（错误状态除外），例如 `imbue()` 、 `xalloc()` 和 `register_callback()` 。使用显式格式化函数（例如 `absl::StreamFormat()` ）而不是流操纵器或格式化标志来控制格式化细节，例如基数、精度或填充。
If you do use streams, avoid the stateful parts of the streams API (other than error state), such as `imbue()`, `xalloc()`, and `register_callback()`. Use explicit formatting functions (such as `absl::StreamFormat()`) rather than stream manipulators or formatting flags to control formatting details such as number base, precision, or padding.

仅当您的类型表示一个值时，才将 `<<` 重载为您类型的流式运算符，并且 `<<` 会写出该值的人类可读字符串表示形式。避免在 `<<` 的输出中暴露实现细节；如果您需要打印对象内部结构以进行调试，请改用命名函数（名为 `DebugString()` 的方法是最常见的约定）。
Overload `<<` as a streaming operator for your type only if your type represents a value, and `<<` writes out a human-readable string representation of that value. Avoid exposing implementation details in the output of `<<`; if you need to print object internals for debugging, use named functions instead (a method named `DebugString()` is the most common convention).

## 前自增和前自减

## Preincrement and Predecrement

除非需要后缀语义，否则请使用递增和递减运算符的前缀形式 (`++i`)。
Use the prefix form (`++i`) of the increment and decrement operators unless you need postfix semantics.

当变量递增（`++i` 或 `i++`）或递减（`--i` 或 `i--`）并且不使用表达式的值时，必须决定是否预递增（递减）或后递增（递减）。
When a variable is incremented (`++i` or `i++`) or decremented (`--i` or `i--`) and the value of the expression is not used, one must decide whether to preincrement (decrement) or postincrement (decrement).

后缀递增/递减表达式的计算结果为 _修改前的值_ 。这可能会导致代码更紧凑但更难阅读。前缀形式通常更具可读性，效率绝不会降低，而且效率更高，因为它不需要像操作之前那样复制值。
A postfix increment/decrement expression evaluates to the value _as it was before it was modified_. This can result in code that is more compact but harder to read. The prefix form is generally more readable, is never less efficient, and can be more efficient because it doesn't need to make a copy of the value as it was before the operation.

在 C 语言中，即使不使用表达式值，特别是在 `for` 循环中，也形成了使用后递增的传统。
The tradition developed, in C, of using post-increment, even when the expression value is not used, especially in `for` loops.

使用前缀递增/递减，除非代码明确需要后缀递增/递减表达式的结果。
Use prefix increment/decrement, unless the code explicitly needs the result of the postfix increment/decrement expression.

## 常量的使用

## Use of const

在 API 中，只要有意义就使用 `const` 。对于 const 的某些用途， `constexpr` 是更好的选择。
In APIs, use `const` whenever it makes sense. `constexpr` is a better choice for some uses of const.

声明的变量和参数可以在关键字 `const` 前面，以指示变量不被更改（例如， `const int foo` ）。类函数可以具有 `const` 限定符来指示该函数不会更改类成员变量的状态（例如，`class Foo { int Bar(char c) const; };`）。
Declared variables and parameters can be preceded by the keyword `const` to indicate the variables are not changed (e.g., `const int foo`). Class functions can have the `const` qualifier to indicate the function does not change the state of the class member variables (e.g., `class Foo { int Bar(char c) const; };`).

人们更容易理解变量是如何使用的。允许编译器进行更好的类型检查，并且可以想象生成更好的代码。帮助人们确信程序的正确性，因为他们知道他们调用的函数在修改变量的方式上受到限制。帮助人们了解在多线程程序中哪些函数可以安全使用而无需锁定。
Easier for people to understand how variables are being used. Allows the compiler to do better type checking, and, conceivably, generate better code. Helps people convince themselves of program correctness because they know the functions they call are limited in how they can modify your variables. Helps people know what functions are safe to use without locks in multi-threaded programs.

`const` 具有病毒式传播：如果将 `const` 变量传递给函数，则该函数的原型中必须有 `const` （或者该变量将需要 `const_cast`）。调用库函数时这可能是一个特殊问题。
`const` is viral: if you pass a `const` variable to a function, that function must have `const` in its prototype (or the variable will need a `const_cast`). This can be a particular problem when calling library functions.

我们强烈建议在 API 中使用 `const` （即在函数参数、方法和非局部变量上），只要它有意义且准确。这提供了一致的、大部分经过编译器验证的文档，说明操作可以改变哪些对象。拥有一致且可靠的方法来区分读取和写入对于编写线程安全代码至关重要，并且在许多其他上下文中也很有用。尤其：
We strongly recommend using `const` in APIs (i.e., on function parameters, methods, and non-local variables) wherever it is meaningful and accurate. This provides consistent, mostly compiler-verified documentation of what objects an operation can mutate. Having a consistent and reliable way to distinguish reads from writes is critical to writing thread-safe code, and is useful in many other contexts as well. In particular:

- 如果函数保证不会修改通过引用或指针传递的参数，则相应的函数参数应该是对 const 的引用（`const T&`）或对 const 的指针（`const T*`） ， 分别。
- If a function guarantees that it will not modify an argument passed by reference or by pointer, the corresponding function parameter should be a reference-to-const (`const T&`) or pointer-to-const (`const T*`), respectively.
- 对于按值传递的函数参数， `const` 对调用者没有影响，因此不建议在函数声明中使用。请参阅 [TotW #109](https://abseil.io/tips/109)。
- For a function parameter passed by value, `const` has no effect on the caller, thus is not recommended in function declarations. See [TotW #109](https://abseil.io/tips/109).
- 将方法声明为 `const` ，除非它们改变对象的逻辑状态（或允许用户修改该状态，例如，通过返回非 `const` 引用，但这种情况很少见），否则它们不能安全被同时调用。
- Declare methods to be `const` unless they alter the logical state of the object (or enable the user to modify that state, e.g., by returning a non-`const` reference, but that's rare), or they can't safely be invoked concurrently.

既不鼓励也不阻止在局部变量上使用 `const` 。
Using `const` on local variables is neither encouraged nor discouraged.

类的所有 `const` 操作都应该可以安全地同时调用。如果这不可行，则必须将该类明确记录为 `线程不安全` 。
All of a class's `const` operations should be safe to invoke concurrently with each other. If that's not feasible, the class must be clearly documented as "thread-unsafe".

### const 放在哪里

### Where to put the const

有些人更喜欢 `int const *foo` 的形式而不是 `const int* foo` 。他们认为这更具可读性，因为它更一致：它保留了 `const` 始终遵循它所描述的对象的规则。然而，这一一致性参数不适用于具有很少深度嵌套指针表达式的代码库，因为大多数 `const` 表达式只有一个 `const` ，并且它适用于基础值。在这种情况下，无法保持一致性。将 `const` 放在前面可以说更具可读性，因为它遵循英语将 "形容词" （ `const` ）放在 "名词" （ `int` ）之前。
Some people favor the form `int const *foo` to `const int* foo`. They argue that this is more readable because it's more consistent: it keeps the rule that `const` always follows the object it's describing. However, this consistency argument doesn't apply in codebases with few deeply-nested pointer expressions since most `const` expressions have only one `const`, and it applies to the underlying value. In such cases, there's no consistency to maintain. Putting the `const` first is arguably more readable, since it follows English in putting the "adjective" (`const`) before the "noun" (`int`).

也就是说，虽然我们鼓励将 `const` 放在第一位，但我们并不要求它。但要与你周围的代码保持一致！
That said, while we encourage putting `const` first, we do not require it. But be consistent with the code around you!

## constexpr 的使用

## Use of constexpr

使用 `constexpr` 定义真正的常量或确保常量初始化。
Use `constexpr` to define true constants or to ensure constant initialization.

某些变量可以声明为 `constexpr` 以指示这些变量是真正的常量，即在编译/链接时固定。某些函数和构造函数可以声明为 `constexpr` ，这使得它们能够用于定义 `constexpr` 变量。
Some variables can be declared `constexpr` to indicate the variables are true constants, i.e., fixed at compilation/link time. Some functions and constructors can be declared `constexpr` which enables them to be used in defining a `constexpr` variable.

使用 `constexpr` 可以使用浮点表达式来定义常量，而不仅仅是字面量；用户定义类型常量的定义；以及带有函数调用的常量定义。
Use of `constexpr` enables definition of constants with floating-point expressions rather than just literals; definition of constants of user-defined types; and definition of constants with function calls.

如果稍后必须降级，过早地将某些内容标记为 `constexpr` 可能会导致迁移问题。当前对 `constexpr` 函数和构造函数中允许的内容的限制可能会在这些定义中引发晦涩的解决方法。
Prematurely marking something as `constexpr` may cause migration problems if later on it has to be downgraded. Current restrictions on what is allowed in `constexpr` functions and constructors may invite obscure workarounds in these definitions.

`constexpr` 定义可以对接口的常量部分进行更稳健的规范。使用 constexpr 指定真正的常量以及支持其定义的函数。避免复杂化函数定义以使其能够与 `constexpr` 一起使用。不要使用 `constexpr` 强制内联。
`constexpr` definitions enable a more robust specification of the constant parts of an interface. Use `constexpr` to specify true constants and the functions that support their definitions. Avoid complexifying function definitions to enable their use with `constexpr`. Do not use `constexpr` to force inlining.

## 整数类型

## Integer Types

在 C++ 内置整数类型中，唯一使用的一种是 `int` 。如果程序需要不同大小的整数类型，请使用 `<cstdint>` 中的精确宽度整数类型，例如 `int16_t` 。如果您的值可能大于或等于 2^31，请使用 64 位类型，例如 `int64_t` 。请记住，即使您的值对于 `int` 来说不会太大，它也可能用于可能需要更大类型的中间计算。如有疑问，请选择较大的类型。
Of the built-in C++ integer types, the only one used is `int`. If a program needs an integer type of a different size, use an exact-width integer type from `<cstdint>`, such as `int16_t`. If you have a value that could ever be greater than or equal to 2^31, use a 64-bit type such as `int64_t`. Keep in mind that even if your value won't ever be too large for an `int`, it may be used in intermediate calculations which may require a larger type. When in doubt, choose a larger type.

C++ 没有指定像 `int` 这样的整数类型的精确大小。当代架构上常见的大小是 16 位 `short` ，32 位 `int` ，32 或 64 位 `long` ，以及 64 位 `long long` ，但不同的平台有不同的选择，特别是 `long`。
C++ does not specify exact sizes for the integer types like `int`. Common sizes on contemporary architectures are 16 bits for `short`, 32 bits for `int`, 32 or 64 bits for `long`, and 64 bits for `long long`, but different platforms make different choices, in particular for `long`.

声明的统一性。
Uniformity of declaration.

C++ 中整型的大小可能因编译器和体系结构而异。
The sizes of integral types in C++ can vary based on compiler and architecture.

标准库头 `<cstdint>` 定义了 `int16_t`、`uint32_t`、`int64_t` 等类型。当您需要时，您应该始终优先使用这些类型而不是 `short`、`unsigned long long` 等对整数大小的保证。在内置整数类型中，仅应使用 `int` 。在适当的时候，欢迎您使用标准类型别名，例如 `size_t` 和 `ptrdiff_t` 。
The standard library header `<cstdint>` defines types like `int16_t`, `uint32_t`, `int64_t`, etc. You should always use those in preference to `short`, `unsigned long long` and the like, when you need a guarantee on the size of an integer. Of the built-in integer types, only `int` should be used. When appropriate, you are welcome to use standard type aliases like `size_t` and `ptrdiff_t`.

我们经常使用 `int` ，因为我们知道整数不会太大，例如循环计数器。对于此类事情，请使用普通的旧 `int` 。您应该假设 `int` 至少为 32 位，但不要假设它超过 32 位。如果您需要 64 位整数类型，请使用 `int64_t` 或 `uint64_t` 。
We use `int` very often, for integers we know are not going to be too big, e.g., loop counters. Use plain old `int` for such things. You should assume that an `int` is at least 32 bits, but don't assume that it has more than 32 bits. If you need a 64-bit integer type, use `int64_t` or `uint64_t`.

对于我们知道可以 `大` 的整数，请使用 `int64_t` 。
For integers we know can be "big", use `int64_t`.

您不应使用无符号整数类型，例如 `uint32_t` ，除非有正当理由（例如表示位模式而不是数字），或者您需要定义溢出模 2^N。特别是，不要使用无符号类型来表示数字永远不会为负数。相反，为此使用断言。
You should not use the unsigned integer types such as `uint32_t`, unless there is a valid reason such as representing a bit pattern rather than a number, or you need defined overflow modulo 2^N. In particular, do not use unsigned types to say a number will never be negative. Instead, use assertions for this.

如果您的代码是返回大小的容器，请确保使用能够适应容器的任何可能用途的类型。如有疑问，请使用较大的类型而不是较小的类型。
If your code is a container that returns a size, be sure to use a type that will accommodate any possible usage of your container. When in doubt, use a larger type rather than a smaller type.

转换整数类型时要小心。整数转换和提升可能会导致未定义的行为，从而导致安全错误和其他问题。
Use care when converting integer types. Integer conversions and promotions can cause undefined behavior, leading to security bugs and other problems.

### 关于无符号整数

### On Unsigned Integers

无符号整数适合表示位域和模算术。由于历史意外，C++ 标准也使用无符号整数来表示容器的大小 - 标准机构的许多成员认为这是一个错误，但目前实际上无法修复。事实上，无符号算术并不模拟简单整数的行为，而是由标准定义来模拟模块化算术（围绕上溢/下溢），这意味着编译器无法诊断一类重要的错误。在其他情况下，定义的行为会阻碍优化。
Unsigned integers are good for representing bitfields and modular arithmetic. Because of historical accident, the C++ standard also uses unsigned integers to represent the size of containers - many members of the standards body believe this to be a mistake, but it is effectively impossible to fix at this point. The fact that unsigned arithmetic doesn't model the behavior of a simple integer, but is instead defined by the standard to model modular arithmetic (wrapping around on overflow/underflow), means that a significant class of bugs cannot be diagnosed by the compiler. In other cases, the defined behavior impedes optimization.

也就是说，混合整数类型的符号会导致同样大的一类问题。我们可以提供的最佳建议：尝试使用迭代器和容器而不是指针和大小，尝试不要混合符号性，并尝试避免无符号类型（表示位域或模算术除外）。不要仅仅使用无符号类型来断言变量是非负的。
That said, mixing signedness of integer types is responsible for an equally large class of problems. The best advice we can provide: try to use iterators and containers rather than pointers and sizes, try not to mix signedness, and try to avoid unsigned types (except for representing bitfields or modular arithmetic). Do not use an unsigned type merely to assert that a variable is non-negative.

## 64 位可移植性

## 64-bit Portability

代码应该对 64 位和 32 位友好。请记住打印、比较和结构对齐的问题。
Code should be 64-bit and 32-bit friendly. Bear in mind problems of printing, comparisons, and structure alignment.

- 某些整型类型定义的正确可移植 `printf()` 转换说明符依赖于宏扩展，我们发现这些宏扩展使用起来不愉快并且要求不切实际（来自 `<cinttypes>` 的 `PRI` 宏）。除非对于您的特定情况没有合理的替代方案，否则请尽量避免甚至升级依赖 `printf` 系列的 API。而是使用支持类型安全数字格式的库，例如 [`StrCat`](https://github.com/abseil/abseil-cpp/blob/master/absl/strings/str_cat.h) 或 [`Substitute`](https://github.com/abseil/abseil-cpp/blob/master/absl/strings/substitute.h) 用于快速简单的转换，或 [`std::ostream`](#Streams)。
- Correct portable `printf()` conversion specifiers for some integral typedefs rely on macro expansions that we find unpleasant to use and impractical to require (the `PRI` macros from `<cinttypes>`). Unless there is no reasonable alternative for your particular case, try to avoid or even upgrade APIs that rely on the `printf` family. Instead use a library supporting typesafe numeric formatting, such as [`StrCat`](https://github.com/abseil/abseil-cpp/blob/master/absl/strings/str_cat.h) or [`Substitute`](https://github.com/abseil/abseil-cpp/blob/master/absl/strings/substitute.h) for fast simple conversions, or [`std::ostream`](#Streams).

  不幸的是， `PRI` 宏是指定标准位宽类型定义（例如 `int64_t` 、 `uint64_t` 、 `int32_t` 、 `uint32_t` 等）转换的唯一可移植方法。如果可能，请避免将位宽 typedef 指定的类型的参数传递给基于 `printf` 的 API。请注意，使用 printf 具有专用长度修饰符的 typedef 是可以接受的，例如 `size_t` （ `z` ）、 `ptrdiff_t` （ `t` ）和 `maxint_t` （ `j` ）。
  Unfortunately, the `PRI` macros are the only portable way to specify a conversion for the standard bitwidth typedefs (e.g., `int64_t`, `uint64_t`, `int32_t`, `uint32_t`, etc). Where possible, avoid passing arguments of types specified by bitwidth typedefs to `printf`\-based APIs. Note that it is acceptable to use typedefs for which printf has dedicated length modifiers, such as `size_t` (`z`), `ptrdiff_t` (`t`), and `maxint_t` (`j`).

- 请记住 `sizeof(void *)` != `sizeof(int)`。如果您想要一个指针大小的整数，请使用 `intptr_t` 。
- Remember that `sizeof(void *)` != `sizeof(int)`. Use `intptr_t` if you want a pointer-sized integer.
- 您可能需要小心结构对齐，特别是对于存储在磁盘上的结构。默认情况下，任何具有 `int64_t`/`uint64_t` 成员的类/结构在 64 位系统上最终都是 8 字节对齐的。如果在磁盘上有这样的结构在 32 位和 64 位代码之间共享，则需要确保它们在两种体系结构上的打包方式相同。大多数编译器提供了一种改变结构对齐的方法。对于 gcc，您可以使用 `__attribute__((packed))`。 MSVC 提供 `#pragma pack()` 和 `__declspec(align())` 。
- You may need to be careful with structure alignments, particularly for structures being stored on disk. Any class/structure with a `int64_t`/`uint64_t` member will by default end up being 8-byte aligned on a 64-bit system. If you have such structures being shared on disk between 32-bit and 64-bit code, you will need to ensure that they are packed the same on both architectures. Most compilers offer a way to alter structure alignment. For gcc, you can use `__attribute__((packed))`. MSVC offers `#pragma pack()` and `__declspec(align())`.
- 根据需要使用 [braced-initialization](#Casting) 创建 64 位常量。例如：
- Use [braced-initialization](#Casting) as needed to create 64-bit constants. For example:

  ```C++
  int64_t my_value{0x123456789};
  uint64_t my_mask{uint64_t{3} << 48};
  ```

## 预处理器宏

## Preprocessor Macros

避免定义宏，尤其是在标头中；更喜欢内联函数、枚举和 `const` 变量。使用特定于项目的前缀命名宏。不要使用宏来定义 C++ API 的各个部分。
Avoid defining macros, especially in headers; prefer inline functions, enums, and `const` variables. Name macros with a project-specific prefix. Do not use macros to define pieces of a C++ API.

宏意味着您看到的代码与编译器看到的代码不同。这可能会引入意外的行为，特别是因为宏具有全局作用域。
Macros mean that the code you see is not the same as the code the compiler sees. This can introduce unexpected behavior, especially since macros have global scope.

当宏用于定义 C++ API 的各个部分时，宏带来的问题尤其严重，对于公共 API 来说更是如此。当开发人员错误地使用该接口时，编译器发出的每条错误消息现在都必须解释宏如何形成接口。重构和分析工具更新界面的难度要大得多。因此，我们明确禁止以这种方式使用宏。例如，避免以下模式：
The problems introduced by macros are especially severe when they are used to define pieces of a C++ API, and still more so for public APIs. Every error message from the compiler when developers incorrectly use that interface now must explain how the macros formed the interface. Refactoring and analysis tools have a dramatically harder time updating the interface. As a consequence, we specifically disallow using macros in this way. For example, avoid patterns like:

**badcode**

```C++
class WOMBAT_TYPE(Foo) {
  // ...

 public:
  EXPAND_PUBLIC_WOMBAT_API(Foo)

  EXPAND_WOMBAT_COMPARISONS(Foo, ==, <)
};
```

幸运的是，宏在 C++ 中并不像在 C 中那样必要。不要使用宏来内联性能关键型代码，而是使用内联函数。不要使用宏来存储常量，而是使用 `const` 变量。不要使用宏来 "缩写" 长变量名，而是使用引用。不要使用宏来有条件地编译代码……好吧，根本不要这样做（当然，除了 `#define` 防护以防止头文件的双重包含）。它使测试变得更加困难。
Luckily, macros are not nearly as necessary in C++ as they are in C. Instead of using a macro to inline performance-critical code, use an inline function. Instead of using a macro to store a constant, use a `const` variable. Instead of using a macro to "abbreviate" a long variable name, use a reference. Instead of using a macro to conditionally compile code ... well, don't do that at all (except, of course, for the `#define` guards to prevent double inclusion of header files). It makes testing much more difficult.

宏可以做其他技术无法完成的事情，并且您确实在代码库中看到它们，尤其是在较低级别的库中。它们的一些特殊功能（如字符串化、连接等）无法通过语言本身获得。但在使用宏之前，请仔细考虑是否有非宏方法可以达到相同的结果。如果您需要使用宏来定义接口，请联系您的项目负责人请求放弃此规则。
Macros can do things these other techniques cannot, and you do see them in the codebase, especially in the lower-level libraries. And some of their special features (like stringifying, concatenation, and so forth) are not available through the language proper. But before using a macro, consider carefully whether there's a non-macro way to achieve the same result. If you need to use a macro to define an interface, contact your project leads to request a waiver of this rule.

以下使用模式将避免宏的许多问题；如果您使用宏，请尽可能遵循它：
The following usage pattern will avoid many problems with macros; if you use macros, follow it whenever possible:

- 不要在`.h`文件中定义宏。
- Don't define macros in a `.h` file.
- 在使用宏之前使用 `#define` 宏，在使用宏之后使用 `#undef` 宏。
- `#define` macros right before you use them, and `#undef` them right after.
- 在用您自己的宏替换之前，不要只是 `#undef` 现有的宏；相反，选择一个可能是唯一的名称。
- Do not just `#undef` an existing macro before replacing it with your own; instead, pick a name that's likely to be unique.
- 尽量不要使用扩展为不平衡 C++ 结构的宏，或者至少很好地记录该行为。
- Try not to use macros that expand to unbalanced C++ constructs, or at least document that behavior well.
- 最好不要使用 `##` 来生成函数/类/变量名称。
- Prefer not using `##` to generate function/class/variable names.

强烈建议不要从标头中导出宏（即在标头中定义它们，而不在标头末尾之前对其进行 `#undef` ）。如果确实从标头导出宏，则它必须具有全局唯一的名称。为了实现这一点，它必须使用由项目的命名空间名称（但大写）组成的前缀来命名。
Exporting macros from headers (i.e., defining them in a header without `#undef`ing them before the end of the header) is extremely strongly discouraged. If you do export a macro from a header, it must have a globally unique name. To achieve this, it must be named with a prefix consisting of your project's namespace name (but upper case).

## 0 和 nullptr/NULL

## 0 and nullptr/NULL

对指针使用 `nullptr` ，对字符使用 `\0` （而不是 `0` 字面量）。
Use `nullptr` for pointers, and `'\0'` for chars (and not the `0` literal).

对于指针（地址值），请使用 `nullptr` ，因为这提供了类型安全。
For pointers (address values), use `nullptr`, as this provides type-safety.

使用 `\0` 作为空字符。使用正确的类型可以使代码更具可读性。
Use `'\0'` for the null character. Using the correct type makes the code more readable.

## sizeof

## sizeof

更喜欢 `sizeof(varname)` 而不是 `sizeof(type)` 。
Prefer `sizeof(varname)` to `sizeof(type)`.

当您获取特定变量的大小时，请使用 `sizeof(varname)` 。如果有人现在或以后更改变量类型， `sizeof(varname)` 将适当更新。您可以将 `sizeof(type)` 用于与任何特定变量无关的代码，例如管理外部或内部数据格式的代码，其中不方便使用适当的 C++ 类型的变量。
Use `sizeof(varname)` when you take the size of a particular variable. `sizeof(varname)` will update appropriately if someone changes the variable type either now or later. You may use `sizeof(type)` for code unrelated to any particular variable, such as code that manages an external or internal data format where a variable of an appropriate C++ type is not convenient.

```C++
MyStruct data;
memset(&data, 0, sizeof(data));
```

**badcode**

```C++
memset(&data, 0, sizeof(MyStruct));
```

```C++
if (raw_size < sizeof(int)) {
  LOG(ERROR) << "compressed record not big enough for count: " << raw_size;
  return false;
}
```

## 类型推导（包括 auto）

## Type Deduction (including auto)

仅当类型推导使不熟悉项目的读者更清楚代码或者使代码更安全时才使用类型推导。不要仅仅为了避免编写显式类型带来的不便而使用它。
Use type deduction only if it makes the code clearer to readers who aren't familiar with the project, or if it makes the code safer. Do not use it merely to avoid the inconvenience of writing an explicit type.

在多种情况下，C++ 允许（甚至要求）编译器推导类型，而不是在代码中显式说明：
There are several contexts in which C++ allows (or even requires) types to be deduced by the compiler, rather than spelled out explicitly in the code:

### [函数模板参数推导](https://en.cppreference.com/w/cpp/language/template_argument_deduction)

### [Function template argument deduction](https://en.cppreference.com/w/cpp/language/template_argument_deduction)

无需显式模板参数即可调用函数模板。编译器根据函数参数的类型推导这些参数：
A function template can be invoked without explicit template arguments. The compiler deduces those arguments from the types of the function arguments:

**neutralcode**

```C++
template <typename T>
void f(T t);

f(0);  // Invokes f<int>(0)
```

### [`auto` 变量声明](https://en.cppreference.com/w/cpp/language/auto)

### [`auto` variable declarations](https://en.cppreference.com/w/cpp/language/auto)

变量声明可以使用 `auto` 关键字来代替类型。编译器从变量的初始值设定项推导类型，遵循与具有相同初始值设定项的函数模板参数推导相同的规则（只要不使用花括号而不是括号）。
A variable declaration can use the `auto` keyword in place of the type. The compiler deduces the type from the variable's initializer, following the same rules as function template argument deduction with the same initializer (so long as you don't use curly braces instead of parentheses).

**neutralcode**

```C++
auto a = 42;  // a is an int
auto& b = a;  // b is an int&
auto c = b;   // c is an int
auto d{42};   // d is an int, not a std::initializer_list<int>
```

`auto` 可以用 `const` 限定，并且可以用作指针或引用类型的一部分，但不能用作模板参数。此语法的一个罕见变体使用 `decltype(auto)` 而不是 `auto`，在这种情况下，推导的类型是应用 [`decltype`](https://en.cppreference.com/w/cpp/language/decltype) 到初始化器。
`auto` can be qualified with `const`, and can be used as part of a pointer or reference type, but it can't be used as a template argument. A rare variant of this syntax uses `decltype(auto)` instead of `auto`, in which case the deduced type is the result of applying [`decltype`](https://en.cppreference.com/w/cpp/language/decltype) to the initializer.

### [函数返回类型推导](https://en.cppreference.com/w/cpp/language/function#Return_type_deduction)

### [Function return type deduction](https://en.cppreference.com/w/cpp/language/function#Return_type_deduction)

`auto`（和 `decltype(auto)`）也可以用来代替函数返回类型。编译器从函数体中的 `return` 语句推导出返回类型，遵循与变量声明相同的规则：
`auto` (and `decltype(auto)`) can also be used in place of a function return type. The compiler deduces the return type from the `return` statements in the function body, following the same rules as for variable declarations:

**neutralcode**

```C++
auto f() { return 0; }  // The return type of f is int
```

_Lambda 表达式_ 返回类型可以用相同的方式推导，但这是通过省略返回类型来触发的，而不是通过显式的 `auto` 来触发。令人困惑的是，函数的尾随返回类型语法也在返回类型位置使用 `auto` ，但这并不依赖于类型推导；它只是显式返回类型的替代语法。
_Lambda expression_ return types can be deduced in the same way, but this is triggered by omitting the return type, rather than by an explicit `auto`. Confusingly, _trailing return type_ syntax for functions also uses `auto` in the return-type position, but that doesn't rely on type deduction; it's just an alternate syntax for an explicit return type.

### [泛型 lambda](https://isocpp.org/wiki/faq/cpp14-language#generic-lambdas)

### [Generic lambdas](https://isocpp.org/wiki/faq/cpp14-language#generic-lambdas)

lambda 表达式可以使用 `auto` 关键字代替其一个或多个参数类型。这导致 lambda 的调用运算符成为函数模板而不是普通函数，每个 `auto` 函数参数都有一个单独的模板参数：
A lambda expression can use the `auto` keyword in place of one or more of its parameter types. This causes the lambda's call operator to be a function template instead of an ordinary function, with a separate template parameter for each `auto` function parameter:

**neutralcode**

```C++
// Sort `vec` in decreasing order
std::sort(vec.begin(), vec.end(), [](auto lhs, auto rhs) { return lhs > rhs; });
```

### [Lambda 初始化捕获](https://isocpp.org/wiki/faq/cpp14-language#lambda-captures)

### [Lambda init captures](https://isocpp.org/wiki/faq/cpp14-language#lambda-captures)

Lambda 捕获可以具有显式初始化程序，可用于声明全新的变量，而不仅仅是捕获现有变量：
Lambda captures can have explicit initializers, which can be used to declare wholly new variables rather than only capturing existing ones:

**neutralcode**

```C++
[x = 42, y = "foo"] { ... }  // x is an int, and y is a const char*
```

此语法不允许指定类型；相反，它是使用 `auto` 变量的规则推导出来的。
This syntax doesn't allow the type to be specified; instead, it's deduced using the rules for `auto` variables.

### [类模板参数推导](https://en.cppreference.com/w/cpp/language/class_template_argument_deduction)

### [Class template argument deduction](https://en.cppreference.com/w/cpp/language/class_template_argument_deduction)

见 _下文_。
See _below_.

### [结构化绑定](https://en.cppreference.com/w/cpp/language/structured_binding)

### [Structured bindings](https://en.cppreference.com/w/cpp/language/structured_binding)

当使用 `auto` 声明元组、结构体或数组时，您可以指定单个元素的名称，而不是整个对象的名称；这些名称称为 "结构化绑定" ，整个声明称为 "结构化绑定声明" 。此语法无法指定封闭对象或单个名称的类型：

When declaring a tuple, struct, or array using `auto`, you can specify names for the individual elements instead of a name for the whole object; these names are called "structured bindings", and the whole declaration is called a "structured binding declaration". This syntax provides no way of specifying the type of either the enclosing object or the individual names:

**neutralcode**

```C++
auto [iter, success] = my_map.insert({key, value});
if (!success) {
  iter->second = value;
}
```

`auto` 也可以用 `const`、`&` 和 `&&` 进行限定，但请注意，这些限定符在技术上适用于匿名元组/结构体/数组，而不是单个绑定。确定绑定类型的规则相当复杂；结果往往并不令人惊讶，只是即使声明声明了引用，绑定类型通常也不会是引用（但无论如何它们通常都会表现得像引用）。
The `auto` can also be qualified with `const`, `&`, and `&&`, but note that these qualifiers technically apply to the anonymous tuple/struct/array, rather than the individual bindings. The rules that determine the types of the bindings are quite complex; the results tend to be unsurprising, except that the binding types typically won't be references even if the declaration declares a reference (but they will usually behave like references anyway).

（这些摘要省略了许多细节和警告；请参阅链接以获取更多信息。）
(These summaries omit many details and caveats; see the links for further information.)

- C++ 类型名称可能又长又麻烦，尤其是当它们涉及模板或命名空间时。
- C++ type names can be long and cumbersome, especially when they involve templates or namespaces.
- 当 C++ 类型名称在单个声明或小代码区域中重复时，重复可能不会提高可读性。
- When a C++ type name is repeated within a single declaration or a small code region, the repetition may not be aiding readability.
- 有时推导类型会更安全，因为这可以避免意外复制或类型转换的可能性。
- It is sometimes safer to let the type be deduced, since that avoids the possibility of unintended copies or type conversions.

当类型显式时，C++ 代码通常会更清晰，尤其是当类型推导依赖于代码中较远部分的信息时。在这样的表达中：
C++ code is usually clearer when types are explicit, especially when type deduction would depend on information from distant parts of the code. In expressions like:

**badcode**

```C++
auto foo = x.add_foo();
auto i = y.Find(key);
```

如果 `y` 的类型不太为人所知，或者 `y` 已在多行之前声明，那么结果类型可能并不明显。
it may not be obvious what the resulting types are if the type of `y` isn't very well known, or if `y` was declared many lines earlier.

程序员必须了解类型推导何时会或不会产生引用类型，否则他们会在无意时获得副本。
Programmers have to understand when type deduction will or won't produce a reference type, or they'll get copies when they didn't mean to.

如果推导类型用作接口的一部分，那么程序员可能会在仅打算更改其值的同时更改其类型，从而导致比预期更彻底的 API 更改。
If a deduced type is used as part of an interface, then a programmer might change its type while only intending to change its value, leading to a more radical API change than intended.

基本规则是：仅使用类型推导使代码更清晰或更安全，而不要仅仅为了避免编写显式类型的不便而使用它。在判断代码是否更清晰时，请记住，您的读者不一定是您的团队成员，也不一定熟悉您的项目，因此您和您的审阅者认为不必要的混乱的类型通常会为其他人提供有用的信息。例如，您可以假设 `make_unique<Foo>()` 的返回类型是显而易见的，但 `MyWidgetFactory()` 的返回类型可能不是。
The fundamental rule is: use type deduction only to make the code clearer or safer, and do not use it merely to avoid the inconvenience of writing an explicit type. When judging whether the code is clearer, keep in mind that your readers are not necessarily on your team, or familiar with your project, so types that you and your reviewer experience as unnecessary clutter will very often provide useful information to others. For example, you can assume that the return type of `make_unique<Foo>()` is obvious, but the return type of `MyWidgetFactory()` probably isn't.

这些原则适用于所有形式的类型推导，但细节有所不同，如以下各节所述。
These principles apply to all forms of type deduction, but the details vary, as described in the following sections.

### 函数模板参数推导

### Function template argument deduction

函数模板参数推导几乎总是可以的。类型推导是与函数模板交互的预期默认方式，因为它允许函数模板像无限的普通函数重载集一样工作。因此，函数模板的设计几乎总是使模板参数推导清晰且安全，否则无法编译。
Function template argument deduction is almost always OK. Type deduction is the expected default way of interacting with function templates, because it allows function templates to act like infinite sets of ordinary function overloads. Consequently, function templates are almost always designed so that template argument deduction is clear and safe, or doesn't compile.

### 局部变量类型推导

### Local variable type deduction

对于局部变量，可以使用类型推导，通过消除明显或不相关的类型信息来使代码更加清晰，以便读者可以专注于代码中有意义的部分：
For local variables, you can use type deduction to make the code clearer by eliminating type information that is obvious or irrelevant, so that the reader can focus on the meaningful parts of the code:

**neutralcode**

```C++
std::unique_ptr<WidgetWithBellsAndWhistles> widget =
    std::make_unique<WidgetWithBellsAndWhistles>(arg1, arg2);
absl::flat_hash_map<std::string,
                    std::unique_ptr<WidgetWithBellsAndWhistles>>::const_iterator
    it = my_map_.find(key);
std::array<int, 6> numbers = {4, 8, 15, 16, 23, 42};
```

**goodcode**

```C++
auto widget = std::make_unique<WidgetWithBellsAndWhistles>(arg1, arg2);
auto it = my_map_.find(key);
std::array numbers = {4, 8, 15, 16, 23, 42};
```

类型有时包含有用信息和样板文件的混合体，例如上面示例中的 `it` ：很明显，该类型是一个迭代器，并且在许多上下文中，容器类型甚至键类型都不相关，但类型这些值可能很有用。在这种情况下，通常可以使用传达相关信息的显式类型来定义局部变量：
Types sometimes contain a mixture of useful information and boilerplate, such as `it` in the example above: it's obvious that the type is an iterator, and in many contexts the container type and even the key type aren't relevant, but the type of the values is probably useful. In such situations, it's often possible to define local variables with explicit types that convey the relevant information:

**goodcode**

```C++
if (auto it = my_map_.find(key); it != my_map_.end()) {
  WidgetWithBellsAndWhistles& widget = *it->second;
  // Do stuff with `widget`
}
```

如果类型是模板实例，并且参数是样板文件，但模板本身提供信息，则可以使用类模板参数推导来抑制样板文件。然而，这实际上提供了有意义的好处的情况非常罕见。请注意，类模板参数推导也受*单独的样式规则*的约束。
If the type is a template instance, and the parameters are boilerplate but the template itself is informative, you can use class template argument deduction to suppress the boilerplate. However, cases where this actually provides a meaningful benefit are quite rare. Note that class template argument deduction is also subject to a _separate style rule_.

如果更简单的选项可以工作，请不要使用 `decltype(auto)` ，因为它是一个相当晦涩的功能，因此在代码清晰度方面的成本很高。
Do not use `decltype(auto)` if a simpler option will work, because it's a fairly obscure feature, so it has a high cost in code clarity.

### 返回类型推导

### Return type deduction

仅当函数体只有很少数量的 return 语句且其他代码很少时才使用返回类型推导（对于函数和 lambda），因为否则读者可能无法一眼看出返回类型是。此外，仅当函数或 lambda 的范围非常窄时才使用它，因为具有推导返回类型的函数不定义抽象边界：实现就是接口。特别是，头文件中的公共函数几乎不应该推导返回类型。
Use return type deduction (for both functions and lambdas) only if the function body has a very small number of `return` statements, and very little other code, because otherwise the reader may not be able to tell at a glance what the return type is. Furthermore, use it only if the function or lambda has a very narrow scope, because functions with deduced return types don't define abstraction boundaries: the implementation _is_ the interface. In particular, public functions in header files should almost never have deduced return types.

### 参数类型推导

### Parameter type deduction

应谨慎使用 lambda 的 `auto` 参数类型，因为实际类型由调用 lambda 的代码确定，而不是由 lambda 的定义确定。因此，显式类型几乎总是更清晰，除非 lambda 显式调用的位置非常接近其定义位置（以便读者可以轻松地看到两者），或者 lambda 被传递到一个众所周知的接口，以至于很明显什么参数它最终将被调用（例如上面的`std::sort`示例）。
`auto` parameter types for lambdas should be used with caution, because the actual type is determined by the code that calls the lambda, rather than by the definition of the lambda. Consequently, an explicit type will almost always be clearer unless the lambda is explicitly called very close to where it's defined (so that the reader can easily see both), or the lambda is passed to an interface so well-known that it's obvious what arguments it will eventually be called with (e.g., the `std::sort` example above).

### Lambda 初始化捕获

### Lambda init captures

初始化捕获由*更具体的样式规则*覆盖，该规则在很大程度上取代了类型推导的一般规则。
Init captures are covered by a _more specific style rule_, which largely supersedes the general rules for type deduction.

### 结构化绑定

### Structured bindings

与其他形式的类型推导不同，结构化绑定实际上可以通过为较大对象的元素提供有意义的名称来为读者提供附加信息。这意味着结构化绑定声明可以比显式类型提供净可读性改进，即使在 `auto` 不能的情况下也是如此。当对象是一对或元组时（如上面的 `插入` 示例），结构化绑定特别有用，因为它们一开始就没有有意义的字段名称，但请注意，通常[不应该使用对或元组](#Structs_vs._Tuples) 除非像 `insert` 这样预先存在的 API 强制您这样做。
Unlike other forms of type deduction, structured bindings can actually give the reader additional information, by giving meaningful names to the elements of a larger object. This means that a structured binding declaration may provide a net readability improvement over an explicit type, even in cases where `auto` would not. Structured bindings are especially beneficial when the object is a pair or tuple (as in the `insert` example above), because they don't have meaningful field names to begin with, but note that you generally [shouldn't use pairs or tuples](#Structs_vs._Tuples) unless a pre-existing API like `insert` forces you to.

如果绑定的对象是一个结构体，有时提供更适合您的使用的名称可能会有所帮助，但请记住，这也可能意味着您的读者比字段名称更难识别这些名称。我们建议使用注释来指示基础字段的名称，如果它与绑定的名称不匹配，请使用与函数参数注释相同的语法：
If the object being bound is a struct, it may sometimes be helpful to provide names that are more specific to your usage, but keep in mind that this may also mean the names are less recognizable to your reader than the field names. We recommend using a comment to indicate the name of the underlying field, if it doesn't match the name of the binding, using the same syntax as for function parameter comments:

```C++
auto [/*field_name1=*/bound_name1, /*field_name2=*/bound_name2] = ...
```

与函数参数注释一样，这可以使工具检测到字段的顺序是否错误。
As with function parameter comments, this can enable tools to detect if you get the order of the fields wrong.

## 类模板参数推导

## Class Template Argument Deduction

仅对已明确选择支持它的模板使用类模板参数推导。
Use class template argument deduction only with templates that have explicitly opted into supporting it.

[类模板参数推导](https://en.cppreference.com/w/cpp/language/class_template_argument_deduction)（通常缩写为 "CTAD" ）当使用命名模板的类型声明变量并且模板参数时发生未提供列表（甚至没有空尖括号）：
[Class template argument deduction](https://en.cppreference.com/w/cpp/language/class_template_argument_deduction) (often abbreviated "CTAD") occurs when a variable is declared with a type that names a template, and the template argument list is not provided (not even empty angle brackets):

**neutralcode**

```C++
std::array a = {1, 2, 3};  // `a` is a std::array<int, 3>
```

编译器使用模板的 "推导指南" 从初始值设定项推导参数，该 "推导指南" 可以是显式的，也可以是隐式的。
The compiler deduces the arguments from the initializer using the template's "deduction guides", which can be explicit or implicit.

显式推导指南看起来像带有尾随返回类型的函数声明，只不过没有前导 `auto` ，并且函数名称是模板的名称。例如，上面的例子依赖于`std::array`的推导指南：
Explicit deduction guides look like function declarations with trailing return types, except that there's no leading `auto`, and the function name is the name of the template. For example, the above example relies on this deduction guide for `std::array`:

**neutralcode**

```C++
namespace std {
template <class T, class... U>
array(T, U...) -> std::array<T, 1 + sizeof...(U)>;
}
```

主模板（与模板专门化相对）中的构造函数也隐式定义推导指南。
Constructors in a primary template (as opposed to a template specialization) also implicitly define deduction guides.

当您声明依赖于 CTAD 的变量时，编译器会使用构造函数重载解析规则选择推导指南，并且该指南的返回类型将成为变量的类型。
When you declare a variable that relies on CTAD, the compiler selects a deduction guide using the rules of constructor overload resolution, and that guide's return type becomes the type of the variable.

CTAD 有时可以允许您从代码中省略样板文件。
CTAD can sometimes allow you to omit boilerplate from your code.

从构造函数生成的隐式推导指南可能具有不良行为，或者完全不正确。对于 C++17 中引入 CTAD 之前编写的构造函数来说，这尤其成问题，因为这些构造函数的作者无法了解（更不用说修复）其构造函数会给 CTAD 造成的任何问题。此外，添加显式推导指南来解决这些问题可能会破坏依赖隐式推导指南的任何现有代码。
The implicit deduction guides that are generated from constructors may have undesirable behavior, or be outright incorrect. This is particularly problematic for constructors written before CTAD was introduced in C++17, because the authors of those constructors had no way of knowing about (much less fixing) any problems that their constructors would cause for CTAD. Furthermore, adding explicit deduction guides to fix those problems might break any existing code that relies on the implicit deduction guides.

CTAD 还存在许多与 `auto` 相同的缺点，因为它们都是从变量的初始值设定项推导变量的全部或部分类型的机制。 CTAD 确实为读者提供了比 `auto` 更多的信息，但它也没有给读者一个明显的提示：信息已被省略。
CTAD also suffers from many of the same drawbacks as `auto`, because they are both mechanisms for deducing all or part of a variable's type from its initializer. CTAD does give the reader more information than `auto`, but it also doesn't give the reader an obvious cue that information has been omitted.

不要将 CTAD 与给定模板一起使用，除非模板的维护者已选择通过提供至少一个显式推导指南来支持 CTAD 的使用（ `std` 命名空间中的所有模板也被假定已选择加入）。如果有的话，应该通过编译器警告来强制执行。
Do not use CTAD with a given template unless the template's maintainers have opted into supporting use of CTAD by providing at least one explicit deduction guide (all templates in the `std` namespace are also presumed to have opted in). This should be enforced with a compiler warning if available.

CTAD 的使用还必须遵循*类型推导*的一般规则。
Uses of CTAD must also follow the general rules on _Type deduction_.

## 指定初始化器

## Designated Initializers

仅以符合 C++20 的形式使用指定的初始值设定项。
Use designated initializers only in their C++20-compliant form.

[指定初始化器](https://en.cppreference.com/w/cpp/language/aggregate_initialization#Designated_initializers) 是一种允许通过显式命名聚合字段来初始化聚合（ `普通旧结构` ）的语法：
[Designated initializers](https://en.cppreference.com/w/cpp/language/aggregate_initialization#Designated_initializers) are a syntax that allows for initializing an aggregate ("plain old struct") by naming its fields explicitly:

**neutralcode**

```C++
  struct Point {
    float x = 0.0;
    float y = 0.0;
    float z = 0.0;
  };

  Point p = {
    .x = 1.0,
    .y = 2.0,
    // z will be 0.0
  };
```

显式列出的字段将按照指定进行初始化，其他字段将按照传统聚合初始化表达式（如 `Point{1.0, 2.0}` ）中的相同方式进行初始化。
The explicitly listed fields will be initialized as specified, and others will be initialized in the same way they would be in a traditional aggregate initialization expression like `Point{1.0, 2.0}`.

指定的初始值设定项可以提供方便且高度可读的聚合表达式，特别是对于字段排序不如上面的 `Point` 示例那么简单的结构。
Designated initializers can make for convenient and highly readable aggregate expressions, especially for structs with less straightforward ordering of fields than the `Point` example above.

虽然指定初始值设定项长期以来一直是 C 标准的一部分，并作为扩展受到 C++ 编译器的支持，但直到最近它们才纳入 C++ 标准，并作为 C++20 的一部分添加。
While designated initializers have long been part of the C standard and supported by C++ compilers as an extension, only recently have they made it into the C++ standard, being added as part of C++20.

C++ 标准中的规则比 C 和编译器扩展中的规则更严格，要求指定的初始值设定项的出现顺序与结构定义中字段的出现顺序相同。因此，在上面的示例中，根据 C++20，初始化 `x` 然后初始化 `z` 是合法的，但初始化 `y` 然后初始化 `x` 则不合法。
The rules in the C++ standard are stricter than in C and compiler extensions, requiring that the designated initializers appear in the same order as the fields appear in the struct definition. So in the example above, it is legal according to C++20 to initialize `x` and then `z`, but not `y` and then `x`.

仅以与 C++20 标准兼容的形式使用指定的初始值设定项：初始值设定项的顺序与结构定义中出现的相应字段的顺序相同。
Use designated initializers only in the form that is compatible with the C++20 standard: with initializers in the same order as the corresponding fields appear in the struct definition.

## Lambda 表达式

## Lambda Expressions

在适当的地方使用 lambda 表达式。当 lambda 将逃逸当前作用域时，首选显式捕获。
Use lambda expressions where appropriate. Prefer explicit captures when the lambda will escape the current scope.

Lambda 表达式是创建匿名函数对象的一种简洁方式。当将函数作为参数传递时，它们通常很有用。例如：
Lambda expressions are a concise way of creating anonymous function objects. They're often useful when passing functions as arguments. For example:

```C++
std::sort(v.begin(), v.end(), [](int x, int y) {
  return Weight(x) < Weight(y);
});
```

它们还允许从封闭范围中显式地按名称捕获变量，或隐式地使用默认捕获。显式捕获要求将每个变量作为值或引用捕获列出：
They further allow capturing variables from the enclosing scope either explicitly by name, or implicitly using a default capture. Explicit captures require each variable to be listed, as either a value or reference capture:

```C++
int weight = 3;
int sum = 0;
// Captures `weight` by value and `sum` by reference.
std::for_each(v.begin(), v.end(), [weight, &sum](int x) {
  sum += weight * x;
});
```

默认捕获隐式捕获 lambda 主体中引用的任何变量，包括 `this` （如果使用了任何成员）：
Default captures implicitly capture any variable referenced in the lambda body, including `this` if any members are used:

```C++
const std::vector<int> lookup_table = ...;
std::vector<int> indices = ...;
// Captures `lookup_table` by reference, sorts `indices` by the value
// of the associated element in `lookup_table`.
std::sort(indices.begin(), indices.end(), [&](int a, int b) {
  return lookup_table[a] < lookup_table[b];
});
```

变量捕获还可以有一个显式初始化程序，它可用于按值捕获仅移动变量，或用于普通引用或值捕获无法处理的其他情况：
A variable capture can also have an explicit initializer, which can be used for capturing move-only variables by value, or for other situations not handled by ordinary reference or value captures:

```C++
std::unique_ptr<Foo> foo = ...;
[foo = std::move(foo)] () {
  ...
}
```

此类捕获（通常称为"init 捕获"或"广义 lambda 捕获"）实际上不需要从封闭范围中"捕获"任何内容，甚至不需要具有封闭范围中的名称；此语法是定义 lambda 对象成员的完全通用方法：
Such captures (often called "init captures" or "generalized lambda captures") need not actually "capture" anything from the enclosing scope, or even have a name from the enclosing scope; this syntax is a fully general way to define members of a lambda object:

**neutralcode**

```C++
[foo = std::vector<int>({1, 2, 3})] () {
  ...
}
```

使用与 `auto` 相同的规则推导带有初始值设定项的捕获类型。
The type of a capture with an initializer is deduced using the same rules as `auto`.

- Lambda 比定义要传递给 STL 算法的函数对象的其他方法要简洁得多，这可以提高可读性。
- Lambdas are much more concise than other ways of defining function objects to be passed to STL algorithms, which can be a readability improvement.
- 适当使用默认捕获可以消除冗余并突出显示默认情况下的重要异常。
- Appropriate use of default captures can remove redundancy and highlight important exceptions from the default.
- Lambdas、`std::function` 和 `std::bind` 可以组合使用作为通用回调机制；它们使编写将绑定函数作为参数的函数变得容易。
- Lambdas, `std::function`, and `std::bind` can be used in combination as a general purpose callback mechanism; they make it easy to write functions that take bound functions as arguments.

- lambda 中的变量捕获可能是悬空指针错误的根源，特别是当 lambda 逃逸当前作用域时。
- Variable capture in lambdas can be a source of dangling-pointer bugs, particularly if a lambda escapes the current scope.
- 默认按值捕获可能会产生误导，因为它们不能防止悬空指针错误。按值捕获指针不会导致深层复制，因此它通常具有与按引用捕获相同的生命周期问题。当按值捕获 `this` 时，这尤其令人困惑，因为 `this` 的使用通常是隐式的。
- Default captures by value can be misleading because they do not prevent dangling-pointer bugs. Capturing a pointer by value doesn't cause a deep copy, so it often has the same lifetime issues as capture by reference. This is especially confusing when capturing `this` by value, since the use of `this` is often implicit.
- 捕获实际上声明了新变量（无论捕获是否具有初始值设定项），但它们看起来与 C++ 中的任何其他变量声明语法完全不同。特别是，没有地方容纳变量的类型，甚至没有 `auto` 占位符（尽管 init 捕获可以间接指示它，例如通过强制转换）。这使得甚至很难将它们识别为声明。
- Captures actually declare new variables (whether or not the captures have initializers), but they look nothing like any other variable declaration syntax in C++. In particular, there's no place for the variable's type, or even an `auto` placeholder (although init captures can indicate it indirectly, e.g., with a cast). This can make it difficult to even recognize them as declarations.
- Init 捕获本质上依赖于*类型推导*，并且具有许多与 `auto` 相同的缺点，还有一个额外的问题，即语法甚至没有提示读者正在进行推导。
- Init captures inherently rely on _type deduction_, and suffer from many of the same drawbacks as `auto`, with the additional problem that the syntax doesn't even cue the reader that deduction is taking place.
- lambda 的使用可能会失控；太长的嵌套匿名函数会使代码更难理解。
- It's possible for use of lambdas to get out of hand; very long nested anonymous functions can make code harder to understand.

- 在适当的情况下使用 lambda 表达式，格式设置如下所述。
- Use lambda expressions where appropriate, with formatting as described _below_.
- 如果 lambda 可能逃逸当前范围，则首选显式捕获。例如，代替：
- Prefer explicit captures if the lambda may escape the current scope. For example, instead of:

  **badcode**

  ```C++
  {
    Foo foo;
    ...
    executor->Schedule([&] { Frobnicate(foo); })
    ...
  }
  // BAD! The fact that the lambda makes use of a reference to `foo` and
  // possibly `this` (if `Frobnicate` is a member function) may not be
  // apparent on a cursory inspection. If the lambda is invoked after
  // the function returns, that would be bad, because both `foo`
  // and the enclosing object could have been destroyed.
  ```

  更喜欢写：
  prefer to write:

  ```C++
  {
    Foo foo;
    ...
    executor->Schedule([&foo] { Frobnicate(foo); })
    ...
  }
  // BETTER - The compile will fail if `Frobnicate` is a member
  // function, and it's clearer that `foo` is dangerously captured by
  // reference.
  ```

- 仅当 lambda 的生命周期明显短于任何潜在捕获时，才使用默认的引用捕获 (`[&]`)。
- Use default capture by reference (`[&]`) only when the lifetime of the lambda is obviously shorter than any potential captures.
- 仅使用默认的按值捕获 (`[=]`) 作为为短 lambda 绑定一些变量的方法，其中捕获的变量集一目了然，并且不会导致隐式捕获 `this` 。 （这意味着出现在非静态类成员函数中并在其主体中引用非静态类成员的 lambda 必须显式捕获 `this` 或通过 `[&]` 捕获。） 最好不要编写长或复杂的 lambda 默认按值捕获。
- Use default capture by value (`[=]`) only as a means of binding a few variables for a short lambda, where the set of captured variables is obvious at a glance, and which does not result in capturing `this` implicitly. (That means that a lambda that appears in a non-static class member function and refers to non-static class members in its body must capture `this` explicitly or via `[&]`.) Prefer not to write long or complex lambdas with default capture by value.
- 仅使用捕获来实际捕获封闭范围中的变量。不要将捕获与初始值设定项一起使用来引入新名称，或大幅更改现有名称的含义。相反，以传统方式声明一个新变量，然后捕获它，或者避免使用 lambda 简写并显式定义一个函数对象。
- Use captures only to actually capture variables from the enclosing scope. Do not use captures with initializers to introduce new names, or to substantially change the meaning of an existing name. Instead, declare a new variable in the conventional way and then capture it, or avoid the lambda shorthand and define a function object explicitly.
- 有关指定参数和返回类型的指导，请参阅*类型推导*部分。
- See the section on _type deduction_ for guidance on specifying the parameter and return types.

## 模板元编程

## Template Metaprogramming

避免复杂的模板编程。
Avoid complicated template programming.

模板元编程是指利用 C++ 模板实例化机制是图灵完备这一事实的一系列技术，可用于在类型域中执行任意编译时计算。
Template metaprogramming refers to a family of techniques that exploit the fact that the C++ template instantiation mechanism is Turing complete and can be used to perform arbitrary compile-time computation in the type domain.

模板元编程允许极其灵活的接口，类型安全且高性能。如果没有它，像 [GoogleTest](https://github.com/google/googletest)、`std::tuple`、`std::function` 和 Boost.Spirit 这样的工具是不可能的。
Template metaprogramming allows extremely flexible interfaces that are type safe and high performance. Facilities like [GoogleTest](https://github.com/google/googletest), `std::tuple`, `std::function`, and Boost.Spirit would be impossible without it.

除了语言专家之外，模板元编程中使用的技术通常对任何人来说都是晦涩难懂的。以复杂方式使用模板的代码通常不可读，并且难以调试或维护。
The techniques used in template metaprogramming are often obscure to anyone but language experts. Code that uses templates in complicated ways is often unreadable, and is hard to debug or maintain.

模板元编程通常会导致极其糟糕的编译时错误消息：即使接口很简单，当用户做错事情时，复杂的实现细节也会变得可见。
Template metaprogramming often leads to extremely poor compile time error messages: even if an interface is simple, the complicated implementation details become visible when the user does something wrong.

模板元编程使重构工具的工作变得更加困难，从而干扰大规模重构。首先，模板代码在多个上下文中扩展，并且很难验证转换在所有上下文中是否有意义。其次，一些重构工具使用的 AST 只表示模板扩展后的代码结构。自动恢复到需要重写的原始源结构可能很困难。
Template metaprogramming interferes with large scale refactoring by making the job of refactoring tools harder. First, the template code is expanded in multiple contexts, and it's hard to verify that the transformation makes sense in all of them. Second, some refactoring tools work with an AST that only represents the structure of the code after template expansion. It can be difficult to automatically work back to the original source construct that needs to be rewritten.

模板元编程有时可以提供比没有它时更干净、更易于使用的界面，但它也常常会让人变得过于聪明。它最好用于少量的低级组件，其中额外的维护负担分散在大量的用途中。
Template metaprogramming sometimes allows cleaner and easier-to-use interfaces than would be possible without it, but it's also often a temptation to be overly clever. It's best used in a small number of low level components where the extra maintenance burden is spread out over a large number of uses.

在使用模板元编程或其他复杂的模板技术之前请三思；考虑一下在你切换到另一个项目后，你的团队的普通成员是否能够很好地理解你的代码来维护它，或者非 C++ 程序员或随意浏览代码库的人是否能够理解错误消息或者跟踪他们想要调用的函数的流程。如果您使用递归模板实例化或类型列表或元函数或表达式模板，或者依赖 SFINAE 或 `sizeof` 技巧来检测函数重载解析，那么您很可能走得太远了。
Think twice before using template metaprogramming or other complicated template techniques; think about whether the average member of your team will be able to understand your code well enough to maintain it after you switch to another project, or whether a non-C++ programmer or someone casually browsing the code base will be able to understand the error messages or trace the flow of a function they want to call. If you're using recursive template instantiations or type lists or metafunctions or expression templates, or relying on SFINAE or on the `sizeof` trick for detecting function overload resolution, then there's a good chance you've gone too far.

如果您使用模板元编程，您应该付出相当大的努力来最小化和隔离复杂性。您应该尽可能将元编程隐藏为实现细节，以便面向用户的标头可读，并且您应该确保对棘手的代码进行了很好的注释。您应该仔细记录代码的使用方式，并且应该说明 "生成的" 代码是什么样子的。当用户犯错时，请特别注意编译器发出的错误消息。错误消息是用户界面的一部分，您的代码应该根据需要进行调整，以便从用户的角度来看错误消息是可以理解和可操作的。
If you use template metaprogramming, you should expect to put considerable effort into minimizing and isolating the complexity. You should hide metaprogramming as an implementation detail whenever possible, so that user-facing headers are readable, and you should make sure that tricky code is especially well commented. You should carefully document how the code is used, and you should say something about what the "generated" code looks like. Pay extra attention to the error messages that the compiler emits when users make mistakes. The error messages are part of your user interface, and your code should be tweaked as necessary so that the error messages are understandable and actionable from a user point of view.

## Boost

## Boost

仅使用 Boost 库集合中经批准的库。
Use only approved libraries from the Boost library collection.

[Boost 库集合](https://www.boost.org/) 是经过同行评审的免费开源 C++ 库的流行集合。
The [Boost library collection](https://www.boost.org/) is a popular collection of peer-reviewed, free, open-source C++ libraries.

Boost 代码通常质量非常高，具有广泛的可移植性，并且填补了 C++ 标准库中的许多重要空白，例如类型特征和更好的绑定器。
Boost code is generally very high-quality, is widely portable, and fills many important gaps in the C++ standard library, such as type traits and better binders.

一些 Boost 库鼓励可能妨碍可读性的编码实践，例如元编程和其他高级模板技术，以及过度 "函数式" 的编程风格。
Some Boost libraries encourage coding practices which can hamper readability, such as metaprogramming and other advanced template techniques, and an excessively "functional" style of programming.

为了让所有可能阅读和维护代码的贡献者保持高水平的可读性，我们只允许经过批准的 Boost 功能子集。目前，允许使用以下库：
In order to maintain a high level of readability for all contributors who might read and maintain code, we only allow an approved subset of Boost features. Currently, the following libraries are permitted:

- [呼叫特征](https://www.boost.org/libs/utility/call_traits.htm) 来自 `boost/call_traits.hpp`
- [Call Traits](https://www.boost.org/libs/utility/call_traits.htm) from `boost/call_traits.hpp`
- 来自`boost/compressed_pair.hpp`的[压缩对](https://www.boost.org/libs/utility/compressed_pair.htm)
- [Compressed Pair](https://www.boost.org/libs/utility/compressed_pair.htm) from `boost/compressed_pair.hpp`
- [Boost Graph Library (BGL)](https://www.boost.org/libs/graph/) 来自 `boost/graph`，序列化 (`adj_list_serialize.hpp`) 和并行/分布式算法和数据结构除外（`boost/graph/parallel/*` 和 `boost/graph/distributed/*`）。
- [The Boost Graph Library (BGL)](https://www.boost.org/libs/graph/) from `boost/graph`, except serialization (`adj_list_serialize.hpp`) and parallel/distributed algorithms and data structures (`boost/graph/parallel/*` and `boost/graph/distributed/*`).
- 来自 `boost/property_map` 的 [属性映射](https://www.boost.org/libs/property_map/)，并行/分布式属性映射 (`boost/property_map/parallel/*`) 除外。
- [Property Map](https://www.boost.org/libs/property_map/) from `boost/property_map`, except parallel/distributed property maps (`boost/property_map/parallel/*`).
- 来自`boost/iterator`的[迭代器](https://www.boost.org/libs/iterator/)
- [Iterator](https://www.boost.org/libs/iterator/) from `boost/iterator`
- [Polygon](https://www.boost.org/libs/polygon/) 中处理 Voronoi 图构造的部分，不依赖于 Polygon 的其余部分：`boost/polygon/voronoi_builder.hpp`， `boost/polygon/voronoi_diagram.hpp` 和 `boost/polygon/voronoi_geometry_type.hpp`
- The part of [Polygon](https://www.boost.org/libs/polygon/) that deals with Voronoi diagram construction and doesn't depend on the rest of Polygon: `boost/polygon/voronoi_builder.hpp`, `boost/polygon/voronoi_diagram.hpp`, and `boost/polygon/voronoi_geometry_type.hpp`
- [Bimap](https://www.boost.org/libs/bimap/) 来自 `boost/bimap`
- [Bimap](https://www.boost.org/libs/bimap/) from `boost/bimap`
- [统计分布和函数](https://www.boost.org/libs/math/doc/html/dist.html) 来自`boost/math/distributions`
- [Statistical Distributions and Functions](https://www.boost.org/libs/math/doc/html/dist.html) from `boost/math/distributions`
- [特殊函数](https://www.boost.org/libs/math/doc/html/special.html) 来自 `boost/math/special_functions`
- [Special Functions](https://www.boost.org/libs/math/doc/html/special.html) from `boost/math/special_functions`
- [求根和最小化函数](https://www.boost.org/libs/math/doc/html/root_finding.html) 来自 `boost/math/tools`
- [Root Finding & Minimization Functions](https://www.boost.org/libs/math/doc/html/root_finding.html) from `boost/math/tools`
- [多索引](https://www.boost.org/libs/multi_index/) 来自 `boost/multi_index`
- [Multi-index](https://www.boost.org/libs/multi_index/) from `boost/multi_index`
- 来自`boost/heap`的[堆](https://www.boost.org/libs/heap/)
- [Heap](https://www.boost.org/libs/heap/) from `boost/heap`
- 来自 [Container](https://www.boost.org/libs/container/) 的扁平容器： `boost/container/flat_map` 和 `boost/container/flat_set`
- The flat containers from [Container](https://www.boost.org/libs/container/): `boost/container/flat_map`, and `boost/container/flat_set`
- 来自`boost/intrusive`的[侵入](https://www.boost.org/libs/intrusive/)。
- [Intrusive](https://www.boost.org/libs/intrusive/) from `boost/intrusive`.
- [`boost/sort` 库](https://www.boost.org/libs/sort/)。
- [The `boost/sort` library](https://www.boost.org/libs/sort/).
- 来自`boost/preprocessor`的[预处理器](https://www.boost.org/libs/preprocessor/)。
- [Preprocessor](https://www.boost.org/libs/preprocessor/) from `boost/preprocessor`.

我们正在积极考虑将其他 Boost 功能添加到列表中，因此将来可能会扩展该列表。
We are actively considering adding other Boost features to the list, so this list may be expanded in the future.

## 其他 C++ 功能

## Other C++ Features

与 _Boost_ 一样，一些现代 C++ 扩展鼓励妨碍可读性的编码实践，例如通过删除可能对读者有帮助的检查冗余（例如类型名称），或鼓励模板元编程。其他扩展重复了现有机制中可用的功能，这可能会导致混乱和转换成本。
As with _Boost_, some modern C++ extensions encourage coding practices that hamper readability—for example by removing checked redundancy (such as type names) that may be helpful to readers, or by encouraging template metaprogramming. Other extensions duplicate functionality available through existing mechanisms, which may lead to confusion and conversion costs.

除了样式指南其余部分中描述的内容之外，不得使用以下 C++ 功能：
In addition to what's described in the rest of the style guide, the following C++ features may not be used:

- 编译时有理数（`<ratio>`），因为担心它与更依赖模板的界面风格相关。
- Compile-time rational numbers (`<ratio>`), because of concerns that it's tied to a more template-heavy interface style.
- `<cfenv>` 和 `<fenv.h>` 标头，因为许多编译器不可靠地支持这些功能。
- The `<cfenv>` and `<fenv.h>` headers, because many compilers do not support those features reliably.
- `<filesystem>` 标头没有足够的测试支持，并且存在固有的安全漏洞。
- The `<filesystem>` header, which does not have sufficient support for testing, and suffers from inherent security vulnerabilities.

## 非标准扩展

## Nonstandard Extensions

除非另有说明，否则不得使用 C++ 的非标准扩展。
Nonstandard extensions to C++ may not be used unless otherwise specified.

编译器支持不属于标准 C++ 的各种扩展。此类扩展包括 GCC 的 `__attribute__` 、内部函数（例如 `__builtin_prefetch` 或 SIMD、 `#pragma` 、内联汇编、 `__COUNTER__` 、 `__PRETTY_FUNCTION__` ）、复合语句表达式（例如， `foo = ({ int x; Bar( &x); x })`、可变长度数组和 `alloca()`，以及 "[Elvis Operator](https://en.wikipedia.org/wiki/Elvis_operator)" `a?:b`。
Compilers support various extensions that are not part of standard C++. Such extensions include GCC's `__attribute__`, intrinsic functions such as `__builtin_prefetch` or SIMD, `#pragma`, inline assembly, `__COUNTER__`, `__PRETTY_FUNCTION__`, compound statement expressions (e.g., `foo = ({ int x; Bar(&x); x })`, variable-length arrays and `alloca()`, and the "[Elvis Operator](https://en.wikipedia.org/wiki/Elvis_operator)" `a?:b`.

- 非标准扩展可能提供标准 C++ 中不存在的有用功能。
- Nonstandard extensions may provide useful features that do not exist in standard C++.
- 对编译器的重要性能指导只能使用扩展来指定。
- Important performance guidance to the compiler can only be specified using extensions.

- 非标准扩展不适用于所有编译器。使用非标准扩展会降低代码的可移植性。
- Nonstandard extensions do not work in all compilers. Use of nonstandard extensions reduces portability of code.
- 即使所有目标编译器都支持它们，但扩展通常没有明确指定，并且编译器之间可能存在细微的行为差异。
- Even if they are supported in all targeted compilers, the extensions are often not well-specified, and there may be subtle behavior differences between compilers.
- 非标准扩展添加到读者必须了解的语言功能才能理解代码。
- Nonstandard extensions add to the language features that a reader must know to understand the code.
- 非标准扩展需要额外的工作才能跨架构移植。
- Nonstandard extensions require additional work to port across architectures.

不要使用非标准扩展。您可以使用通过非标准扩展实现的可移植性包装器，只要这些包装器是由指定的项目范围可移植性标头提供的。
Do not use nonstandard extensions. You may use portability wrappers that are implemented using nonstandard extensions, so long as those wrappers are provided by a designated project-wide portability header.

## 别名

## Aliases

公共别名是为了 API 用户的利益，应明确记录。
Public aliases are for the benefit of an API's user, and should be clearly documented.

有多种方法可以创建作为其他实体的别名的名称：
There are several ways to create names that are aliases of other entities:

```C++
typedef Foo Bar;
using Bar = Foo;
using other_namespace::Foo;
```

在新代码中， `using` 优于 `typedef` ，因为它提供了与 C++ 其余部分更一致的语法，并且可以与模板一起使用。
In new code, `using` is preferable to `typedef`, because it provides a more consistent syntax with the rest of C++ and works with templates.

与其他声明一样，头文件中声明的别名是该头文件的公共 API 的一部分，除非它们位于函数定义中、类的私有部分中或显式标记的内部命名空间中。这些区域或 `.cc` 文件中的别名是实现细节（因为客户端代码无法引用它们），并且不受此规则的限制。
Like other declarations, aliases declared in a header file are part of that header's public API unless they're in a function definition, in the private portion of a class, or in an explicitly-marked internal namespace. Aliases in such areas or in `.cc` files are implementation details (because client code can't refer to them), and are not restricted by this rule.

- 别名可以通过简化长或复杂的名称来提高可读性。
- Aliases can improve readability by simplifying a long or complicated name.
- 别名可以通过在一个地方命名 API 中重复使用的类型来减少重复，这可能会让以后更容易更改类型。
- Aliases can reduce duplication by naming in one place a type used repeatedly in an API, which _might_ make it easier to change the type later.

- 当放置在客户端代码可以引用它们的标头中时，别名会增加该标头 API 中的实体数量，从而增加其复杂性。
- When placed in a header where client code can refer to them, aliases increase the number of entities in that header's API, increasing its complexity.
- 客户可以轻松依赖公共别名的意外详细信息，从而使更改变得困难。
- Clients can easily rely on unintended details of public aliases, making changes difficult.
- 创建一个仅用于实现的公共别名可能很诱人，而不考虑它对 API 或可维护性的影响。
- It can be tempting to create a public alias that is only intended for use in the implementation, without considering its impact on the API, or on maintainability.
- 别名可能会造成名称冲突的风险
- Aliases can create risk of name collisions
- 别名会给熟悉的结构赋予不熟悉的名称，从而降低可读性
- Aliases can reduce readability by giving a familiar construct an unfamiliar name
- 类型别名可能会产生不明确的 API 契约：不清楚是否保证别名与其别名的类型相同、是否具有相同的 API，或者是否只能以指定的狭窄方式使用
- Type aliases can create an unclear API contract: it is unclear whether the alias is guaranteed to be identical to the type it aliases, to have the same API, or only to be usable in specified narrow ways

不要仅仅为了节省实现中的输入而在公共 API 中添加别名；仅当您打算供客户使用时才这样做。
Don't put an alias in your public API just to save typing in the implementation; do so only if you intend it to be used by your clients.

定义公共别名时，记录新名称的意图，包括是否保证它始终与其当前别名的类型相同，或者是否打算更有限的兼容性。这让用户知道他们是否可以将类型视为可替换，或者是否必须遵循更具体的规则，并且可以帮助实现保留一定程度的自由度来更改别名。
When defining a public alias, document the intent of the new name, including whether it is guaranteed to always be the same as the type it's currently aliased to, or whether a more limited compatibility is intended. This lets the user know whether they can treat the types as substitutable or whether more specific rules must be followed, and can help the implementation retain some degree of freedom to change the alias.

不要将命名空间别名放入公共 API 中。 （另请参阅*命名空间*）。
Don't put namespace aliases in your public API. (See also _Namespaces_).

例如，这些别名记录了它们在客户端代码中的使用方式：
For example, these aliases document how they are intended to be used in client code:

```C++
namespace mynamespace {
// Used to store field measurements. DataPoint may change from Bar* to some internal type.
// Client code should treat it as an opaque pointer.
using DataPoint = ::foo::Bar*;

// A set of measurements. Just an alias for user convenience.
using TimeSeries = std::unordered_set<DataPoint, std::hash<DataPoint>, DataPointComparator>;
}  // namespace mynamespace
```

这些别名不记录预期用途，其中一半不适合客户端使用：
These aliases don't document intended use, and half of them aren't meant for client use:

**badcode**

```C++
namespace mynamespace {
// Bad: none of these say how they should be used.
using DataPoint = ::foo::Bar*;
using ::std::unordered_set;  // Bad: just for local convenience
using ::std::hash;           // Bad: just for local convenience
typedef unordered_set<DataPoint, hash<DataPoint>, DataPointComparator> TimeSeries;
}  // namespace mynamespace
```

然而，本地便利别名在函数定义、类的 `private` 部分、显式标记的内部命名空间以及 `.cc` 文件中很好用：
However, local convenience aliases are fine in function definitions, `private` sections of classes, explicitly marked internal namespaces, and in `.cc` files:

```C++
// In a .cc file
using ::foo::Bar;
```

## Switch 语句

## Switch Statements

如果不以枚举值为条件，则 switch 语句应始终具有 `default` 情况（在枚举值的情况下，如果未处理任何值，编译器将警告您）。如果默认情况永远不应该执行，请将其视为错误。例如：
If not conditional on an enumerated value, switch statements should always have a `default` case (in the case of an enumerated value, the compiler will warn you if any values are not handled). If the default case should never execute, treat this as an error. For example:

```C++
switch (var) {
  case 0: {
    ...
    break;
  }
  case 1: {
    ...
    break;
  }
  default: {
    LOG(FATAL) << "Invalid value in switch statement: " << var;
  }
}
```

从一个案例标签到另一个案例标签的跌落必须使用 `[[fallthrough]];` 属性进行注释。 `[[fallthrough]];` 应放置在发生跳转到下一个 case 标签的执行点。一个常见的例外是连续的 case 标签没有中间代码，在这种情况下不需要注释。
Fall-through from one case label to another must be annotated using the `[[fallthrough]];` attribute. `[[fallthrough]];` should be placed at a point of execution where a fall-through to the next case label occurs. A common exception is consecutive case labels without intervening code, in which case no annotation is needed.

```C++
switch (x) {
  case 41:  // No annotation needed here.
  case 43:
    if (dont_be_picky) {
      // Use this instead of or along with annotations in comments.
      [[fallthrough]];
    } else {
      CloseButNoCigar();
      break;
    }
  case 42:
    DoSomethingSpecial();
    [[fallthrough]];
  default:
    DoSomethingGeneric();
    break;
}
```

# 包容性语言

# Inclusive Language

在所有代码中，包括命名和注释，请使用包容性语言并避免其他程序员可能认为不尊重或冒犯的术语（例如 "主" 和 "从" 、 "黑名单" 和 "白名单" 或 "红线" ），即使这些术语表面上也具有中性含义。同样，请使用中性语言，除非您指的是特定的人（并使用他们的代词）。例如，对未指定性别的人使用 "they" / "them" / "their" （[即使是单数](https://apastyle.apa.org/style-grammar-guidelines/grammar/singular-they)） ， "it" / "its" 用于软件、计算机和其他非人的事物。
In all code, including naming and comments, use inclusive language and avoid terms that other programmers might find disrespectful or offensive (such as "master" and "slave", "blacklist" and "whitelist", or "redline"), even if the terms also have an ostensibly neutral meaning. Similarly, use gender-neutral language unless you're referring to a specific person (and using their pronouns). For example, use "they"/"them"/"their" for people of unspecified gender ([even when singular](https://apastyle.apa.org/style-grammar-guidelines/grammar/singular-they)), and "it"/"its" for software, computers, and other things that aren't people.

# 命名

# Naming

最重要的一致性规则是管理命名的规则。名称的样式立即告诉我们命名实体是什么类型：类型、变量、函数、常量、宏等，而不需要我们搜索该实体的声明。我们大脑中的模式匹配引擎很大程度上依赖于这些命名规则。
The most important consistency rules are those that govern naming. The style of a name immediately informs us what sort of thing the named entity is: a type, a variable, a function, a constant, a macro, etc., without requiring us to search for the declaration of that entity. The pattern-matching engine in our brains relies a great deal on these naming rules.

命名规则相当随意，但我们认为在这方面一致性比个人偏好更重要，所以无论你认为它们是否合理，规则就是规则。
Naming rules are pretty arbitrary, but we feel that consistency is more important than individual preferences in this area, so regardless of whether you find them sensible or not, the rules are the rules.

## 一般命名规则

## General Naming Rules

使用即使对于不同团队的人员来说也清晰的名称来优化可读性。
Optimize for readability using names that would be clear even to people on a different team.

使用描述对象的目的或意图的名称。不要担心节省水平空间，因为让新读者立即理解您的代码更为重要。尽量减少项目外人员可能不知道的缩写词的使用（尤其是首字母缩略词和首字母缩写词）。不要通过删除单词中的字母来缩写。根据经验，如果维基百科中列出了缩写，则可能没问题。一般来说，描述性应该与名称的可见范围成正比。例如， `n` 在 5 行函数中可能是一个很好的名称，但在类的范围内，它可能太模糊了。
Use names that describe the purpose or intent of the object. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Minimize the use of abbreviations that would likely be unknown to someone outside your project (especially acronyms and initialisms). Do not abbreviate by deleting letters within a word. As a rule of thumb, an abbreviation is probably OK if it's listed in Wikipedia. Generally speaking, descriptiveness should be proportional to the name's scope of visibility. For example, `n` may be a fine name within a 5-line function, but within the scope of a class, it's likely too vague.

```C++
class MyClass {
 public:
  int CountFooErrors(const std::vector<Foo>& foos) {
    int n = 0;  // Clear meaning given limited scope and context
    for (const auto& foo : foos) {
      ...
      ++n;
    }
    return n;
  }
  void DoSomethingImportant() {
    std::string fqdn = ...;  // Well-known abbreviation for Fully Qualified Domain Name
  }
 private:
  const int kMaxAllowedConnections = ...;  // Clear meaning within context
};
```

**badcode**

```C++
class MyClass {
 public:
  int CountFooErrors(const std::vector<Foo>& foos) {
    int total_number_of_foo_errors = 0;  // Overly verbose given limited scope and context
    for (int foo_index = 0; foo_index < foos.size(); ++foo_index) {  // Use idiomatic `i`
      ...
      ++total_number_of_foo_errors;
    }
    return total_number_of_foo_errors;
  }
  void DoSomethingImportant() {
    int cstmr_id = ...;  // Deletes internal letters
  }
 private:
  const int kNum = ...;  // Unclear meaning within broad scope
};
```

请注意，某些众所周知的缩写是可以的，例如迭代变量的 `i` 和模板参数的 `T` 。
Note that certain universally-known abbreviations are OK, such as `i` for an iteration variable and `T` for a template parameter.

出于以下命名规则的目的， "单词" 是您用英语编写的没有内部空格的任何内容。这包括缩写词，例如首字母缩略词和缩写词。对于以混合大小写书写的名称（有时也称为 "[camel case](https://en.wikipedia.org/wiki/Camel_case)" 或 "[Pascal case](https://en.wiktionary.org/wiki/Pascal_case)")，其中每个单词的第一个字母都大写，更喜欢将缩写作为单个单词大写，例如 `StartRpc()` 而不是 `StartRPC()` 。
For the purposes of the naming rules below, a "word" is anything that you would write in English without internal spaces. This includes abbreviations, such as acronyms and initialisms. For names written in mixed case (also sometimes referred to as "[camel case](https://en.wikipedia.org/wiki/Camel_case)" or "[Pascal case](https://en.wiktionary.org/wiki/Pascal_case)"), in which the first letter of each word is capitalized, prefer to capitalize abbreviations as single words, e.g., `StartRpc()` rather than `StartRPC()`.

模板参数应遵循其类别的命名风格：类型模板参数应遵循*类型名称*规则，非类型模板参数应遵循*变量名称*规则。
Template parameters should follow the naming style for their category: type template parameters should follow the rules for _type names_, and non-type template parameters should follow the rules for _variable names_.

## 文件名

## File Names

文件名应全部小写，并且可以包含下划线 (`_`) 或破折号 (`-`)。遵循您的项目使用的约定。如果没有一致的本地模式可遵循，则首选 "`_`" 。
Filenames should be all lowercase and can include underscores (`_`) or dashes (`-`). Follow the convention that your project uses. If there is no consistent local pattern to follow, prefer "`_`".

可接受的文件名示例：
Examples of acceptable file names:

- `my_useful_class.cc`
- `my-useful-class.cc`
- `myusefulclass.cc`
- `myusefulclass_test.cc // _unittest and _regtest are deprecated.`

C++ 文件应以 `.cc` 结尾，头文件应以 `.h` 结尾。依赖于在特定点以文本方式包含的文件应以 `.inc` 结尾（另请参阅有关[自包含标头](#Self_contained_Headers)的部分）。
C++ files should end in `.cc` and header files should end in `.h`. Files that rely on being textually included at specific points should end in `.inc` (see also the section on [self-contained headers](#Self_contained_Headers)).

不要使用`/usr/include`中已经存在的文件名，例如`db.h`。
Do not use filenames that already exist in `/usr/include`, such as `db.h`.

一般来说，让你的文件名非常具体。例如，使用 `http_server_logs.h` 而不是 `logs.h` 。一个非常常见的情况是有一对名为 `foo_bar.h` 和 `foo_bar.cc` 的文件，定义一个名为 `FooBar` 的类。
In general, make your filenames very specific. For example, use `http_server_logs.h` rather than `logs.h`. A very common case is to have a pair of files called, e.g., `foo_bar.h` and `foo_bar.cc`, defining a class called `FooBar`.

## 类型名称

## Type Names

类型名称以大写字母开头，每个新单词都大写，不带下划线：`MyExcitingClass`、`MyExcitingEnum`。
Type names start with a capital letter and have a capital letter for each new word, with no underscores: `MyExcitingClass`, `MyExcitingEnum`.

所有类型的名称（类、结构、类型别名、枚举和类型模板参数）都具有相同的命名约定。类型名称应以大写字母开头，并且每个新单词都有大写字母。没有下划线。例如：
The names of all types — classes, structs, type aliases, enums, and type template parameters — have the same naming convention. Type names should start with a capital letter and have a capital letter for each new word. No underscores. For example:

```C++
// classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// typedefs
typedef hash_map<UrlTableProperties *, std::string> PropertiesMap;

// using aliases
using PropertiesMap = hash_map<UrlTableProperties *, std::string>;

// enums
enum class UrlTableError { ...
```

## 变量名称

## Variable Names

变量（包括函数参数）和数据成员的名称为 `snake_case` （全部小写，单词之间有下划线）。类（但不是结构）的数据成员另外还有尾随下划线。例如：`a_local_variable`、`a_struct_data_member`、`a_class_data_member_`。
The names of variables (including function parameters) and data members are `snake_case` (all lowercase, with underscores between words). Data members of classes (but not structs) additionally have trailing underscores. For instance: `a_local_variable`, `a_struct_data_member`, `a_class_data_member_`.

### 常用变量名

### Common Variable names

例如：
For example:

```C++
std::string table_name;  // OK - snake_case.
```

**badcode**

```C++
std::string tableName;   // Bad - mixed case.
```

### 类数据成员

### Class Data Members

类的数据成员（静态和非静态）的命名方式与普通非成员变量类似，但末尾带有下划线。
Data members of classes, both static and non-static, are named like ordinary nonmember variables, but with a trailing underscore.

```C++
class TableInfo {
  ...
 private:
  std::string table_name_;  // OK - underscore at end.
  static Pool<TableInfo>* pool_;  // OK.
};
```

### 结构数据成员

### Struct Data Members

结构的数据成员（静态和非静态）的命名方式与普通非成员变量类似。它们没有类中数据成员所具有的尾部下划线。
Data members of structs, both static and non-static, are named like ordinary nonmember variables. They do not have the trailing underscores that data members in classes have.

```C++
struct UrlTableProperties {
  std::string name;
  int num_entries;
  static Pool<UrlTableProperties>* pool;
};
```

请参阅[结构与类](#Structs_vs._Classes) 了解何时使用结构与类的讨论。
See [Structs vs. Classes](#Structs_vs._Classes) for a discussion of when to use a struct versus a class.

## 常量名称

## Constant Names

声明为 `constexpr` 或 `const` 的变量，其值在程序运行期间是固定的，以前导 `k` 命名，后跟混合大小写。在极少数无法使用大写字母进行分隔的情况下，可以使用下划线作为分隔符。例如：
Variables declared `constexpr` or `const`, and whose value is fixed for the duration of the program, are named with a leading "k" followed by mixed case. Underscores can be used as separators in the rare cases where capitalization cannot be used for separation. For example:

```C++
const int kDaysInAWeek = 7;
const int kAndroid8_0_0 = 24;  // Android 8.0.0
```

所有具有静态存储持续时间的变量（即静态变量和全局变量，有关详细信息，请参阅[存储持续时间](http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration)）都应该以这种方式命名。对于其他存储类的变量，例如自动变量，此约定是可选的；否则，应用通常的变量命名规则。例如：
All such variables with static storage duration (i.e., statics and globals, see [Storage Duration](http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration) for details) should be named this way. This convention is optional for variables of other storage classes, e.g., automatic variables; otherwise the usual variable naming rules apply. For example:

```C++
void ComputeFoo(absl::string_view suffix) {
  // Either of these is acceptable.
  const absl::string_view kPrefix = "prefix";
  const absl::string_view prefix = "prefix";
  ...
}
```

**badcode**

```C++
void ComputeFoo(absl::string_view suffix) {
  // Bad - different invocations of ComputeFoo give kCombined different values.
  const std::string kCombined = absl::StrCat(kPrefix, suffix);
  ...
}
```

## 函数名称

## Function Names

常规函数具有混合大小写；访问器和修改器可以像变量一样命名。
Regular functions have mixed case; accessors and mutators may be named like variables.

通常，函数应以大写字母开头，并且每个新单词都有一个大写字母。
Ordinarily, functions should start with a capital letter and have a capital letter for each new word.

```C++
AddTableEntry()
DeleteUrl()
OpenFileOrDie()
```

（相同的命名规则适用于作为 API 的一部分公开的类和命名空间范围常量，这些常量旨在看起来像函数，因为它们是对象而不是函数这一事实是一个不重要的实现细节。）
(The same naming rule applies to class- and namespace-scope constants that are exposed as part of an API and that are intended to look like functions, because the fact that they're objects rather than functions is an unimportant implementation detail.)

访问器和修改器（获取和设置函数）可以像变量一样命名。这些通常对应于实际的成员变量，但这不是必需的。例如， `int count()` 和 `void set_count(int count)` 。
Accessors and mutators (get and set functions) may be named like variables. These often correspond to actual member variables, but this is not required. For example, `int count()` and `void set_count(int count)`.

## 命名空间名称

## Namespace Names

命名空间名称全部小写，单词之间用下划线分隔。顶级命名空间名称基于项目名称。避免嵌套命名空间和众所周知的顶级命名空间之间的冲突。
Namespace names are all lower-case, with words separated by underscores. Top-level namespace names are based on the project name . Avoid collisions between nested namespaces and well-known top-level namespaces.

顶级命名空间的名称通常应该是其代码包含在该命名空间中的项目或团队的名称。该命名空间中的代码通常应位于基本名称与命名空间名称匹配的目录中（或其子目录中）。
The name of a top-level namespace should usually be the name of the project or team whose code is contained in that namespace. The code in that namespace should usually be in a directory whose basename matches the namespace name (or in subdirectories thereof).

请记住，*针对缩写名称的规则*与变量名称一样适用于名称空间。命名空间内的代码很少需要提及命名空间名称，因此通常不需要特别缩写。
Keep in mind that the _rule against abbreviated names_ applies to namespaces just as much as variable names. Code inside the namespace seldom needs to mention the namespace name, so there's usually no particular need for abbreviation anyway.

避免与众所周知的顶级命名空间匹配的嵌套命名空间。由于名称查找规则，名称空间名称之间的冲突可能会导致意外的构建中断。特别是，不要创建任何嵌套的 `std` 命名空间。优先选择唯一的项目标识符（`websearch::index`、`websearch::index_util`）而不是容易发生冲突的名称，例如`websearch::util`。还要避免嵌套过深的命名空间（[TotW #130](https://abseil.io/tips/130)）。
Avoid nested namespaces that match well-known top-level namespaces. Collisions between namespace names can lead to surprising build breaks because of name lookup rules. In particular, do not create any nested `std` namespaces. Prefer unique project identifiers (`websearch::index`, `websearch::index_util`) over collision-prone names like `websearch::util`. Also avoid overly deep nesting namespaces ([TotW #130](https://abseil.io/tips/130)).

对于 `内部` 命名空间，请注意其他代码添加到同一 `内部` 命名空间会导致冲突（团队内的内部助手往往是相关的，并可能导致冲突）。在这种情况下，使用文件名创建唯一的内部名称会很有帮助（ `websearch::index::frobber_internal` 用于 `frobber.h` ）。
For `internal` namespaces, be wary of other code being added to the same `internal` namespace causing a collision (internal helpers within a team tend to be related and may lead to collisions). In such a situation, using the filename to make a unique internal name is helpful (`websearch::index::frobber_internal` for use in `frobber.h`).

## 枚举器名称

## Enumerator Names

枚举数（对于有作用域和无作用域枚举）应该命名为*constants*，而不是*macros*。也就是说，使用 `kEnumName` 而不是 `ENUM_NAME` 。
Enumerators (for both scoped and unscoped enums) should be named like _constants_, not like _macros_. That is, use `kEnumName` not `ENUM_NAME`.

```C++
enum class UrlTableError {
  kOk = 0,
  kOutOfMemory,
  kMalformedInput,
};
```

**badcode**

```C++
enum class AlternateUrlTableError {
  OK = 0,
  OUT_OF_MEMORY = 1,
  MALFORMED_INPUT = 2,
};
```

2009 年 1 月之前，枚举值的命名风格仍为 _macros_。这导致了枚举值和宏之间的名称冲突问题。因此，倾向于使用常量样式命名的更改已经到位。新代码应使用常量样式命名。
Until January 2009, the style was to name enum values like _macros_. This caused problems with name collisions between enum values and macros. Hence, the change to prefer constant-style naming was put in place. New code should use constant-style naming.

## 宏名称

## Macro Names

您不会真的要*定义宏*，是吗？如果你这样做，它们就像这样： `MY_MACRO_THAT_SCARES_SMALL_CHILDREN_AND_ADULTS_ALIKE` 。
You're not really going to _define a macro_, are you? If you do, they're like this: `MY_MACRO_THAT_SCARES_SMALL_CHILDREN_AND_ADULTS_ALIKE`.

请参阅*宏说明*；一般来说，不应使用宏。但是，如果绝对需要它们，则应使用全部大写和下划线以及特定于项目的前缀来命名它们。
Please see the _description of macros_; in general macros should _not_ be used. However, if they are absolutely needed, then they should be named with all capitals and underscores, and with a project-specific prefix.

```C++
#define MYPROJECT_ROUND(x) ...
```

## 命名规则的例外情况

## Exceptions to Naming Rules

如果您要命名的内容类似于现有的 C 或 C++ 实体，那么您可以遵循现有的命名约定方案。
If you are naming something that is analogous to an existing C or C++ entity then you can follow the existing naming convention scheme.

- `bigopen()`

  函数名称，遵循`open()`的形式
  function name, follows form of `open()`

- `uint`

  `typedef`

- `bigpos`

  `struct` 或 `class`，遵循 `pos` 的形式
  `struct` or `class`, follows form of `pos`

- `sparse_hash_map`

  类似 STL 的实体；遵循 STL 命名约定
  STL-like entity; follows STL naming conventions

- `LONGLONG_MAX`

  一个常量，如 `INT_MAX`
  a constant, as in `INT_MAX`

# 注释

# Comments

注释对于保持我们的代码可读性绝对至关重要。以下规则描述了您应该注释什么以及在哪里注释。但请记住：虽然注释非常重要，但最好的代码是自我记录的。为类型和变量提供合理的名称比使用晦涩的名称要好得多，然后您必须通过注释进行解释。
Comments are absolutely vital to keeping our code readable. The following rules describe what you should comment and where. But remember: while comments are very important, the best code is self-documenting. Giving sensible names to types and variables is much better than using obscure names that you must then explain through comments.

在撰写注释时，请为您的受众撰写：下一个需要理解您的代码的贡献者。慷慨一点——下一个可能就是你！
When writing your comments, write for your audience: the next contributor who will need to understand your code. Be generous — the next one may be you!

## 注释风格

## Comment Style

使用 `//` 或 `/* */` 语法，只要保持一致即可。
Use either the `//` or `/* */` syntax, as long as you are consistent.

您可以使用 `//` 或 `/* */` 语法；然而， `//` 更为常见。与你的注释方式和你在哪里使用的风格保持一致。
You can use either the `//` or the `/* */` syntax; however, `//` is _much_ more common. Be consistent with how you comment and what style you use where.

## 文件注释

## File Comments

使用许可证样板启动每个文件。
Start each file with license boilerplate.

如果源文件（例如 `.h` 文件）声明了多个面向用户的抽象（通用函数、相关类等），请包含描述这些抽象集合的注释。包含足够的细节，以便未来的作者知道哪些不适合那里。但是，有关各个抽象的详细文档属于这些抽象，而不是文件级别。
If a source file (such as a `.h` file) declares multiple user-facing abstractions (common functions, related classes, etc.), include a comment describing the collection of those abstractions. Include enough detail for future authors to know what does not fit there. However, the detailed documentation about individual abstractions belongs with those abstractions, not at the file level.

例如，如果您为 `frobber.h` 编写文件注释，则不需要在 `frobber.cc` 或 `frobber_test.cc` 中包含文件注释。另一方面，如果您在 `registered_objects.cc` 中编写没有关联头文件的类集合，则必须在 `registered_objects.cc` 中包含文件注释。
For instance, if you write a file comment for `frobber.h`, you do not need to include a file comment in `frobber.cc` or `frobber_test.cc`. On the other hand, if you write a collection of classes in `registered_objects.cc` that has no associated header file, you must include a file comment in `registered_objects.cc`.

### 法律声明和作者行

### Legal Notice and Author Line

每个文件都应包含许可证样板。为项目使用的许可证选择适当的样板（例如，Apache 2.0、BSD、LGPL、GPL）。
Every file should contain license boilerplate. Choose the appropriate boilerplate for the license used by the project (for example, Apache 2.0, BSD, LGPL, GPL).

如果您对包含作者行的文件进行重大更改，请考虑删除作者行。新文件通常不应包含版权声明或作者行。
If you make significant changes to a file with an author line, consider deleting the author line. New files should usually not contain copyright notice or author line.

## 类注释

## Class Comments

每个不明显的类或结构声明都应该有一个附带的注释，描述它的用途以及如何使用它。
Every non-obvious class or struct declaration should have an accompanying comment that describes what it is for and how it should be used.

```C++
// Iterates over the contents of a GargantuanTable.
// Example:
//    std::unique_ptr<GargantuanTableIterator> iter = table->NewIterator();
//    for (iter->Seek("foo"); !iter->done(); iter->Next()) {
//      process(iter->key(), iter->value());
//    }
class GargantuanTableIterator {
  ...
};
```

类注释应该为读者提供足够的信息，以了解如何以及何时使用该类，以及正确使用该类所需的任何其他注意事项。记录该类所做的同步假设（如果有）。如果类的实例可以由多个线程访问，请特别注意记录有关多线程使用的规则和不变量。
The class comment should provide the reader with enough information to know how and when to use the class, as well as any additional considerations necessary to correctly use the class. Document the synchronization assumptions the class makes, if any. If an instance of the class can be accessed by multiple threads, take extra care to document the rules and invariants surrounding multithreaded use.

类注释通常是放置小示例代码片段的好地方，演示该类的简单且集中的用法。
The class comment is often a good place for a small example code snippet demonstrating a simple and focused usage of the class.

当充分分离时（例如， `.h` 和 `.cc` 文件），描述类使用的注释应与其接口定义一起使用；关于类操作和实现的注释应该伴随类方法的实现。
When sufficiently separated (e.g., `.h` and `.cc` files), comments describing the use of the class should go together with its interface definition; comments about the class operation and implementation should accompany the implementation of the class's methods.

## 函数注释

## Function Comments

声明注释描述了函数的使用（当它不明显时）；函数定义处的注释描述操作。
Declaration comments describe use of the function (when it is non-obvious); comments at the definition of a function describe operation.

### 函数声明

### Function Declarations

几乎每个函数声明前面都应该有注释，描述该函数的作用以及如何使用它。仅当函数简单且明显时（例如，用于类的明显属性的简单访问器），才可以省略这些注释。在 `.cc` 文件中声明的私有方法和函数也不例外。函数注释应以 "此函数" 作为隐含主语，并以动词短语开头；例如， "Opens the file" ，而不是 "Open the file" 。一般来说，这些注释不描述函数如何执行其任务。相反，应该将其留给函数定义中的注释。
Almost every function declaration should have comments immediately preceding it that describe what the function does and how to use it. These comments may be omitted only if the function is simple and obvious (e.g., simple accessors for obvious properties of the class). Private methods and functions declared in `.cc` files are not exempt. Function comments should be written with an implied subject of _This function_ and should start with the verb phrase; for example, "Opens the file", rather than "Open the file". In general, these comments do not describe how the function performs its task. Instead, that should be left to comments in the function definition.

函数声明注释中要提及的内容类型：
Types of things to mention in comments at the function declaration:

- 输入和输出是什么。如果函数参数名称在 `反引号` 中提供，那么代码索引工具可能能够更好地呈现文档。
- What the inputs and outputs are. If function argument names are provided in `backticks`, then code-indexing tools may be able to present the documentation better.
- 对于类成员函数：对象是否在方法调用持续时间之外记住引用或指针参数。这对于构造函数的指针/引用参数来说很常见。
- For class member functions: whether the object remembers reference or pointer arguments beyond the duration of the method call. This is quite common for pointer/reference arguments to constructors.
- 对于每个指针参数，是否允许为空以及如果为空会发生什么。
- For each pointer argument, whether it is allowed to be null and what happens if it is.
- 对于每个输出或输入/输出参数，该参数所处的任何状态会发生什么情况。（例如，状态是附加的还是覆盖的？）。
- For each output or input/output argument, what happens to any state that argument is in. (E.g. is the state appended to or overwritten?).
- 函数的使用方式是否对性能有任何影响。
- If there are any performance implications of how a function is used.

这是一个例子：
Here is an example:

```C++
// Returns an iterator for this table, positioned at the first entry
// lexically greater than or equal to `start_word`. If there is no
// such entry, returns a null pointer. The client must not use the
// iterator after the underlying GargantuanTable has been destroyed.
//
// This method is equivalent to:
//    std::unique_ptr<Iterator> iter = table->NewIterator();
//    iter->Seek(start_word);
//    return iter;
std::unique_ptr<Iterator> GetIterator(absl::string_view start_word) const;
```

但是，不要过于冗长或陈述完全显而易见的内容。
However, do not be unnecessarily verbose or state the completely obvious.

在记录函数重写时，请重点关注重写本身的细节，而不是重复被重写函数的注释。在许多这样的情况下，覆盖不需要额外的文档，因此不需要任何注释。
When documenting function overrides, focus on the specifics of the override itself, rather than repeating the comment from the overridden function. In many of these cases, the override needs no additional documentation and thus no comment is required.

在注释构造函数和析构函数时，请记住阅读代码的人知道构造函数和析构函数的用途，因此仅说 "销毁此对象" 之类的注释是没有用的。记录构造函数对其参数执行的操作（例如，如果它们获取指针的所有权），以及析构函数执行的清理操作。如果这是微不足道的，请跳过注释。析构函数没有标题注释是很常见的。
When commenting constructors and destructors, remember that the person reading your code knows what constructors and destructors are for, so comments that just say something like "destroys this object" are not useful. Document what constructors do with their arguments (for example, if they take ownership of pointers), and what cleanup the destructor does. If this is trivial, just skip the comment. It is quite common for destructors not to have a header comment.

### 函数定义

### Function Definitions

如果函数如何完成其工作有任何棘手的地方，函数定义应该有一个解释性注释。例如，在定义注释中，您可以描述您使用的任何编码技巧，概述您所经历的步骤，或者解释为什么您选择以这种方式实现该函数，而不是使用可行的替代方案。例如，您可能会提到为什么它必须为函数的前半部分获取锁，但为什么后半部分不需要它。
If there is anything tricky about how a function does its job, the function definition should have an explanatory comment. For example, in the definition comment you might describe any coding tricks you use, give an overview of the steps you go through, or explain why you chose to implement the function in the way you did rather than using a viable alternative. For instance, you might mention why it must acquire a lock for the first half of the function but why it is not needed for the second half.

请注意，您不应该只是在 `.h` 文件或任何地方重复函数声明中给出的注释。简要概括一下该函数的作用是可以的，但注释的重点应该是它是如何实现的。
Note you should _not_ just repeat the comments given with the function declaration, in the `.h` file or wherever. It's okay to recapitulate briefly what the function does, but the focus of the comments should be on how it does it.

## 变量注释

## Variable Comments

一般来说，变量的实际名称应该具有足够的描述性，以便很好地了解变量的用途。在某些情况下，需要更多注释。
In general the actual name of the variable should be descriptive enough to give a good idea of what the variable is used for. In certain cases, more comments are required.

### 类数据成员

### Class Data Members

每个类数据成员（也称为实例变量或成员变量）的用途必须明确。如果有任何不变量（特殊值、成员之间的关系、生命周期要求）没有通过类型和名称明确表达，则必须对其进行注释。但是，如果类型和名称足够（`int num_events_;`），则不需要注释。
The purpose of each class data member (also called an instance variable or member variable) must be clear. If there are any invariants (special values, relationships between members, lifetime requirements) not clearly expressed by the type and name, they must be commented. However, if the type and name suffice (`int num_events_;`), no comment is needed.

特别是，当哨兵值不明显时，添加注释来描述哨兵值的存在和含义，例如 nullptr 或 -1。例如：
In particular, add comments to describe the existence and meaning of sentinel values, such as nullptr or -1, when they are not obvious. For example:

```C++
private:
 // Used to bounds-check table accesses. -1 means
 // that we don't yet know how many entries the table has.
 int num_total_entries_;
```

### 全局变量

### Global Variables

所有全局变量都应该有一个注释，描述它们是什么、它们的用途以及（如果不清楚）为什么它们需要是全局的。例如：
All global variables should have a comment describing what they are, what they are used for, and (if unclear) why they need to be global. For example:

```C++
// The total number of test cases that we run through in this regression test.
const int kNumTestCases = 6;
```

## 实现注释

## Implementation Comments

在你的实现中，你应该在代码中棘手的、不明显的、有趣的或重要的部分添加注释。
In your implementation you should have comments in tricky, non-obvious, interesting, or important parts of your code.

### 解释性注释

### Explanatory Comments

棘手或复杂的代码块前面应该有注释。
Tricky or complicated code blocks should have comments before them.

### 函数参数注释

### Function Argument Comments

当函数参数的含义不明显时，请考虑以下补救措施之一：
When the meaning of a function argument is nonobvious, consider one of the following remedies:

- 如果参数是字面量常量，并且在多个函数调用中使用相同的常量，默认它们是相同的，则应该使用命名常量来明确该约束，并保证它成立。
- If the argument is a literal constant, and the same constant is used in multiple function calls in a way that tacitly assumes they're the same, you should use a named constant to make that constraint explicit, and to guarantee that it holds.
- 考虑更改函数签名以将 `bool` 参数替换为 `enum` 参数。这将使论证值具有自我描述性。
- Consider changing the function signature to replace a `bool` argument with an `enum` argument. This will make the argument values self-describing.
- 对于具有多个配置选项的函数，请考虑定义一个类或结构来保存所有选项，并传递其实例。这种方法有几个优点。选项在调用站点通过名称引用，这阐明了它们的含义。它还减少了函数参数数量，从而使函数调用更易于读写。作为一个额外的好处，您在添加另一个选项时不必更改调用站点。
- For functions that have several configuration options, consider defining a single class or struct to hold all the options , and pass an instance of that. This approach has several advantages. Options are referenced by name at the call site, which clarifies their meaning. It also reduces function argument count, which makes function calls easier to read and write. As an added benefit, you don't have to change call sites when you add another option.
- 用命名变量替换大型或复杂的嵌套表达式。
- Replace large or complex nested expressions with named variables.
- 作为最后的手段，使用注释来澄清调用站点的参数含义。
- As a last resort, use comments to clarify argument meanings at the call site.

考虑以下示例：
Consider the following example:

**badcode**

```C++
// What are these arguments?
const DecimalNumber product = CalculateProduct(values, 7, false, nullptr);
```

相对：
versus:

```C++
ProductOptions options;
options.set_precision_decimals(7);
options.set_use_cache(ProductOptions::kDontUseCache);
const DecimalNumber product =
    CalculateProduct(values, options, /*completion_callback=*/nullptr);
```

### 不该做的事

### Don'ts

不要陈述显而易见的事情。特别是，不要从字面上描述代码的作用，除非该行为对于很好理解 C++ 的读者来说并不明显。相反，提供更高级别的注释来描述代码为何执行其操作，或者使代码自我描述。
Do not state the obvious. In particular, don't literally describe what code does, unless the behavior is nonobvious to a reader who understands C++ well. Instead, provide higher level comments that describe _why_ the code does what it does, or make the code self describing.

比较一下：
Compare this:

**badcode**

```C++
// Find the element in the vector.  <-- Bad: obvious!
if (std::find(v.begin(), v.end(), element) != v.end()) {
  Process(element);
}
```

对此：
To this:

```C++
// Process "element" unless it was already processed.
if (std::find(v.begin(), v.end(), element) != v.end()) {
  Process(element);
}
```

自描述代码不需要注释。上面例子的注释是显而易见的：
Self-describing code doesn't need a comment. The comment from the example above would be obvious:

```C++
if (!IsAlreadyProcessed(element)) {
  Process(element);
}
```

## 标点符号、拼写和语法

## Punctuation, Spelling, and Grammar

注意标点符号、拼写和语法；阅读写得好的注释比阅读写得不好的注释更容易。
Pay attention to punctuation, spelling, and grammar; it is easier to read well-written comments than badly written ones.

注释应该像叙述性文本一样可读，并使用正确的大写和标点符号。在许多情况下，完整的句子比句子片段更具可读性。较短的注释，例如一行代码末尾的注释，有时可能不太正式，但您应该与您的风格保持一致。
Comments should be as readable as narrative text, with proper capitalization and punctuation. In many cases, complete sentences are more readable than sentence fragments. Shorter comments, such as comments at the end of a line of code, can sometimes be less formal, but you should be consistent with your style.

尽管代码审阅者指出您在应该使用分号时使用了逗号可能会令人沮丧，但源代码保持高度的清晰度和可读性非常重要。正确的标点符号、拼写和语法有助于实现这一目标。
Although it can be frustrating to have a code reviewer point out that you are using a comma when you should be using a semicolon, it is very important that source code maintain a high level of clarity and readability. Proper punctuation, spelling, and grammar help with that goal.

## TODO 注释

## TODO Comments

对临时代码、短期解决方案或足够好但不完美的代码使用 `TODO` 注释。
Use `TODO` comments for code that is temporary, a short-term solution, or good-enough but not perfect.

`TODO` 应包含全部大写的字符串 `TODO` ，后跟错误 ID、名称、电子邮件地址或人员或问题的其他标识符，以及有关 `TODO` 所引用问题的最佳上下文。
`TODO`s should include the string `TODO` in all caps, followed by the bug ID, name, e-mail address, or other identifier of the person or issue with the best context about the problem referenced by the `TODO`.

```C++
// TODO: bug 12345678 - Remove this after the 2047q4 compatibility window expires.
// TODO: example.com/my-design-doc - Manually fix up this code the next time it's touched.
// TODO(bug 12345678): Update this list after the Foo service is turned down.
// TODO(John): Use a "\*" here for concatenation operator.
```

如果您的 `TODO` 的形式是 "在将来的某个日期做某事" ，请确保您包含一个非常具体的日期（ "2005 年 11 月之前修复" ）或一个非常具体的事件（ "当所有客户端都可以处理 XML 响应时删除此代码。"）。
If your `TODO` is of the form "At a future date do something" make sure that you either include a very specific date ("Fix by November 2005") or a very specific event ("Remove this code when all clients can handle XML responses.").

# 格式化

# Formatting

编码风格和格式非常随意，但如果每个人都使用相同的风格，项目就会更容易遵循。个人可能并不同意格式规则的每个方面，并且某些规则可能需要一些时间来适应，但重要的是所有项目贡献者都遵循样式规则，以便他们都可以轻松阅读和理解每个人的代码。
Coding style and formatting are pretty arbitrary, but a project is much easier to follow if everyone uses the same style. Individuals may not agree with every aspect of the formatting rules, and some of the rules may take some getting used to, but it is important that all project contributors follow the style rules so that they can all read and understand everyone's code easily.

为了帮助您正确设置代码格式，我们创建了一个 [emacs 设置文件](https://raw.githubusercontent.com/google/styleguide/gh-pages/google-c-style.el)。
To help you format code correctly, we've created a [settings file for emacs](https://raw.githubusercontent.com/google/styleguide/gh-pages/google-c-style.el).

## 行长

## Line Length

代码中的每行文本长度最多应为 80 个字符。
Each line of text in your code should be at most 80 characters long.

我们认识到这条规则是有争议的，但很多现有代码已经遵守它，我们认为一致性很重要。
We recognize that this rule is controversial, but so much existing code already adheres to it, and we feel that consistency is important.

那些支持这一规则的人认为，强迫他们调整窗户大小是粗鲁的，而且没有必要再做任何事情了。有些人习惯于并排使用多个代码窗口，因此在任何情况下都没有空间来扩大窗口。人们在设置工作环境时会假设特定的最大窗户宽度，而 80 列一直是传统标准。为什么要改变它？
Those who favor this rule argue that it is rude to force them to resize their windows and there is no need for anything longer. Some folks are used to having several code windows side-by-side, and thus don't have room to widen their windows in any case. People set up their work environment assuming a particular maximum window width, and 80 columns has been the traditional standard. Why change it?

变革的支持者认为，更宽的行可以使代码更具可读性。 80 列的限制是对 20 世纪 60 年代大型机的墨守成规。现代设备具有宽屏幕，可以轻松显示更长的队伍。
Proponents of change argue that a wider line can make code more readable. The 80-column limit is an hidebound throwback to 1960s mainframes; modern equipment has wide screens that can easily show longer lines.

最多 80 个字符。
80 characters is the maximum.

一行可能超过 80 个字符，如果
A line may exceed 80 characters if it is

- 在不损害可读性、易于剪切和粘贴或自动链接的情况下无法分割的注释行 - 例如，如果一行包含示例命令或长度超过 80 个字符的字面量 URL。
- a comment line which is not feasible to split without harming readability, ease of cut and paste or auto-linking -- e.g., if a line contains an example command or a literal URL longer than 80 characters.
- 无法轻易包装为 80 列的字符串字面量。这可能是因为它包含 URI 或其他语义关键部分，或者因为字面量包含嵌入式语言，或多行字面量，其换行符像帮助消息一样重要。在这些情况下，分解字面量会降低可读性、可搜索性、单击链接的能力等。除了测试代码之外，此类字面量应出现在文件顶部附近的命名空间范围内。如果像 Clang-Format 这样的工具无法识别不可分割的内容，请 [禁用该工具](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#disabling-formatting-on-a-piece-of-code) 根据需要围绕内容。
- a string literal that cannot easily be wrapped at 80 columns. This may be because it contains URIs or other semantically-critical pieces, or because the literal contains an embedded language, or a multiline literal whose newlines are significant like help messages. In these cases, breaking up the literal would reduce readability, searchability, ability to click links, etc. Except for test code, such literals should appear at namespace scope near the top of a file. If a tool like Clang-Format doesn't recognize the unsplittable content, [disable the tool](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#disabling-formatting-on-a-piece-of-code) around the content as necessary.

  （我们必须在这些字面量的可用性/可搜索性和它们周围的代码的可读性之间取得平衡。）
  (We must balance between usability/searchability of such literals and the readability of the code around them.)

- 包含语句。
- an include statement.
- _头文件保护_
- a _header guard_
- 使用声明
- a using-declaration

## 非 ASCII 字符

## Non-ASCII Characters

非 ASCII 字符应该很少出现，并且必须使用 UTF-8 格式。
Non-ASCII characters should be rare, and must use UTF-8 formatting.

您不应该在源代码中对面向用户的文本进行硬编码，即使是英语，因此应该很少使用非 ASCII 字符。但是，在某些情况下，在代码中包含此类单词是合适的。例如，如果您的代码解析来自外部源的数据文件，则可能适合将这些数据文件中使用的非 ASCII 字符串硬编码为分隔符。更常见的是，单元测试代码（不需要本地化）可能包含非 ASCII 字符串。在这种情况下，您应该使用 UTF-8，因为这是大多数能够处理 ASCII 以外的工具所理解的编码。
You shouldn't hard-code user-facing text in source, even English, so use of non-ASCII characters should be rare. However, in certain cases it is appropriate to include such words in your code. For example, if your code parses data files from foreign sources, it may be appropriate to hard-code the non-ASCII string(s) used in those data files as delimiters. More commonly, unittest code (which does not need to be localized) might contain non-ASCII strings. In such cases, you should use UTF-8, since that is an encoding understood by most tools able to handle more than just ASCII.

十六进制编码也可以，并且在增强可读性的地方受到鼓励 - 例如，`"\xEF\xBB\xBF"`，或者更简单的是，`"\uFEFF"`，是 Unicode 零宽度不间断空格字符，如果以纯 UTF-8 形式包含在源中，该字符将不可见。
Hex encoding is also OK, and encouraged where it enhances readability — for example, `"\xEF\xBB\xBF"`, or, even more simply, `"\uFEFF"`, is the Unicode zero-width no-break space character, which would be invisible if included in the source as straight UTF-8.

如果可能，请避免使用 `u8` 前缀。从 C++20 开始，它的语义与 C++17 中的语义显着不同，生成 `char8_t` 数组而不是 `char` 数组。
When possible, avoid the `u8` prefix. It has significantly different semantics starting in C++20 than in C++17, producing arrays of `char8_t` rather than `char`.

您不应使用 `char16_t` 和 `char32_t` 字符类型，因为它们适用于非 UTF-8 文本。出于类似的原因，您也不应该使用 `wchar_t` （除非您正在编写与 Windows API 交互的代码，该 API 广泛使用 `wchar_t` ）。
You shouldn't use `char16_t` and `char32_t` character types, since they're for non-UTF-8 text. For similar reasons you also shouldn't use `wchar_t` (unless you're writing code that interacts with the Windows API, which uses `wchar_t` extensively).

## 空格与制表符

## Spaces vs. Tabs

仅使用空格，并且一次缩进 2 个空格。
Use only spaces, and indent 2 spaces at a time.

我们使用空格进行缩进。不要在代码中使用制表符。您应该将编辑器设置为在按 Tab 键时发出空格。
We use spaces for indentation. Do not use tabs in your code. You should set your editor to emit spaces when you hit the tab key.

## 函数声明和定义

## Function Declarations and Definitions

返回类型与函数名称在同一行，参数在同一行（如果合适）。包装那些不适合放在一行上的参数列表，就像您将参数包装在*函数调用*中一样。
Return type on the same line as function name, parameters on the same line if they fit. Wrap parameter lists which do not fit on a single line as you would wrap arguments in a _function call_.

函数如下所示：
Functions look like this:

```C++
ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
  DoSomething();
  ...
}
```

如果您的文本太多而无法在一行中容纳：
If you have too much text to fit on one line:

```C++
ReturnType ClassName::ReallyLongFunctionName(Type par_name1, Type par_name2,
                                             Type par_name3) {
  DoSomething();
  ...
}
```

或者如果您甚至无法容纳第一个参数：
or if you cannot fit even the first parameter:

```C++
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
    Type par_name1,  // 4 space indent
    Type par_name2,
    Type par_name3) {
  DoSomething();  // 2 space indent
  ...
}
```

需要注意的几点：
Some points to note:

- 选择好的参数名称。
- Choose good parameter names.
- 仅当函数定义中未使用参数时，才可以省略参数名称。
- A parameter name may be omitted only if the parameter is not used in the function's definition.
- 如果无法将返回类型和函数名称放在一行中，请在它们之间换行。
- If you cannot fit the return type and the function name on a single line, break between them.
- 如果在函数声明或定义的返回类型之后中断，请勿缩进。
- If you break after the return type of a function declaration or definition, do not indent.
- 左括号始终与函数名称在同一行。
- The open parenthesis is always on the same line as the function name.
- 函数名称和左括号之间绝不能有空格。
- There is never a space between the function name and the open parenthesis.
- 括号和参数之间决不能有空格。
- There is never a space between the parentheses and the parameters.
- 左大括号始终位于函数声明的最后一行的末尾，而不是下一行的开头。
- The open curly brace is always on the end of the last line of the function declaration, not the start of the next line.
- 右花括号单独位于最后一行，或者与左花括号位于同一行。
- The close curly brace is either on the last line by itself or on the same line as the open curly brace.
- 右括号和左花括号之间应该有一个空格。
- There should be a space between the close parenthesis and the open curly brace.
- 如果可能的话，所有参数应对齐。
- All parameters should be aligned if possible.
- 默认缩进为 2 个空格。
- Default indentation is 2 spaces.
- 包裹参数有 4 个空格缩进。
- Wrapped parameters have a 4 space indent.

从上下文中显而易见的未使用的参数可以被省略：
Unused parameters that are obvious from context may be omitted:

```C++
class Foo {
 public:
  Foo(const Foo&) = delete;
  Foo& operator=(const Foo&) = delete;
};
```

可能不明显的未使用参数应注释掉函数定义中的变量名称：
Unused parameters that might not be obvious should comment out the variable name in the function definition:

```C++
class Shape {
 public:
  virtual void Rotate(double radians) = 0;
};

class Circle : public Shape {
 public:
  void Rotate(double radians) override;
};

void Circle::Rotate(double /*radians*/) {}
```

**badcode**

```C++
// Bad - if someone wants to implement later, it's not clear what the
// variable means.
void Circle::Rotate(double) {}
```

属性和扩展为属性的宏出现在函数声明或定义的最开头，返回类型之前：
Attributes, and macros that expand to attributes, appear at the very beginning of the function declaration or definition, before the return type:

```C++
  ABSL_ATTRIBUTE_NOINLINE void ExpensiveFunction();
  [[nodiscard]] bool IsOk();
```

## Lambda 表达式

## Lambda Expressions

像任何其他函数一样格式化参数和主体，并像其他逗号分隔列表一样捕获列表。
Format parameters and bodies as for any other function, and capture lists like other comma-separated lists.

对于按引用捕获，请勿在与号 (`&`) 和变量名称之间留有空格。
For by-reference captures, do not leave a space between the ampersand (`&`) and the variable name.

```C++
int x = 0;
auto x_plus_n = [&x](int n) -> int { return x + n; }
```

短 lambda 可以内联编写为函数参数。
Short lambdas may be written inline as function arguments.

```C++
absl::flat_hash_set<int> to_remove = {7, 8, 9};
std::vector<int> digits = {3, 9, 1, 8, 4, 7, 1};
digits.erase(std::remove_if(digits.begin(), digits.end(), [&to_remove](int i) {
               return to_remove.contains(i);
             }),
             digits.end());
```

## 浮点字面量

## Floating-point Literals

浮点字面量应该始终有一个小数点，两边都有数字，即使它们使用指数表示法。如果所有浮点字面量都采用这种熟悉的形式，则可读性会得到提高，因为这有助于确保它们不会被误认为整数字面量，并且指数表示法的 `E` / `e` 不会被误认为十六进制数字。可以使用整数字面量来初始化浮点变量（假设变量类型可以精确表示该整数），但请注意，指数表示法中的数字永远不是整数字面量。
Floating-point literals should always have a radix point, with digits on both sides, even if they use exponential notation. Readability is improved if all floating-point literals take this familiar form, as this helps ensure that they are not mistaken for integer literals, and that the `E`/`e` of the exponential notation is not mistaken for a hexadecimal digit. It is fine to initialize a floating-point variable with an integer literal (assuming the variable type can exactly represent that integer), but note that a number in exponential notation is never an integer literal.

**badcode**

```C++
float f = 1.f;
long double ld = -.5L;
double d = 1248e6;
```

**goodcode**

```C++
float f = 1.0f;
float f2 = 1;   // Also OK
long double ld = -0.5L;
double d = 1248.0e6;
```

## 函数调用

## Function Calls

将调用全部写在一行上，将参数括在括号中，或者在新行上开始参数，缩进 4 个空格，并在 4 个空格缩进处继续。在没有其他考虑的情况下，请使用最少的行数，包括在适当的情况下在每行上放置多个参数。
Either write the call all on a single line, wrap the arguments at the parenthesis, or start the arguments on a new line indented by four spaces and continue at that 4 space indent. In the absence of other considerations, use the minimum number of lines, including placing multiple arguments on each line where appropriate.

函数调用具有以下格式：
Function calls have the following format:

```C++
bool result = DoSomething(argument1, argument2, argument3);
```

如果参数不能全部放在一行上，则应将它们分成多行，每个后续行与第一个参数对齐。不要在左括号之后或右括号之前添加空格：
If the arguments do not all fit on one line, they should be broken up onto multiple lines, with each subsequent line aligned with the first argument. Do not add spaces after the open paren or before the close paren:

```C++
bool result = DoSomething(averyveryveryverylongargument1,
                          argument2, argument3);
```

参数可以选择全部放置在后续行中，并缩进四个空格：
Arguments may optionally all be placed on subsequent lines with a four space indent:

```C++
if (...) {
  ...
  ...
  if (...) {
    bool result = DoSomething(
        argument1, argument2,  // 4 space indent
        argument3, argument4);
    ...
  }
```

将多个参数放在一行上以减少调用函数所需的行数，除非存在特定的可读性问题。有些人发现，在每一行上严格使用一个参数进行格式化更具可读性，并且简化了参数的编辑。然而，我们优先考虑的是读者编辑参数的便利性，并且大多数可读性问题可以通过以下技术更好地解决。
Put multiple arguments on a single line to reduce the number of lines necessary for calling a function unless there is a specific readability problem. Some find that formatting with strictly one argument on each line is more readable and simplifies editing of the arguments. However, we prioritize for the reader over the ease of editing arguments, and most readability problems are better addressed with the following techniques.

如果由于构成某些参数的表达式的复杂性或令人困惑的性质，在一行中包含多个参数会降低可读性，请尝试创建以描述性名称捕获这些参数的变量：
If having multiple arguments in a single line decreases readability due to the complexity or confusing nature of the expressions that make up some arguments, try creating variables that capture those arguments in a descriptive name:

```C++
int my_heuristic = scores[x] * y + bases[x];
bool result = DoSomething(my_heuristic, x, y, z);
```

或者将令人困惑的论点单独放在一行并附上解释性注释：
Or put the confusing argument on its own line with an explanatory comment:

```C++
bool result = DoSomething(scores[x] * y + bases[x],  // Score heuristic.
                          x, y, z);
```

如果仍然存在一个参数在其自己的行中明显更具可读性的情况，则将其放在自己的行中。该决定应该针对更具可读性的论点，而不是一般政策。
If there is still a case where one argument is significantly more readable on its own line, then put it on its own line. The decision should be specific to the argument which is made more readable rather than a general policy.

有时参数形成的结构对于可读性很重要。在这些情况下，请随意根据该结构格式化参数：
Sometimes arguments form a structure that is important for readability. In those cases, feel free to format the arguments according to that structure:

```C++
// Transform the widget by a 3x3 matrix.
my_widget.Transform(x1, x2, x3,
                    y1, y2, y3,
                    z1, z2, z3);
```

## 大括号初始化列表格式

## Braced Initializer List Format

格式化大括号初始值设定项列表的格式与格式化函数调用的位置完全相同。
Format a braced initializer list exactly like you would format a function call in its place.

如果大括号列表后面有一个名称（例如，类型或变量名称），则格式就像 `{}` 是使用该名称的函数调用的括号一样。如果没有名称，则假定名称长度为零。
If the braced list follows a name (e.g., a type or variable name), format as if the `{}` were the parentheses of a function call with that name. If there is no name, assume a zero-length name.

```C++
// Examples of braced init list on a single line.
return {foo, bar};
functioncall({foo, bar});
std::pair<int, int> p{foo, bar};

// When you have to wrap.
SomeFunction(
    {"assume a zero-length name before {"},
    some_other_function_parameter);
SomeType variable{
    some, other, values,
    {"assume a zero-length name before {"},
    SomeOtherType{
        "Very long string requiring the surrounding breaks.",
        some, other, values},
    SomeOtherType{"Slightly shorter string",
                  some, other, values}};
SomeType variable{
    "This is too long to fit all in one line"};
MyType m = {  // Here, you could also break before {.
    superlongvariablename1,
    superlongvariablename2,
    {short, interior, list},
    {interiorwrappinglist,
     interiorwrappinglist2}};
```

## 循环和分支语句

## Looping and branching statements

在较高层次上，循环或分支语句由以下**组件**组成：
At a high level, looping or branching statements consist of the following **components**:

- 一个或多个**语句关键字**（例如 `if` 、 `else` 、 `switch` 、 `while` 、 `do` 或 `for` ）。
- One or more **statement keywords** (e.g. `if`, `else`, `switch`, `while`, `do`, or `for`).
- 一个**条件或迭代说明符**，位于括号内。
- One **condition or iteration specifier**, inside parentheses.
- 一个或多个**受控语句**，或受控语句块。
- One or more **controlled statements**, or blocks of controlled statements.

对于这些陈述：
For these statements:

- 语句的各个组成部分应该用单个空格（而不是换行符）分隔。
- The components of the statement should be separated by single spaces (not line breaks).
- 在条件或迭代说明符内，在每个分号和下一个标记之间放置一个空格（或换行符），除非该标记是右括号或另一个分号。
- Inside the condition or iteration specifier, put one space (or a line break) between each semicolon and the next token, except if the token is a closing parenthesis or another semicolon.
- 在条件或迭代说明符内，请勿在左括号之后或右括号之前放置空格。
- Inside the condition or iteration specifier, do not put a space after the opening parenthesis or before the closing parenthesis.
- 将任何受控语句放入块内（即使用花括号）。
- Put any controlled statements inside blocks (i.e. use curly braces).
- 在受控块内，在紧接左大括号之后放置一个换行符，并在紧邻右大括号之前放置一个换行符。
- Inside the controlled blocks, put one line break immediately after the opening brace, and one line break immediately before the closing brace.

```C++
if (condition) {                   // Good - no spaces inside parentheses, space before brace.
  DoOneThing();                    // Good - two-space indent.
  DoAnotherThing();
} else if (int a = f(); a != 3) {  // Good - closing brace on new line, else on same line.
  DoAThirdThing(a);
} else {
  DoNothing();
}

// Good - the same rules apply to loops.
while (condition) {
  RepeatAThing();
}

// Good - the same rules apply to loops.
do {
  RepeatAThing();
} while (condition);

// Good - the same rules apply to loops.
for (int i = 0; i < 10; ++i) {
  RepeatAThing();
}
```

**badcode**

```C++
if(condition) {}                   // Bad - space missing after `if`.
else if ( condition ) {}           // Bad - space between the parentheses and the condition.
else if (condition){}              // Bad - space missing before `{`.
else if(condition){}               // Bad - multiple spaces missing.

for (int a = f();a == 10) {}       // Bad - space missing after the semicolon.

// Bad - `if ... else` statement does not have braces everywhere.
if (condition)
  foo;
else {
  bar;
}

// Bad - `if` statement too long to omit braces.
if (condition)
  // Comment
  DoSomething();

// Bad - `if` statement too long to omit braces.
if (condition1 &&
    condition2)
  DoSomething();
```

由于历史原因，我们允许上述规则有一个例外：受控语句的大括号或大括号内的换行符可以被省略，如果结果是整个语句出现在单行上（在这种情况下，右括号和受控语句之间的空格）或两行（在这种情况下，右括号后面有一个换行符，并且没有大括号）。
For historical reasons, we allow one exception to the above rules: the curly braces for the controlled statement or the line breaks inside the curly braces may be omitted if as a result the entire statement appears on either a single line (in which case there is a space between the closing parenthesis and the controlled statement) or on two lines (in which case there is a line break after the closing parenthesis and there are no braces).

**neutralcode**

```C++
// OK - fits on one line.
if (x == kFoo) { return new Foo(); }

// OK - braces are optional in this case.
if (x == kFoo) return new Foo();

// OK - condition fits on one line, body fits on another.
if (x == kBar)
  Bar(arg1, arg2, arg3);
```

此例外不适用于多关键字语句，例如 `if ... else` 或 `do ... while` 。
This exception does not apply to multi-keyword statements like `if ... else` or `do ... while`.

**badcode**

```C++
// Bad - `if ... else` statement is missing braces.
if (x) DoThis();
else DoThat();

// Bad - `do ... while` statement is missing braces.
do DoThis();
while (x);
```

仅当语句简短时才使用此样式，并考虑到具有复杂条件的循环和分支语句或受控语句使用花括号可能会更具可读性。有些项目始终需要花括号。
Use this style only when the statement is brief, and consider that loops and branching statements with complex conditions or controlled statements may be more readable with curly braces. Some projects require curly braces always.

`switch` 语句中的 `case` 块可以有花括号，也可以没有，具体取决于您的偏好。如果您确实包含花括号，则应如下所示放置它们。
`case` blocks in `switch` statements can have curly braces or not, depending on your preference. If you do include curly braces, they should be placed as shown below.

```C++
switch (var) {
  case 0: {  // 2 space indent
    Foo();   // 4 space indent
    break;
  }
  default: {
    Bar();
  }
}
```

空循环体应该使用一对空大括号或不带大括号的 `continue` ，而不是单个分号。
Empty loop bodies should use either an empty pair of braces or `continue` with no braces, rather than a single semicolon.

```C++
while (condition) {}  // Good - `{}` indicates no logic.
while (condition) {
  // Comments are okay, too
}
while (condition) continue;  // Good - `continue` indicates no logic.
```

**badcode**

```C++
while (condition);  // Bad - looks like part of `do-while` loop.
```

## 指针和引用表达式

## Pointer and Reference Expressions

句点或箭头周围没有空格。指针运算符没有尾随空格。
No spaces around period or arrow. Pointer operators do not have trailing spaces.

以下是格式正确的指针和引用表达式的示例：
The following are examples of correctly-formatted pointer and reference expressions:

```C++
x = *p;
p = &x;
x = r.y;
x = r->y;
```

注意：
Note that:

- 访问成员时，句点或箭头周围没有空格。
- There are no spaces around the period or arrow when accessing a member.
- 指针运算符在 `*` 或 `&` 之后没有空格。
- Pointer operators have no space after the `*` or `&`.

当引用指针或引用（变量声明或定义、参数、返回类型、模板参数等）时，可以在星号/与号之前或之后放置空格。在尾随空格样式中，在某些情况下（模板参数等）会省略空格。
When referring to a pointer or reference (variable declarations or definitions, arguments, return types, template parameters, etc), you may place the space before or after the asterisk/ampersand. In the trailing-space style, the space is elided in some cases (template parameters, etc).

```C++
// These are fine, space preceding.
char *c;
const std::string &str;
int *GetPointer();
std::vector<char *>

// These are fine, space following (or elided).
char* c;
const std::string& str;
int* GetPointer();
std::vector<char*>  // Note no space between '*' and '>'
```

您应该在单个文件中一致地执行此操作。修改现有文件时，请使用该文件中的样式。
You should do this consistently within a single file. When modifying an existing file, use the style in that file.

允许（如果不寻常）在同一个声明中声明多个变量，但如果其中任何一个变量具有指针或引用修饰，则不允许这样做。此类声明很容易被误读。
It is allowed (if unusual) to declare multiple variables in the same declaration, but it is disallowed if any of those have pointer or reference decorations. Such declarations are easily misread.

```C++
// Fine if helpful for readability.
int x, y;
```

**badcode**

```C++
int x, *y;  // Disallowed - no & or * in multiple declaration
int* x, *y;  // Disallowed - no & or * in multiple declaration; inconsistent spacing
char * c;  // Bad - spaces on both sides of *
const std::string & str;  // Bad - spaces on both sides of &
```

## 布尔表达式

## Boolean Expressions

当您的布尔表达式比*标准行长度*长时，请在拆分行时保持一致。
When you have a boolean expression that is longer than the _standard line length_, be consistent in how you break up the lines.

在此示例中，逻辑 AND 运算符始终位于行的末尾：
In this example, the logical AND operator is always at the end of the lines:

```C++
if (this_one_thing > this_other_thing &&
    a_third_thing == a_fourth_thing &&
    yet_another && last_one) {
  ...
}
```

请注意，当代码在此示例中换行时，两个 `&&` 逻辑 AND 运算符都位于行尾。这在 Google 代码中更常见，尽管也允许将所有运算符包装在行的开头。请随意明智地插入额外的括号，因为如果使用得当，它们对提高可读性非常有帮助，但要小心过度使用。另请注意，您应该始终使用标点符号运算符，例如 `&&` 和 `~` ，而不是单词运算符，例如 `and` 和 `compl` 。
Note that when the code wraps in this example, both of the `&&` logical AND operators are at the end of the line. This is more common in Google code, though wrapping all operators at the beginning of the line is also allowed. Feel free to insert extra parentheses judiciously because they can be very helpful in increasing readability when used appropriately, but be careful about overuse. Also note that you should always use the punctuation operators, such as `&&` and `~`, rather than the word operators, such as `and` and `compl`.

## 返回值

## Return Values

不要不必要地将 `return` 表达式用括号括起来。
Do not needlessly surround the `return` expression with parentheses.

仅当在 `x = expr;` 中使用括号时，才在 `return expr;` 中使用括号。
Use parentheses in `return expr;` only where you would use them in `x = expr;`.

```C++
return result;                  // No parentheses in the simple case.
// Parentheses OK to make a complex expression more readable.
return (some_long_condition &&
        another_condition);
```

**badcode**

```C++
return (value);                // You wouldn't write var = (value);
return(result);                // return is not a function!
```

## 变量和数组初始化

## Variable and Array Initialization

您可以在 `=` 、 `()` 和 `{}` 之间进行选择；以下都是正确的：
You may choose between `=`, `()`, and `{}`; the following are all correct:

```C++
int x = 3;
int x(3);
int x{3};
std::string name = "Some Name";
std::string name("Some Name");
std::string name{"Some Name"};
```

在带有 `std::initializer_list` 构造函数的类型上使用大括号初始化列表 `{...}` 时要小心。只要可能，非空 _大括号初始化列表_ 更喜欢 `std::initializer_list` 构造函数。请注意，空大括号 `{}` 是特殊的，如果可用，它将调用默认构造函数。要强制使用非`std::initializer_list` 构造函数，请使用括号而不是大括号。
Be careful when using a braced initialization list `{...}` on a type with an `std::initializer_list` constructor. A nonempty _braced-init-list_ prefers the `std::initializer_list` constructor whenever possible. Note that empty braces `{}` are special, and will call a default constructor if available. To force the non-`std::initializer_list` constructor, use parentheses instead of braces.

```C++
std::vector<int> v(100, 1);  // A vector containing 100 items: All 1s.
std::vector<int> v{100, 1};  // A vector containing 2 items: 100 and 1.
```

此外，大括号形式可以防止整数类型变窄。这可以防止某些类型的编程错误。
Also, the brace form prevents narrowing of integral types. This can prevent some types of programming errors.

```C++
int pi(3.14);  // OK -- pi == 3.
int pi{3.14};  // Compile error: narrowing conversion.
```

## 预处理器指令

## Preprocessor Directives

启动预处理器指令的哈希标记应始终位于行的开头。
The hash mark that starts a preprocessor directive should always be at the beginning of the line.

即使预处理器指令位于缩进代码体内，这些指令也应从行首开始。
Even when preprocessor directives are within the body of indented code, the directives should start at the beginning of the line.

```C++
// Good - directives at beginning of line
  if (lopsided_score) {
#if DISASTER_PENDING      // Correct -- Starts at beginning of line
    DropEverything();
# if NOTIFY               // OK but not required -- Spaces after #
    NotifyClient();
# endif
#endif
    BackToNormal();
  }
```

**badcode**

```C++
// Bad - indented directives
  if (lopsided_score) {
    #if DISASTER_PENDING  // Wrong!  The "#if" should be at beginning of line
    DropEverything();
    #endif                // Wrong!  Do not indent "#endif"
    BackToNormal();
  }
```

## 类格式

## Class Format

`public` 、 `protected` 和 `private` 顺序中的部分，每个部分缩进一个空格。
Sections in `public`, `protected` and `private` order, each indented one space.

类定义的基本格式（缺少注释，请参阅*类注释*以讨论需要哪些注释）：
The basic format for a class definition (lacking the comments, see _Class Comments_ for a discussion of what comments are needed) is:

```C++
class MyClass : public OtherClass {
 public:      // Note the 1 space indent!
  MyClass();  // Regular 2 space indent.
  explicit MyClass(int var);
  ~MyClass() {}

  void SomeFunction();
  void SomeFunctionThatDoesNothing() {
  }

  void set_some_var(int var) { some_var_ = var; }
  int some_var() const { return some_var_; }

 private:
  bool SomeInternalFunction();

  int some_var_;
  int some_other_var_;
};
```

注意事项：
Things to note:

- 任何基类名称应与子类名称在同一行，但须遵守 80 列的限制。
- Any base class name should be on the same line as the subclass name, subject to the 80-column limit.
- `public:`、`protected:` 和 `private:` 关键字应缩进一个空格。
- The `public:`, `protected:`, and `private:` keywords should be indented one space.
- 除第一个实例外，这些关键字前面应有一个空行。在小班中，此规则是可选的。
- Except for the first instance, these keywords should be preceded by a blank line. This rule is optional in small classes.
- 不要在这些关键字后留空行。
- Do not leave a blank line after these keywords.
- `public` 部分应该放在第一位，然后是 `protected` 部分，最后是 `private` 部分。
- The `public` section should be first, followed by the `protected` and finally the `private` section.
- 有关每个部分中声明排序的规则，请参阅*声明顺序*。
- See _Declaration Order_ for rules on ordering declarations within each of these sections.

## 构造函数初始化列表

## Constructor Initializer Lists

构造函数初始值设定项列表可以全部在一行中，也可以在后续行中缩进四个空格。
Constructor initializer lists can be all on one line or with subsequent lines indented four spaces.

可接受的初始值设定项列表格式为：
The acceptable formats for initializer lists are:

```C++
// When everything fits on one line:
MyClass::MyClass(int var) : some_var_(var) {
  DoSomething();
}

// If the signature and initializer list are not all on one line,
// you must wrap before the colon and indent 4 spaces:
MyClass::MyClass(int var)
    : some_var_(var), some_other_var_(var + 1) {
  DoSomething();
}

// When the list spans multiple lines, put each member on its own line
// and align them:
MyClass::MyClass(int var)
    : some_var_(var),             // 4 space indent
      some_other_var_(var + 1) {  // lined up
  DoSomething();
}

// As with any other code block, the close curly can be on the same
// line as the open curly, if it fits.
MyClass::MyClass(int var)
    : some_var_(var) {}
```

## 命名空间格式化

## Namespace Formatting

命名空间的内容不缩进。
The contents of namespaces are not indented.

*命名空间*不添加额外的缩进级别。例如，使用：
_Namespaces_ do not add an extra level of indentation. For example, use:

```C++
namespace {

void foo() {  // Correct.  No extra indentation within namespace.
  ...
}

}  // namespace
```

不要在命名空间内缩进：
Do not indent within a namespace:

**badcode**

```C++
namespace {

  // Wrong!  Indented when it should not be.
  void foo() {
    ...
  }

}  // namespace
```

## 水平空白

## Horizontal Whitespace

水平空白的使用取决于位置。切勿在行尾放置尾随空格。
Use of horizontal whitespace depends on location. Never put trailing whitespace at the end of a line.

### 一般的

### General

```C++
int i = 0;  // Two spaces before end-of-line comments.

void f(bool b) {  // Open braces should always have a space before them.
  ...
int i = 0;  // Semicolons usually have no space before them.
// Spaces inside braces for braced-init-list are optional.  If you use them,
// put them on both sides!
int x[] = { 0 };
int x[] = {0};

// Spaces around the colon in inheritance and initializer lists.
class Foo : public Bar {
 public:
  // For inline function implementations, put spaces between the braces
  // and the implementation itself.
  Foo(int b) : Bar(), baz_(b) {}  // No spaces inside empty braces.
  void Reset() { baz_ = 0; }  // Spaces separating braces from implementation.
  ...
```

添加尾随空格可能会给编辑同一文件的其他人带来额外的工作，当它们合并时，删除现有的尾随空格也会造成额外的工作。所以：不要引入尾随空格。如果您已经更改了该行，请将其删除，或者在单独的清理操作中执行此操作（最好是在没有其他人处理该文件时）。
Adding trailing whitespace can cause extra work for others editing the same file, when they merge, as can removing existing trailing whitespace. So: Don't introduce trailing whitespace. Remove it if you're already changing that line, or do it in a separate clean-up operation (preferably when no-one else is working on the file).

### 循环和条件

### Loops and Conditionals

```C++
if (b) {          // Space after the keyword in conditions and loops.
} else {          // Spaces around else.
}
while (test) {}   // There is usually no space inside parentheses.
switch (i) {
for (int i = 0; i < 5; ++i) {
// Loops and conditions may have spaces inside parentheses, but this
// is rare.  Be consistent.
switch ( i ) {
if ( test ) {
for ( int i = 0; i < 5; ++i ) {
// For loops always have a space after the semicolon.  They may have a space
// before the semicolon, but this is rare.
for ( ; i < 5 ; ++i) {
  ...

// Range-based for loops always have a space before and after the colon.
for (auto x : counts) {
  ...
}
switch (i) {
  case 1:         // No space before colon in a switch case.
    ...
  case 2: break;  // Use a space after a colon if there's code after it.
```

### 运算符

### Operators

```C++
// Assignment operators always have spaces around them.
x = 0;

// Other binary operators usually have spaces around them, but it's
// OK to remove spaces around factors.  Parentheses should have no
// internal padding.
v = w * x + y / z;
v = w*x + y/z;
v = w * (x + z);

// No spaces separating unary operators and their arguments.
x = -5;
++x;
if (x && !y)
  ...
```

### 模板和转换

### Templates and Casts

```C++
// No spaces inside the angle brackets (< and >), before
// <, or between >( in a cast
std::vector<std::string> x;
y = static_cast<char*>(x);

// Spaces between type and pointer are OK, but be consistent.
std::vector<char *> x;
```

## 垂直空白

## Vertical Whitespace

尽量减少垂直空白的使用。
Minimize use of vertical whitespace.

这与其说是一条规则，不如说是一条原则：不必要时不要使用空行。特别是，函数之间不要放置超过一两个空行，不要以空行开始函数，不要以空行结束函数，并且要少用空行。代码块中的空行就像散文中的段落分隔符：在视觉上分隔两个想法。
This is more a principle than a rule: don't use blank lines when you don't have to. In particular, don't put more than one or two blank lines between functions, resist starting functions with a blank line, don't end functions with a blank line, and be sparing with your use of blank lines. A blank line within a block of code serves like a paragraph break in prose: visually separating two thoughts.

基本原则是：一个屏幕上显示的代码越多，就越容易遵循和理解程序的控制流程。有目的地使用空格来在该流程中提供分隔。
The basic principle is: The more code that fits on one screen, the easier it is to follow and understand the control flow of the program. Use whitespace purposefully to provide separation in that flow.

当空行可能有用时，一些经验法则可以提供帮助：
Some rules of thumb to help when blank lines may be useful:

- 函数开头或结尾的空行无助于可读性。
- Blank lines at the beginning or end of a function do not help readability.
- if-else 块链内的空行可能有助于提高可读性。
- Blank lines inside a chain of if-else blocks may well help readability.
- 注释行之前的空行通常有助于提高可读性——新注释的引入表明新想法的开始，并且空行清楚地表明注释与后面的内容而不是前面的内容相关。
- A blank line before a comment line usually helps readability — the introduction of a new comment suggests the start of a new thought, and the blank line makes it clear that the comment goes with the following thing instead of the preceding.
- 命名空间或命名空间块的声明内紧邻的空行可以通过在视觉上将承载内容与（主要是非语义的）组织包装器分开来提高可读性。特别是当命名空间内的第一个声明前面有注释时，这成为前一个规则的特例，帮助注释 "附加" 到后续声明。
- Blank lines immediately inside a declaration of a namespace or block of namespaces may help readability by visually separating the load-bearing content from the (largely non-semantic) organizational wrapper. Especially when the first declaration inside the namespace(s) is preceded by a comment, this becomes a special case of the previous rule, helping the comment to "attach" to the subsequent declaration.

# 规则的例外情况

# Exceptions to the Rules

上述编码约定是强制性的。然而，就像所有好的规则一样，这些规则有时也有例外，我们在这里讨论。
The coding conventions described above are mandatory. However, like all good rules, these sometimes have exceptions, which we discuss here.

## 现有的不合格代码

## Existing Non-conformant Code

在处理不符合本风格指南的代码时，您可能会偏离规则。
You may diverge from the rules when dealing with code that does not conform to this style guide.

如果您发现自己修改的代码不是按照本指南提供的规范编写的，那么您可能必须偏离这些规则，以便与该代码中的本地约定保持一致。如果您对如何执行此操作有疑问，请询问原作者或当前负责代码的人员。请记住，*一致性*也包括本地一致性。
If you find yourself modifying code that was written to specifications other than those presented by this guide, you may have to diverge from these rules in order to stay consistent with the local conventions in that code. If you are in doubt about how to do this, ask the original author or the person currently responsible for the code. Remember that _consistency_ includes local consistency, too.

## Windows 代码

## Windows Code

Windows 程序员开发了自己的一套编码约定，主要源自 Windows 标头和其他 Microsoft 代码中的约定。我们希望让任何人都能轻松理解您的代码，因此我们为在任何平台上编写 C++ 的每个人提供了一套指南。
Windows programmers have developed their own set of coding conventions, mainly derived from the conventions in Windows headers and other Microsoft code. We want to make it easy for anyone to understand your code, so we have a single set of guidelines for everyone writing C++ on any platform.

如果您习惯了流行的 Windows 风格，那么值得重申一下您可能会忘记的一些准则：
It is worth reiterating a few of the guidelines that you might forget if you are used to the prevalent Windows style:

- 不要使用匈牙利表示法（例如，命名整数 `iNum` ）。使用 Google 命名约定，包括源文件的 `.cc` 扩展名。
- Do not use Hungarian notation (for example, naming an integer `iNum`). Use the Google naming conventions, including the `.cc` extension for source files.
- Windows 为原始类型定义了许多自己的同义词，例如 `DWORD` 、 `HANDLE` 等。在调用 Windows API 函数时使用这些类型是完全可以接受和鼓励的。即便如此，请尽可能接近底层的 C++ 类型。例如，使用 `const TCHAR *` 而不是 `LPCTSTR` 。
- Windows defines many of its own synonyms for primitive types, such as `DWORD`, `HANDLE`, etc. It is perfectly acceptable, and encouraged, that you use these types when calling Windows API functions. Even so, keep as close as you can to the underlying C++ types. For example, use `const TCHAR *` instead of `LPCTSTR`.
- 使用 Microsoft Visual C++ 进行编译时，将编译器设置为警告级别 3 或更高，并将所有警告视为错误。
- When compiling with Microsoft Visual C++, set the compiler to warning level 3 or higher, and treat all warnings as errors.
- 不要使用`#pragma Once`；相反，使用标准的 Google include 防护。包含防护中的路径应该相对于项目树的顶部。
- Do not use `#pragma once`; instead use the standard Google include guards. The path in the include guards should be relative to the top of your project tree.
- 事实上，不要使用任何非标准扩展，例如 `#pragma` 和 `__declspec` ，除非绝对必须。允许使用 `__declspec(dllimport)` 和 `__declspec(dllexport)` ；但是，您必须通过 `DLLIMPORT` 和 `DLLEXPORT` 等宏来使用它们，以便有人共享代码时可以轻松禁用扩展。
- In fact, do not use any nonstandard extensions, like `#pragma` and `__declspec`, unless you absolutely must. Using `__declspec(dllimport)` and `__declspec(dllexport)` is allowed; however, you must use them through macros such as `DLLIMPORT` and `DLLEXPORT`, so that someone can easily disable the extensions if they share the code.

然而，在 Windows 上我们偶尔需要打破一些规则：
However, there are just a few rules that we occasionally need to break on Windows:

- 通常我们*强烈不鼓励使用多重实现继承*；但是，在使用 COM 和某些 ATL/WTL 类时需要它。您可以使用多个实现继承来实现 COM 或 ATL/WTL 类和接口。
- Normally we _strongly discourage the use of multiple implementation inheritance_; however, it is required when using COM and some ATL/WTL classes. You may use multiple implementation inheritance to implement COM or ATL/WTL classes and interfaces.
- 尽管您不应在自己的代码中使用异常，但它们在 ATL 和某些 STL（包括 Visual C++ 附带的异常）中广泛使用。使用 ATL 时，您应该定义 `_ATL_NO_EXCEPTIONS` 来禁用异常。您应该研究是否也可以在 STL 中禁用异常，但如果不能，则可以在编译器中打开异常。 （请注意，这只是为了编译 STL。您仍然不应该自己编写异常处理代码。）
- Although you should not use exceptions in your own code, they are used extensively in the ATL and some STLs, including the one that comes with Visual C++. When using the ATL, you should define `_ATL_NO_EXCEPTIONS` to disable exceptions. You should investigate whether you can also disable exceptions in your STL, but if not, it is OK to turn on exceptions in the compiler. (Note that this is only to get the STL to compile. You should still not write exception handling code yourself.)
- 使用预编译头的通常方法是在每个源文件的顶部包含一个头文件，通常名称为 `StdAfx.h` 或 `precompile.h` 。为了使您的代码更容易与其他项目共享，请避免显式包含此文件（ `precompile.cc` 中除外），并使用 `/FI` 编译器选项自动包含该文件。
- The usual way of working with precompiled headers is to include a header file at the top of each source file, typically with a name like `StdAfx.h` or `precompile.h`. To make your code easier to share with other projects, avoid including this file explicitly (except in `precompile.cc`), and use the `/FI` compiler option to include the file automatically.
- 资源标头通常名为 `resource.h` 并且仅包含宏，不需要符合这些样式指南。
- Resource headers, which are usually named `resource.h` and contain only macros, do not need to conform to these style guidelines.
